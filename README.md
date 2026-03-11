# con agentes
____

1. En Windsurf (Ahorra tokens de Antigravity):
Abre la carpeta en Windsurf y pídele que prepare el terreno. Como ya tienes la carpeta frontend/, dile a Cascade:

🛠️ Prompt Maestro de Inicialización (Linux + Astro + Tailwind + Svelte)
"Inicializa un nuevo proyecto de Astro en la carpeta frontend/ usando pnpm.

1. Gestión de Permisos (Linux):
Ejecuta todos los comandos asegurándote de que los permisos de las carpetas y archivos creados sean los correctos para mi usuario local (chown si es necesario), evitando bloqueos de permisos al instalar dependencias.

2. Stack Técnico y Estabilidad:

  - Instala Tailwind CSS v3 (específicamente la versión estable 3.4.x, evita la v4) para garantizar compatibilidad con la integración oficial de Astro.

  - Crea un archivo postcss.config.cjs que incluya tailwindcss y autoprefixer.

  - Configura la integración de Svelte en astro.config.mjs.

3. Acciones de Ejecución:

  - Usa pnpm create astro@latest ./frontend --template minimal --install --yes.

  - Añade las integraciones con pnpm astro add tailwind svelte --yes.

  - Validación Final: Ejecuta pnpm dev y verifica que el servidor arranque sin errores de PostCSS o Svelte. Si hay errores de versiones de Svelte (v4 vs v5), ajusta los overrides en el package.json para mantener la estabilidad.

*Regla de Oro:* No des por terminada la tarea hasta que confirmes que el 'Hola Mundo' en index.astro es visible en el navegador."
_____
