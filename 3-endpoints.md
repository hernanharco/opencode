## CONTEXTO DEL PROYECTO — Leer antes de escribir cualquier línea

Lee el archivo `.windsurf/backend.md` para entender la estructura exacta
del backend existente. Ese archivo es la fuente de verdad sobre rutas,
patrones y convenciones del proyecto.

Mapa de imports válidos — NO inferir, usar exactamente estos:

| Qué necesitas importar | Desde dónde importarlo           |
|------------------------|----------------------------------|
| `get_db`               | `app.db.session`                 |
| `AsyncSession`         | `sqlalchemy.ext.asyncio`         |
| `Base`                 | `app.models.base`                |
| `Settings/config`      | `app.core.config`                |
| `upload_image`         | `app.core.cloudinary`            |

---

## CLOUDINARY — Ejecutar en este orden antes de cualquier endpoint

### 1. Agregar al archivo `.env` (sin borrar variables existentes)
```env
CLOUDINARY_CLOUD_NAME=dxyk76jhu
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
```

### 2. Actualizar `backend/app/core/config.py`

Agregar estos campos a la clase `Settings` existente sin borrar
los campos que ya están:
```python
# Cloudinary
cloudinary_cloud_name: str = Field(..., env="CLOUDINARY_CLOUD_NAME")
cloudinary_api_key: str = Field(..., env="CLOUDINARY_API_KEY")
cloudinary_api_secret: str = Field(..., env="CLOUDINARY_API_SECRET")
```

### 3. Instalar dependencia
```bash
poetry add cloudinary
```

Verificar que `cloudinary` quedó en `pyproject.toml` antes de continuar.

### 4. Crear `backend/app/core/cloudinary.py`
```python
import cloudinary
import cloudinary.uploader
from fastapi import UploadFile
from app.core.config import settings

cloudinary.config(
    cloud_name=settings.cloudinary_cloud_name,
    api_key=settings.cloudinary_api_key,
    api_secret=settings.cloudinary_api_secret,
    secure=True
)

async def upload_image(file: UploadFile) -> str:
    """
    Sube una imagen a Cloudinary y retorna la URL pública segura (https).
    Nunca retorna binario ni ruta local.
    """
    contents = await file.read()
    result = cloudinary.uploader.upload(
        contents,
        folder="elrincondelharco",
        resource_type="image"
    )
    return result["secure_url"]  # siempre str
```

---

## Contexto general

Tengo una web con varias secciones visuales. Cada sección está implementada
como uno o más componentes React en `frontend/src/components/`. Tu trabajo
es crear la capa de persistencia para que cada sección muestre datos reales
desde la base de datos.

---

## PASO 1 — Identificar dominios (obligatorio, espera confirmación)

Recorre TODOS los archivos en `frontend/src/components/` y para cada uno
identifica:

1. **¿Qué entidad de negocio representa?**
   - Un componente = una entidad si tiene datos propios y únicos
   - Varios componentes = una entidad si comparten los mismos datos
     (ej: HeroTitle.tsx + HeroButton.tsx → ambos son del dominio `hero`)
   - Ignora componentes puramente de UI sin datos (Spinner, Modal, Layout…)

2. **¿Qué campos de datos contiene?**
   - Extrae SOLO los props, estados o datos que el componente renderiza
   - Clasifica cada campo:
     - Texto corto → `String`
     - Texto largo → `Text`
     - Número → `Integer` o `Float`
     - Imagen → `String` (guardará URL de Cloudinary, nunca binario)
     - Icono → `String` (nombre del icono Lucide, ej: "Star")
     - Color → `String` (hex, ej: "#FF0000")
     - Lista u objeto anidado → `JSONB`
     - Booleano → `Boolean`

3. **¿Tiene sentido "último registro"?**
   - Sí: si la sección muestra UNA versión activa (hero, banner, about)
   - No: si la sección muestra MÚLTIPLES items (productos, testimonios, equipo)

Presenta el resultado en esta tabla y NO escribas código hasta recibir
confirmación:

| # | Dominio | Componentes que lo usan | Campos y tipos | GET latest |
|---|---------|-------------------------|----------------|------------|
| 1 | hero    | Hero.tsx, HeroButtons.tsx | title:String, subtitle:Text… | sí |
| 2 | product | ProductCard.tsx, ProductGrid.tsx | name:String, price:Float… | no |

Si tienes dudas sobre si dos componentes son el mismo dominio, pregunta
antes de continuar.

---

## PASO 2 — Estructura de archivos a crear

Por cada dominio confirmado en la tabla, crear exactamente estos 3 archivos:
```
backend/app/models/{dominio}.py
backend/app/schemas/{dominio}.py
backend/app/api/v1/endpoints/{dominio}.py
```

Ningún archivo puede importar de otro dominio. Un dominio no puede
aparecer en más de un archivo de cada tipo.

---

## PASO 3 — Modelo (`backend/app/models/{dominio}.py`)
```python
from app.models.base import Base
from sqlalchemy import Column, Integer, String, Text, Boolean, Float
from sqlalchemy.dialects.postgresql import JSONB

class {Dominio}(Base):
    __tablename__ = "{dominios}"  # plural snake_case

    id        = Column(Integer, primary_key=True, index=True)
    # un campo por cada fila confirmada en la tabla del Paso 1
    image_url = Column(String, nullable=True)  # solo si el dominio tiene imagen
```

Reglas:
- Los nombres de columna deben coincidir exactamente con los keys que
  el frontend espera recibir del API
- `image_url` siempre `nullable=True` — la imagen se sube por separado
- No agregar campos que no estén en la tabla confirmada en el Paso 1

---

## PASO 4 — Schema (`backend/app/schemas/{dominio}.py`)
```python
from pydantic import BaseModel
from typing import Optional, List

class {Dominio}Create(BaseModel):
    # todos los campos del modelo EXCEPTO id e image_url
    # image_url no va aquí porque llega como File, no como JSON

class {Dominio}Update(BaseModel):
    # los mismos campos que Create pero todos Optional[tipo] = None
    # image_url tampoco va aquí

class {Dominio}Response(BaseModel):
    id: int
    # todos los campos del modelo
    image_url: Optional[str] = None

    class Config:
        orm_mode = True
```

---

## PASO 5 — Endpoint (`backend/app/api/v1/endpoints/{dominio}.py`)

**Imports fijos en este orden — no cambiar ni mover:**
```python
from fastapi import APIRouter, Depends, HTTPException, Form, File, UploadFile
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy import select
from typing import Optional
from app.db.session import get_db
from app.core.cloudinary import upload_image
from app.models.{dominio} import {Dominio}
from app.schemas.{dominio} import {Dominio}Create, {Dominio}Update, {Dominio}Response
```

**Función de dependencia local (en el mismo archivo, antes del router):**
```python
async def get_{dominio}_form(
    # un parámetro Form(...) por cada campo de {Dominio}Create
    campo1: tipo = Form(...),
) -> {Dominio}Create:
    return {Dominio}Create(campo1=campo1)

async def get_{dominio}_update_form(
    # los mismos parámetros pero todos Optional con default None
    campo1: Optional[tipo] = Form(None),
) -> {Dominio}Update:
    return {Dominio}Update(campo1=campo1)
```

**Endpoints:**
```python
router = APIRouter()

@router.get("/", response_model=list[{Dominio}Response])
async def get_all(db: AsyncSession = Depends(get_db)):
    result = await db.execute(select({Dominio}))
    return result.scalars().all()

@router.get("/{id}", response_model={Dominio}Response)
async def get_one(id: int, db: AsyncSession = Depends(get_db)):
    obj = await db.get({Dominio}, id)
    if not obj:
        raise HTTPException(status_code=404, detail="{Dominio} no encontrado")
    return obj

# Solo si GET latest = sí en la tabla del Paso 1
@router.get("/latest/", response_model={Dominio}Response)
async def get_latest(db: AsyncSession = Depends(get_db)):
    result = await db.execute(
        select({Dominio}).order_by({Dominio}.id.desc()).limit(1)
    )
    obj = result.scalars().first()
    if not obj:
        raise HTTPException(status_code=404, detail="No hay registros")
    return obj

@router.post("/", response_model={Dominio}Response)
async def create(
    form_data: {Dominio}Create = Depends(get_{dominio}_form),
    image: Optional[UploadFile] = File(None),
    db: AsyncSession = Depends(get_db)
):
    image_url = None
    if image and image.filename:
        image_url = await upload_image(image)  # retorna str con URL pública
    db_obj = {Dominio}(**form_data.dict(), image_url=image_url)
    db.add(db_obj)
    await db.commit()
    await db.refresh(db_obj)
    return db_obj

@router.put("/{id}", response_model={Dominio}Response)
async def update(
    id: int,
    form_data: {Dominio}Update = Depends(get_{dominio}_update_form),
    image: Optional[UploadFile] = File(None),
    db: AsyncSession = Depends(get_db)
):
    obj = await db.get({Dominio}, id)
    if not obj:
        raise HTTPException(status_code=404, detail="{Dominio} no encontrado")
    for key, value in form_data.dict(exclude_none=True).items():
        setattr(obj, key, value)
    if image and image.filename:
        obj.image_url = await upload_image(image)
    # Si no llega imagen nueva se conserva la URL existente en DB
    await db.commit()
    await db.refresh(obj)
    return obj

@router.delete("/{id}")
async def delete(id: int, db: AsyncSession = Depends(get_db)):
    obj = await db.get({Dominio}, id)
    if not obj:
        raise HTTPException(status_code=404, detail="{Dominio} no encontrado")
    await db.delete(obj)
    await db.commit()
    return {"detail": "{Dominio} eliminado"}
```

---

## PASO 6 — Registrar en `backend/app/api/route.py`

Agregar al archivo existente sin eliminar routers ya registrados:
```python
from app.api.v1.endpoints.{dominio} import router as {dominio}_router

api_router.include_router(
    {dominio}_router,
    prefix="/{dominios}",  # plural snake_case
    tags=["{Dominio}"]
)
```

---

## Reglas absolutas

- ❌ Nunca usar `from app.core.config import get_db` — `get_db` vive en `app.db.session`
- ❌ Nunca inferir paths de imports — usar solo el mapa de imports del inicio
- ❌ Nunca usar `modelo: ModeloCreate` directamente en POST/PUT —
  siempre `Depends(get_{dominio}_form)`
- ❌ Nunca mezclar imports entre dominios
- ❌ Nunca eliminar routers ya existentes en `route.py`
- ❌ Nunca guardar binario de imagen — solo la URL que devuelve `upload_image()`
- ❌ Nunca llamar a `cloudinary.uploader.upload()` fuera de `app/core/cloudinary.py`
- ❌ Nunca asumir que `app.core.cloudinary` existe — verificar y crear si falta
- ✅ `upload_image()` siempre retorna `str` con URL `https://` — usarla
  directamente como `image_url` en el modelo
- ✅ Si en PUT no llega imagen nueva, conservar `image_url` actual de DB
- ✅ Si un campo es opcional en el frontend, marcarlo `nullable=True`
  en el modelo y `Optional` en el schema
- ✅ Ante cualquier ambigüedad de dominio o import, preguntar antes de crear archivos
