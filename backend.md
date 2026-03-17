# Prompt 1 – Crear estructura base (cópialo completo y pégalo en Windsurf)
MarkdownCrea desde cero un proyecto FastAPI llamado **backend** con Poetry, enfocado en máxima seguridad para conexión a Neon Postgres.

Usa Python 3.12 y configura todo para psycopg 3 async + channel_binding=require.

Estructura de carpetas EXACTA:
backend/
├── package.json
├── pyproject.toml
├── .gitignore
├── poetry-setup.sh
├── Dockerfile
└── app/
├── init.py
├── main.py                   # con lifespan y logs con emojis
├── core/
│   └── config.py             # Pydantic Settings v2
├── db/
│   └── session.py            # AsyncEngine con psycopg
├── models/
│   ├── init.py
│   ├── base.py
│   └── example.py               # modelo User simple
├── schemas/
│   ├── init.py
│   ├── user.py
│   └── token.py
├── api/
│   ├── init.py
│   ├── route.py              # router principal
│   └── v1/
│       ├── init.py
│       └── endpoints/
│           ├── init.py
│           ├── example.py
└── tests/
└── init.py
text**package.json EXACTO (cópialo tal cual):**

```json
{
  "name": "auth-core-backend",
  "version": "1.0.0",
  "scripts": {
    "dev": "poetry run uvicorn app.main:app --reload --host 0.0.0.0",
    "start": "poetry run uvicorn app.main:app --host 0.0.0.0 --port 8000",
    "test": "poetry run pytest",
    "test:coverage": "poetry run pytest --cov=app --cov-report=html",
    "lint": "poetry run black app/ && poetry run isort app/ && poetry run flake8 app/",
    "format": "poetry run black app/ && poetry run isort app/",
    "type-check": "poetry run mypy app/",
    "docker:build": "docker build -t auth-core-backend .",
    "docker:run": "docker run -p 8000:8000 --env-file .env auth-core-backend",
    "setup:dev": "bash poetry-setup.sh development",
    "setup:prod": "bash poetry-setup.sh production",
    "shell": "poetry shell",
    "install": "poetry install",
    "install:prod": "poetry install --only main",
    "add": "poetry add",
    "add:dev": "poetry add --group dev",
    "update": "poetry update",
    "export": "poetry export -f requirements.txt --output requirements.txt --without-hashes"
  }
}
```
pyproject.toml – dependencias obligatorias (incluye psycopg para máxima seguridad):
Incluye en [tool.poetry.dependencies]:
fastapi, uvicorn[standard], sqlalchemy[asyncio], psycopg[binary,pool], pydantic-settings, python-dotenv
Grupo dev: black, isort, flake8, mypy, pytest, pytest-cov
main.py debe tener lifespan con logs exactos (🚀 Starting..., 🌍 Timezone..., 🕒 Local Time..., 🔗 Database URL..., --- Verificando..., ✅ Conexión exitosa..., INFO: Application startup complete.) y crear tablas con Base.metadata.create_all
No crees .env todavía (lo haré manual después).
Cuando termines toda la estructura, responde exactamente:
"Estructura base creada correctamente – listo para .env manual y máxima seguridad con psycopg"
text### Después de que Windsurf termine el Prompt 1 y diga el mensaje de confirmación

1. Ve a la carpeta creada: `backend`

2. Crea el archivo `.env` manualmente con este contenido (máxima seguridad):

```env
PGHOST=ep-autumn-forest-al0yrhj9-pooler.c-3.eu-central-1.aws.neon.tech
PGDATABASE=neondb
PGUSER=neondb_owner
PGPASSWORD=npg_DRAFwbvEm59W
PGSSLMODE=require
PGCHANNELBINDING=require

DATABASE_URL=postgresql+psycopg://${PGUSER}:${PGPASSWORD}@${PGHOST}/${PGDATABASE}?sslmode=require&channel_binding=require
```
Guárdalo.

## Prompt 3 – Finalizar con máxima seguridad (pégalo después de crear .env)
MarkdownEl proyecto ya tiene la estructura completa y el archivo .env creado manualmente con credenciales Neon + sslmode=require&channel_binding=require usando postgresql+psycopg (máxima seguridad recomendada por Neon).

Ahora completa y corrige todo el código para que funcione perfectamente con psycopg 3:

- config.py → lee DATABASE_URL con Pydantic Settings v2
- session.py → create_async_engine(settings.database_url) + AsyncSession (psycopg se detecta automáticamente)
- models/base.py → Base = declarative_base()
- models/example.py → modelo Example (id)
- main.py → lifespan con logs emojis exactos + await conn.run_sync(Base.metadata.create_all)
- route.py → api_router con include_router para example (prefix y tags limpios)
- endpoints/example → routers mínimos pero funcionales

Asegúrate de que:
- No haya asyncpg en uso
- No haya errores de connect_args inesperados
- La conexión use channel_binding para máxima protección MITM

Objetivo final: pnpm dev muestra todos los logs con ✅ "Conexión exitosa: Tablas verificadas/creadas en NEON." y crea la tabla example automáticamente en Neon con la configuración segura.

Cuando termines, responde exactamente:

**"Proyecto completo con máxima seguridad – psycopg + channel_binding activo – pnpm dev debería conectar y crear tabla example"**
Una vez que Windsurf termine este Prompt 3 y confirme, ejecuta:

_____
### errores encontrados
el archivo base debe tener esta forma
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

# Importamos los modelos aquí para que se registren en el objeto Base
# Asegúrate de que la ruta coincida con tus archivos reales
try:
    from app.models.user import User 
    # Aquí irás agregando los demás: 
    # from app.models.appointment import Appointment
except ImportError:
    pass

y en main debe utilizar 
from zoneinfo import ZoneInfo
Bashpoetry install
pnpm dev

### ejemplos de clases
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession
from sqlalchemy.orm import sessionmaker
from app.core.config import settings

engine = create_async_engine(
    settings.database_url,
    echo=False,
    pool_pre_ping=True,
)

AsyncSessionLocal = sessionmaker(
    engine, class_=AsyncSession, expire_on_commit=False
)


async def get_db() -> AsyncSession:
    async with AsyncSessionLocal() as session:
        yield session


from contextlib import asynccontextmanager
from datetime import datetime
from zoneinfo import ZoneInfo
import sys

from fastapi import FastAPI
from app.core.config import settings
from app.db.session import engine
from app.models.base import Base
from app.api.route import api_router


@asynccontextmanager
async def lifespan(app: FastAPI):
    print("🚀 Starting FastAPI app in development mode")
    local_tz = ZoneInfo("Europe/Madrid")
    local_time = datetime.now(local_tz).strftime("%Y-%m-%d %H:%M:%S")
    print(f"🌍 Timezone: {local_tz} | 🕒 Local Time: {local_time}")

    print("--- Verificando conexión a NEON (development) ---")
    try:
        async with engine.begin() as conn:
            # Función interna para inspeccionar tablas existentes
            def get_tables(connection):
                from sqlalchemy import inspect
                return inspect(connection).get_table_names()

            existing_tables = await conn.run_sync(get_tables)
            metadata_tables = Base.metadata.tables.keys()
            
            # Solo ejecutamos si hay modelos en código que no están en la DB
            new_tables = [t for t in metadata_tables if t not in existing_tables]
            
            await conn.run_sync(Base.metadata.create_all)
            
            if new_tables:
                print(f"✅ Nuevas tablas creadas/detectadas: {', '.join(new_tables)}")
            else:
                print("info: Schema sincronizado (sin cambios pendientes)")
                
    except Exception as e:
        print(f"❌ Error en DB: {str(e)}", file=sys.stderr)
        raise
    yield
    await engine.dispose()


app = FastAPI(
    title="Auth Core Backend",
    description="Backend de autenticación con FastAPI y Neon Postgres",
    version="1.0.0",
    lifespan=lifespan,
    docs_url="/docs",           # Swagger UI
    redoc_url="/redoc",         # ReDoc (opcional)
    openapi_url="/openapi.json"
)


app.include_router(api_router, prefix="/api/v1")


@app.get("/")
async def root():
    return {"message": "✅ Backend está corriendo correctamente"}


@app.get("/health")
async def health_check():
    # Puedes mejorar esto más adelante verificando realmente la conexión
    return {"status": "healthy", "database": "connected (verificado en startup)"}
