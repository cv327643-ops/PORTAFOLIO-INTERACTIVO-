# Neomech 3D — Landing page

Landing interactiva de Neomech 3D (diseño, ingeniería y fabricación digital con impresión 3D). La página en sí es **un solo documento** (`index.html`, con su CSS y JS adentro) — sin frameworks, sin build step. Los demás archivos de este repo son configuración de hosting, no parte de la página.

## Estructura

```
neomech-landing/
├── index.html      → la página completa (HTML + CSS + JS en un solo archivo)
├── assets/         → fotos reales del portafolio
├── _headers        → cabeceras de seguridad HTTP (formato Netlify)
└── vercel.json     → las mismas cabeceras, en formato Vercel
```

## Seguridad aplicada

Dentro de `index.html`:
- **Content-Security-Policy** vía meta tag: bloquea scripts/estilos de terceros no autorizados, solo permite el iframe del mini brief desde `docs.google.com`.
- Todos los enlaces que abren en pestaña nueva (`target="_blank"`) llevan `rel="noopener noreferrer"` — evita el hueco de seguridad conocido como *tabnabbing*.
- Se quitó del contenido visible la nota interna del fee de diseño (antes se veía en la página pública); ahora es un comentario HTML, invisible para los visitantes.
- `object-src 'none'` y `frame-ancestors 'self'` reducen superficie de ataque.

En `_headers` / `vercel.json` (cabeceras que **no se pueden fijar desde un archivo HTML**, solo desde el servidor — por eso van aparte):
- `X-Frame-Options` y `frame-ancestors`: evita que el sitio se cargue dentro de un iframe ajeno (clickjacking).
- `X-Content-Type-Options: nosniff`: evita que el navegador "adivine" tipos de archivo.
- `Strict-Transport-Security`: fuerza HTTPS en todas las visitas futuras.
- `Referrer-Policy` y `Permissions-Policy`: limitan qué información se comparte al salir del sitio y qué permisos del navegador puede pedir la página (cámara, micrófono, ubicación — ninguno, en este caso).

Netlify y Vercel leen estos archivos automáticamente al desplegar; no hay que configurar nada manualmente en su panel.

## Escalabilidad / rendimiento

- Imágenes con `width`/`height` fijos (evita saltos de layout) y `loading="lazy"` en todo lo que no sea la primera pantalla — la imagen del hero carga con prioridad alta por ser lo primero que se ve.
- Estructura semántica (`header`, `main`, `footer`) para accesibilidad y mejor indexación en buscadores.
- Meta tags de SEO (`description`, `robots`) y de redes sociales (Open Graph, Twitter Card) para que el link se vea bien al compartirlo por WhatsApp, Instagram o LinkedIn.
- Favicon incluido como SVG embebido (no necesita un archivo aparte).
- Hosting en Netlify/Vercel: ambos sirven el sitio por CDN global, con HTTPS automático y escalado sin que tengas que hacer nada — soportan picos de tráfico sin configuración extra.

## Cómo subir esto a GitHub

1. Crea un repositorio nuevo y vacío en https://github.com/new (sin README, sin .gitignore).
2. En tu computador, abre una terminal dentro de esta carpeta (`neomech-landing`) y ejecuta:

```bash
git remote add origin https://github.com/TU-USUARIO/TU-REPO.git
git branch -M main
git push -u origin main
```

Este proyecto ya viene con `git init` hecho y los commits listos — solo falta conectarlo a tu repo de GitHub con esos tres comandos.

Si prefieres no usar la terminal: en GitHub, entra al repo vacío → "uploading an existing file" → arrastra `index.html`, `_headers`, `vercel.json` y la carpeta `assets/`.

## Cómo publicarlo (dominio propio)

Con el repo en GitHub, conéctalo a **Netlify** o **Vercel** (los dos tienen plan gratis):

1. Crea cuenta en [netlify.com](https://netlify.com) o [vercel.com](https://vercel.com) con tu cuenta de GitHub.
2. "Add new site / project" → selecciona este repositorio.
3. No necesita build command ni carpeta especial — es un sitio estático, `index.html` está en la raíz.
4. Una vez publicado, ve a la configuración de dominio del proyecto y conecta tu dominio propio (agregando los registros DNS que te indique Netlify/Vercel).

## Qué falta completar antes de publicar

Busca "TODO" dentro de `index.html`:

- **Fee de diseño** (sección "Cómo se cotiza tu proyecto"): definir el monto o rango real antes de publicar.
- **Mini brief**: el bloque `brief-frame-placeholder` debe reemplazarse por el `<iframe>` de inserción de tu Google Form real.
- **og:url / canonical**: agregar la URL final una vez el dominio esté conectado.

## Historial de cambios

1. Versión inicial con contenido real del portafolio (fotos, casos, FAQ) y WhatsApp como CTA principal.
2. Fondo animado tipo grid generativo en el Hero (primera impresión al entrar).
3. Endurecimiento de seguridad y escalabilidad: CSP, `noopener noreferrer`, cabeceras HTTP, semántica, lazy loading, SEO/OG, favicon embebido, y remoción de la nota interna visible.
