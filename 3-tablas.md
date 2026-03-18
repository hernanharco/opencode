"Ahora que tenemos la base limpia, vamos a implementar la persistencia de datos. Para respetar estrictamente el Principio de Responsabilidad Única (SRP) y nuestra arquitectura de agentes modulares, no crearemos un archivo único.

Necesito que analices los componentes en frontend/src/components (Hero, About, Stack, Projects, etc.) y por cada uno de ellos crees una estructura independiente:

Modelos: Un archivo por dominio en backend/app/models/ (ej: hero.py, projects.py). Usa PostgreSQL con Neon y tipos JSONB donde sea necesario (para listas de iconos o tags).

Schemas: Un archivo por dominio en backend/app/schemas/ (ej: hero.py, projects.py).

Endpoints: Un archivo por dominio en backend/app/api/v1/endpoints/ (ej: hero_router.py, projects_router.py).

Restricciones técnicas:

Las imágenes deben guardarse como campos de URL (string).

Los iconos de Lucide deben guardarse como strings con el nombre del icono.

Los colores deben guardarse como strings (hexadecimal).

Prohibido: No mezcles la lógica de Hero con la de Projects en el mismo archivo. Cada dominio debe ser un 'nodo' independiente que luego conectaremos en el grafo principal.

Por favor, empieza mostrándome la estructura de archivos que vas a crear antes de generar el código de cada uno."
____

🚀 Prompt de Arquitectura Modular para Windsurf (Full SRP & Cloudinary)
Contexto:
Ya tengo un backend FastAPI y un frontend Astro/Svelte. Necesito implementar la gestión de imágenes con Cloudinary y una interfaz /admin profesional. Es obligatorio seguir el Principio de Responsabilidad Única (SRP) para evitar archivos "monstruo".

1. Backend: Desacoplamiento de Servicios (SRP)

Servicio Cloudinary: Crea app/core/cloudinary.py. Responsabilidad única: conectar con la API (usando .env) y proveer la función upload_image(file: UploadFile).

Refactorización de Endpoints: Analiza todos los archivos en app/api/v1/endpoints/.

Para cada dominio (Hero, Project, etc.), actualiza los métodos POST/PUT para aceptar UploadFile.

Usa el servicio de Cloudinary para obtener la URL y guardarla en Neon.

Configuración: Asegura que app/core/config.py valide las variables de Cloudinary y que app/main.py tenga el middleware de CORS para http://localhost:4321.

2. Frontend: Estructura de Componentes Atómicos
Divide la interfaz en archivos independientes para que cada uno gestione su propio dominio:

Nueva Carpeta de Layout: Crea src/layouts/AdminPanel.svelte. Este será el Orquestador. Debe gestionar el estado de la pestaña activa (activeTab) y renderizar dinámicamente el editor correspondiente.

Componentes de Dominio (src/components/admin/):

AdminSidebar.svelte: Solo gestiona la navegación lateral y emite el cambio de pestaña.

ImagePreview.svelte: Componente reutilizable para mostrar la imagen localmente antes de subirla.

Editores Específicos: Crea HeroEditor.svelte, ProjectEditor.svelte, AboutEditor.svelte, etc. Cada uno maneja su propia lógica de fetch, su FormData y su estado de carga.

Página Astro: src/pages/admin.astro debe quedar limpio, importando y cargando únicamente <AdminPanel client:load />.

3. UX y Diseño Visual

Sidebar: Diseño oscuro, fijo a la izquierda, acentos en color ámbar.

Feedback: Implementar estados de "Cargando..." y mensajes de éxito/error independientes en cada editor.

Preview: Al seleccionar un archivo en cualquier editor, el componente ImagePreview debe mostrar la imagen inmediatamente.

Instrucción de Análisis: Antes de escribir código, analiza app/api/v1/endpoints/ y src/components/ para mantener la coherencia con los nombres de campos y archivos existentes. No mezcles lógica de distintos dominios en un mismo componente.


Fragmento de código
```
CLOUDINARY_CLOUD_NAME=dxyk76jhu
CLOUDINARY_API_KEY=185721842557166
CLOUDINARY_API_SECRET=TU_API_SECRET_COMPLETO
```


Respuesta esperada: "CMS con soporte para Cloudinary configurado – Las imágenes ahora se gestionan en la nube."
