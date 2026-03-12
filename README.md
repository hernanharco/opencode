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

# Reglas para este proyecto - Landing Dra. Andrea

## General
- Este es un proyecto Astro + Svelte 5 + Tailwind CSS.
- Siempre convierte componentes React/TSX a Svelte 5 usando runes ($state, $derived, $effect).
- Mantén EXACTAMENTE las mismas clases Tailwind, estructura HTML, nombres de variables/props, ids y data attributes.
- No agregues imports innecesarios, no cambies funcionalidad, no agregues lógica nueva ni optimizaciones sin pedir permiso.
- Usa <script lang="ts"> para TypeScript.
- Para componentes en páginas Astro: usa client:load para Navbar y Hero (si tienen interactividad como menú mobile o animaciones), y client:visible para About, Services, Booking y Footer (lazy loading).
- Convierte eventos React (onClick → on:click, onChange → on:change, etc.).
- Usa $state para variables reactivas, $derived para computados, $effect para side-effects.

## Estilos y Tailwind
- Nunca elimines ni modifiques clases Tailwind existentes (ej: bg-stone-50, text-primary-600, hover:scale-105, etc.).
- Si el componente original usa clases custom (no-Tailwind) de archivos en /src/styles/ o globales, mantenlas y copia los imports correspondientes (ej: import '../styles/hero.css';).
- Preserva todos los inline styles si los hay (style="...").
- Si hay referencias a variables CSS custom (--primary, --accent, etc.), mantenlas intactas.

## Imágenes y Assets
- Mantén exactamente las mismas rutas de imágenes que en el componente original (ej: src="/assets/hero-bg.jpg", src={import.meta.env.BASE_URL + '/images/logo.png'}, etc.).
- Si el componente original usa imports de imágenes (import heroImg from '../assets/hero.png';), conviértelo a la sintaxis equivalente en Svelte/Astro: let heroImg = '/assets/hero.png'; o usa import si es necesario.
- No cambies ni optimices paths de assets; asume que la estructura de carpetas /public o /src/assets se mantiene idéntica.
- Si ves placeholders o URLs externas de Figma, mantenlas tal cual (incluso si son temporales).
- Copia cualquier <img> con alt, loading="lazy", decoding="async", etc., exactamente igual.

## Estructura de archivos
- Genera el componente como Nombre.svelte en la misma carpeta relativa (ej: si era components/Navbar.tsx → components/Navbar.svelte).
- Si el componente tiene <style> scoped en React (styled-jsx o CSS-in-JS), conviértelo a <style> normal en Svelte (scoped por default).
- No uses CSS modules a menos que ya existan; prioriza Tailwind + global styles.

## Extras
- Si detectas animaciones (framer-motion, etc.), conviértelas a Svelte transitions/animations o mantenlas con CSS si son simples.
- Para el ChatWidget: preserva cualquier estado de abierto/cerrado, animaciones de entrada/salida y enlaces a WhatsApp/teléfono.
- Siempre valida que el componente renderice visualmente idéntico al original (mismo spacing, colors, responsive breakpoints).

  
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
