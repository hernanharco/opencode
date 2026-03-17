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
│   └── user.py               # modelo User simple
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
│           ├── auth.py
│           └── users.py
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
- models/user.py → modelo User (id PK, email unique, hashed_password, is_active, created_at)
- main.py → lifespan con logs emojis exactos + await conn.run_sync(Base.metadata.create_all)
- route.py → api_router con include_router para auth y users (prefix y tags limpios)
- endpoints/auth.py y users.py → routers mínimos pero funcionales

Asegúrate de que:
- No haya asyncpg en uso
- No haya errores de connect_args inesperados
- La conexión use channel_binding para máxima protección MITM

Objetivo final: pnpm dev muestra todos los logs con ✅ "Conexión exitosa: Tablas verificadas/creadas en NEON." y crea la tabla users automáticamente en Neon con la configuración segura.

Cuando termines, responde exactamente:

**"Proyecto completo con máxima seguridad – psycopg + channel_binding activo – pnpm dev debería conectar y crear tabla users"**
Una vez que Windsurf termine este Prompt 3 y confirme, ejecuta:
Bashpoetry install
pnpm dev
