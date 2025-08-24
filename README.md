# Sanity Visual Editing Demo

Demo de edición visual usando Sanity + Next.js, con TailwindCSS y Styled Components. Muestra la integración de Sanity Studio con el frontend Next.js para edición en tiempo real y despliegue en Vercel.

Demo: https://sanity-visual-editing-demo-xi-lovat.vercel.app

## Características
- Integración completa con Sanity CMS
- Edición visual en tiempo real (Preview / Visual Editing)
- Next.js (SSR / SSG) con soporte de preview
- TailwindCSS + Styled Components
- Despliegue fácil en Vercel

## Tech stack
- Next.js (TypeScript)
- Sanity (Studio + Content Lake)
- TailwindCSS
- styled-components
- Vercel (hosting / CI)

---

## Requisitos
- Node.js 18+ (recomendado)
- npm o yarn
- Git
- (Opcional) Sanity CLI si quieres ejecutar la studio localmente con `sanity`:
  - npm i -g @sanity/cli

---

## Instalación (local)

1. Clonar
   git clone https://github.com/Lmillan12co/sanity-visual-editing-demo.git
   cd sanity-visual-editing-demo

2. Instalar dependencias
   npm install
   # o
   yarn install

3. Crear archivo de variables de entorno
   - Copia el ejemplo y actualiza los valores:
     cp .env.local.example .env.local
   - (Si no existe el archivo de ejemplo, crea `.env.local` con las variables descritas abajo.)

Variables de entorno necesarias (ejemplo)
- NEXT_PUBLIC_SANITY_PROJECT_ID=yourProjectId
- NEXT_PUBLIC_SANITY_DATASET=production
- SANITY_API_READ_TOKEN=readTokenIfNeeded
- SANITY_API_WRITE_TOKEN=writeTokenForStudio(optional)
- NEXT_PUBLIC_VERCEL_URL=https://your-deployment.vercel.app (opcional para previews)

Ejemplo .env.local
NEXT_PUBLIC_SANITY_PROJECT_ID=yourProjectId
NEXT_PUBLIC_SANITY_DATASET=production
SANITY_API_READ_TOKEN=yourReadToken
SANITY_API_WRITE_TOKEN=yourWriteToken

> Nota: Solo las variables que comienzan con NEXT_PUBLIC_ estarán disponibles en el cliente. Mantén tokens privados fuera del cliente.

---

## Scripts comunes

(Estos nombres pueden variar según package.json; ajusta según tu repo.)

- npm run dev — Ejecuta Next.js en modo desarrollo (http://localhost:3000)
- npm run build — Construye la app para producción
- npm run start — Inicia servidor Next.js en modo producción
- npm run studio — Inicia Sanity Studio local (si está integrado como paquete o carpeta `studio`)
- npm run lint — Ejecuta linters
- npm run format — Formatea el código

Ejecutar en desarrollo (dos terminales):
1) Frontend:
   npm run dev
2) Sanity Studio (si está separado):
   cd studio && sanity start
o si hay script en el root:
   npm run studio

---

## Visual Editing / Preview (guía general)

1. Habilita Preview desde Sanity:
   - En la Studio configura el plugin o los `document` actions para permitir abrir páginas en modo preview. (Revisa `sanity/plugins` o `studio` config en el repo.)
2. Implementa un endpoint de preview en Next.js:
   - Un archivo típico `pages/api/preview.ts` que verifica un token y activa `res.setPreviewData({})` y redirige a la ruta del documento.
3. En las páginas que usan datos de Sanity:
   - En `getStaticProps`, acepta `preview` y usa la query de preview cuando `preview === true`.
   - Opcional: usa `usePreviewSubscription` (Sanity client helpers) para suscribirte a cambios en tiempo real cuando estés en preview.
4. Tokens / permisos:
   - Para preview necesitas un token de lectura con permisos adecuados. Mantenlo en variables de entorno del servidor (no expuesto en el cliente).

Si quieres, puedo inspeccionar los archivos relevantes (por ejemplo `pages/api/preview`, `lib/sanity`, `studio/schemas`) y darte instrucciones exactas adaptadas al código del repo.

---

## Construir y desplegar (Vercel)

1. Conectar repo en Vercel (Import Project).
2. En Settings > Environment Variables, añade:
   - NEXT_PUBLIC_SANITY_PROJECT_ID
   - NEXT_PUBLIC_SANITY_DATASET
   - SANITY_API_READ_TOKEN (si tu app lo necesita en runtime)
   - SANITY_API_WRITE_TOKEN (solo si la Studio está en el mismo repo y Vercel la hospeda)
3. Build command: npm run build
4. Output directory: .next (Vercel detecta Next.js automáticamente)

Despliegue de la Studio:
- Si la Studio está en la misma repo y se sirve como parte del sitio o como una carpeta separada (`/studio`), configura un script para construirla y publicarla o despliega la studio por separado usando Sanity hosting o Vercel (según el flujo del repo).

---

## Estructura sugerida del repo
- /pages o /app — Next.js frontend
- /lib / /utils — helpers para Sanity client y queries
- /studio o /sanity — Sanity Studio, schemas y config
- tailwind.config.js, postcss.config.js — configuración de Tailwind
- .env.local.example — ejemplo de variables de entorno

---

## Buenas prácticas y notas
- Mantén los tokens de escritura fuera de las variables públicas.
- Usa dataset `production` para despliegue y `development` o sandbox para pruebas.
- Revisa las CORS y los permisos del token en el proyecto Sanity.
- Añade checks en CI para lint/build.

---

## Contribuciones
1. Crea una rama feature/mi-cambio
2. Abre un PR contra main con descripción y pasos para reproducir
3. Mantén tests o instrucciones si agregas cambios importantes

---

## Licencia
Añade la licencia que prefieras (MIT, Apache-2.0, etc.). Si quieres, te propongo MIT.

---

¿Quieres que:
- Commitée este README directamente en la rama `main`?
- Lo ajuste para reflejar scripts/archivos reales sacados del package.json y la estructura del repo? (Puedo inspeccionar el repo y personalizarlo.)  

— GitHub Copilot Chat Assistant