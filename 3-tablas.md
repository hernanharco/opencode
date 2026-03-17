genial quedo perfecto ahora. en la carpeta de frontend/src/components revisa cada una de la clases
@About.svelte 
@Footer.svelte 
@Hero.svelte 
@Navbar.svelte 
@Passions.svelte 
@Projects.svelte 
@Stack.svelte 

revisa cada una y crea las tablas, los campos y los endpoints necesarios para modificar la informacion tener encuenta las url en donde haya imagenes, iconos y colores 

____

Prompt para Windsurf: CMS, Cloudinary y Seguridad de Dominio
Contexto:
Ya tenemos el backend FastAPI y el frontend Astro/Svelte. Necesito implementar la gestión de imágenes con Cloudinary y la interfaz /admin, pero respetando estrictamente el Principio de Responsabilidad Única (SRP). No quiero un archivo admin.py que gestione todo; cada dominio debe gestionarse a sí mismo.

1. Configuración del Núcleo (Main & Security):

Seguridad CORS: En app/main.py, configura el CORSMiddleware para permitir http://localhost:4321 con allow_credentials=True, allow_methods=["*"] y allow_headers=["*"].

Registro de Modelos: Asegúrate de que app/main.py importe app.models para que Base.metadata reconozca todas las tablas en el lifespan.

2. Integración de Cloudinary (Backend):

Servicio: Crea app/core/cloudinary.py. Usa la librería cloudinary y configura las credenciales desde el .env (CLOUDINARY_CLOUD_NAME, CLOUDINARY_API_KEY, CLOUDINARY_API_SECRET).

Función de utilidad: Crea una función asíncrona upload_image(file: UploadFile) que devuelva únicamente la URL segura de Cloudinary.

3. Refactorización de Endpoints (SRP):

Eliminar Duplicados: Si existe un archivo api/v1/endpoints/admin.py con lógica de Hero, Projects, etc., elimina esa lógica.

Endpoints de Dominio: Modifica los archivos hero.py, projects.py y otros que requieran imágenes:

Cambia los métodos POST/PUT para que acepten File y UploadFile.

Usa el servicio de Cloudinary para subir la imagen y guarda la URL resultante en la base de datos de Neon.

4. Frontend (Svelte en Astro):

Ruta Admin: Crea /src/pages/admin/index.astro (o similar) que use componentes de Svelte.

Formularios Dinámicos: Los formularios deben usar FormData para enviar archivos al backend.

UX: Implementa previsualización de imagen (Preview) antes de subir y un estado de "Cargando..." mientras se procesa en la nube.

Credenciales .env:

Fragmento de código
´´´
CLOUDINARY_CLOUD_NAME=dxyk76jhu

CLOUDINARY_API_KEY=185721842557166

CLOUDINARY_API_SECRET=TU_API_SECRET_COMPLETO
´´´
Objetivo: El sistema debe permitirme actualizar el Hero o un Proyecto desde el panel /admin, subiendo la imagen a Cloudinary y guardando el link en Neon, sin errores de CORS.

Respuesta esperada: "CMS con soporte para Cloudinary configurado – Las imágenes ahora se gestionan en la nube."
