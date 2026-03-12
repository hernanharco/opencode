# con agentes
____

## 1. En Windsurf (Ahorra tokens de Antigravity):
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

## 2. Pedir que diseñe un archivo md de inicio para que pueda trabajar la LLM ejemplo puede ser claude.md / .windsurfrules / .cursorrules
_______
## 3. "@.cursorrules Migra el componente Navbar.tsx a Svelte. Asegúrate de que los enlaces funcionen como anclas en la misma página de Astro."
______
## 4. 🛠️ Plan de Rescate Visual (Sin gastar tokens extra)
Vamos a usar a Windsurf para este ajuste, ya que es mejor comparando archivos de configuración.

🛠️ Plan de Rescate Visual (Sin gastar tokens extra)
Vamos a usar a Windsurf para este ajuste, ya que es mejor comparando archivos de configuración.

Paso 1: Sincronizar el "ADN" Visual
Pídele esto a Windsurf:

"Compara el archivo tailwind.config.js de la raíz (el viejo) con frontend/tailwind.config.mjs. Copia todos los colors, fontFamily, extend y plugins del viejo al nuevo. Asegúrate de que los colores 'brand' de la doctora sean exactamente los mismos."

Paso 2: Traer las Fuentes
Busca en el index.html original (el de la raíz) las etiquetas <link> de fuentes y dile a Windsurf:

"Copia las importaciones de fuentes del index.html viejo al componente de Layout principal en Astro (o al src/pages/index.astro). El diseño no se ve igual porque faltan las fuentes."

Paso 3: Ajuste de Componentes con Antigravity (Modo Flash)
Una vez que los colores y fuentes sean correctos, si todavía ves algo raro, dile a Antigravity:
___
## 5. Se debe revisar el archivo global.css y se puede tomar como referencia el archivo style/theme.css

"El diseño migrado se ve muy simple comparado con el original. Revisa Hero.svelte y asegúrate de aplicar los mismos márgenes (px, py), tamaños de texto (text-4xl, etc.) y pesos de fuente (font-bold, font-light) que tiene el Hero.tsx original. El original tiene un acabado mucho más profesional."
