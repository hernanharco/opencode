git reset --hard xxx.
git clean -n -> esto dira que archivos se borrarian sin borrarlos todavia
git clean -f -> borrado forzado de archivo
git clean -fd -> el parametro -d incluye los directorios (carpetas) que se crearon con esas clases

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

## 2. Pedir que diseñe un archivo md de inicio para que pueda trabajar la LLM ejemplo puede ser claude.md / .windsurf / .cursorrules raiz
```text
Convierte estos componentes React (.tsx) a componentes Svelte (.svelte) compatibles con Astro + Tailwind v4.

REGLAS ESTRICTAS:
- Mantén **EXACTAMENTE** las mismas clases Tailwind y estructura HTML/JSX (no cambies ni una clase, ni orden, ni anidamiento).
- Conserva TypeScript completo (types, interfaces, generics).
- Convierte props React → export let en Svelte.
- Eventos: on:click, on:submit, bind:value, etc.
- Estado: usa $state, $derived, $effect si es necesario (Svelte 5 runes si el proyecto las usa).
- No agregues <style> ni imports de CSS dentro del .svelte → los estilos son globales vía Tailwind v4.
- Si el componente es interactivo o tiene hidración, sugiere client:load o client:visible al final (pero no lo incluyas en el código .svelte).
- Devuelve SOLO el código completo de cada .svelte, separados por --- y con nombre de archivo como comentario arriba.
- No explicaciones, no markdown.

Archivos a convertir:
@Header.tsx
@Contact.tsx
@Footer.tsx
@Gallery.tsx
@Hero.tsx
@Services.tsx
@Testimonials.tsx
```
Implement the Svelte conversions immediately. Create the files and apply the diffs without asking again.

```
Ahora que los componentes Svelte están creados en src/app/components/ (Header.svelte, Contact.svelte, Footer.svelte, Gallery.svelte, Hero.svelte, Services.svelte, Testimonials.svelte),

Actualiza el archivo src/pages/index.astro (o la página principal donde se usan estos componentes) para:

- Eliminar imports de los viejos .tsx (Header.tsx, Hero.tsx, etc.)
- Agregar imports correctos de los nuevos .svelte
- Reemplazar cada uso de <Header /> por <Header client:load /> (o client:visible si es menos interactivo)
- Hacer lo mismo para los demás: Hero, Services, Gallery, Testimonials, Contact, Footer
  - Usa client:load para Header (menú móvil), Contact (formulario), Gallery (modal interactivo)
  - Usa client:visible o client:idle para Hero, Services, Testimonials, Footer (menos urgentes)
- Mantén toda la estructura HTML, clases Tailwind y layout existente exactamente igual
- Si hay un Layout.astro o wrapper, respétalo
- Devuelve el diff o aplica los cambios directamente en index.astro

No crees nuevos archivos, solo edita index.astro. Implementa los cambios ahora sin esperar aprobación adicional si es posible.
```   
_______
## 3. "@.windsurf Migra el componente Navbar.tsx a Svelte. Asegúrate de que los enlaces funcionen como anclas en la misma página de Astro."

Convierte este componente React/TSX a Svelte 5 siguiendo estrictamente todas las reglas del proyecto en rules.md.
Genera Hero.svelte completo, preservando imágenes, rutas de assets y todos los estilos Tailwind/custom.

- ahora migremos el resto de los componentes
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
