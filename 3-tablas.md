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

Prompt para Windsurf: CMS + Integración de Cloudinary
Contexto:
Ya tenemos el backend con FastAPI y el frontend con Astro/Svelte. Necesito implementar el CMS en /admin y configurar la gestión de imágenes mediante Cloudinary.

Instrucciones para el Backend (FastAPI):

Configuración de Cloudinary: * Utiliza la librería cloudinary.

Lee las credenciales (CLOUDINARY_CLOUD_NAME, CLOUDINARY_API_KEY, CLOUDINARY_API_SECRET) desde el archivo .env.

Servicio de Upload: Crea un módulo app/core/cloudinary.py para gestionar las subidas.

Endpoints de Imagen: Los endpoints de POST y PUT que manejen imágenes (como en Proyectos o Hero etc) deben:

Recibir un UploadFile.

Subir la imagen a Cloudinary.

Guardar únicamente la URL resultante en la base de datos de Neon.

Instrucciones para el Frontend (Svelte):

Input de Archivos: En los formularios del CMS ejemplo (Hero, Proyectos), añade un campo de tipo file para las imágenes.

Previsualización: Muestra una miniatura de la imagen antes de subirla.

Carga: Al dar click en "Guardar", envía el archivo al backend usando FormData.

Credenciales para tu .env (Basado en tu imagen):

CLOUDINARY_CLOUD_NAME=dxyk

CLOUDINARY_API_KEY=1857218

CLOUDINARY_API_SECRET=YqnqGm_iF3J... (Cópialo completo desde tu consola).

Objetivo Final:
Cuando suba una imagen en el panel de control, esta debe guardarse en Cloudinary y el portfolio debe mostrarla usando la URL de la nube.

Respuesta esperada: "CMS con soporte para Cloudinary configurado – Las imágenes ahora se gestionan en la nube."
