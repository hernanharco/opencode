Ahora que tenemos la base limpia, vamos a implementar la persistencia de datos.
Respeta estrictamente SRP: un archivo por dominio, sin mezclar lógica.

## Tarea

Analiza los componentes en frontend/src/components/ y por cada dominio crea:

### 1. Modelo (`backend/app/models/{dominio}.py`)
- Campos que reflejen exactamente los datos del componente frontend
- Imágenes → `String` (URL)
- Iconos Lucide → `String` (nombre del icono)
- Colores → `String` (hexadecimal)
- Listas → `JSONB`

### 2. Schema (`backend/app/schemas/{dominio}.py`)
- `{Dominio}Create`, `{Dominio}Update`, `{Dominio}Response`
- Los campos deben coincidir exactamente con el modelo

### 3. Endpoint (`backend/app/api/v1/endpoints/{dominio}.py`)
- CRUD completo: GET all, GET one, GET latest (si aplica), POST, PUT, DELETE
- ⚠️ CRÍTICO: Los endpoints POST y PUT deben recibir `multipart/form-data`,
  NO JSON. Usar este patrón obligatoriamente:
```python
  from fastapi import Form, File, UploadFile
  from typing import Optional

  # Función de dependencia LOCAL en el mismo archivo
  async def get_{dominio}_form(
      campo1: str = Form(...),
      campo2: str = Form(...),
      # todos los campos del schema
  ) -> {Dominio}Create:
      return {Dominio}Create(campo1=campo1, campo2=campo2)

  @router.post("/", response_model={Dominio}Response)
  async def create_{dominio}(
      form_data: {Dominio}Create = Depends(get_{dominio}_form),
      image: Optional[UploadFile] = File(None),
      db: AsyncSession = Depends(get_db)
  ):
      image_url = None
      if image and image.filename:
          image_url = await upload_image(image)
      db_obj = {Dominio}(**form_data.dict(), image_url=image_url)
      ...
```

- Importar `upload_image` desde `app.core.cloudinary`
- Nunca usar `hero: HeroCreate` directo en POST/PUT — siempre `Depends(get_{dominio}_form)`

## Restricciones
- Un archivo por dominio, sin mezclar lógica entre dominios
- Los campos del modelo DB deben venir de los componentes frontend, no inventarlos
- Registrar cada router en `backend/app/api/route.py`

## Antes de escribir código
Muestra la estructura de archivos que vas a crear y los campos
que extrajiste de cada componente frontend.
