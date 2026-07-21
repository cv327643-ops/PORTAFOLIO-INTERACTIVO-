# Neomech 3D — Landing page

Landing interactiva de Neomech 3D (diseño, ingeniería y fabricación digital con impresión 3D). La página en sí es **un solo documento** (`index.html`, con su CSS y JS adentro) — sin frameworks, sin build step. Pensado para desplegar en **Railway**.

## Estructura

```
neomech-landing/
├── index.html      → la página completa: HTML + CSS + JS + IMÁGENES, todo en un solo archivo
├── assets/         → fotos originales (se conservan como archivo fuente, ya no las usa la página)
└── Caddyfile       → configuración del servidor + cabeceras de seguridad (Railway)
```

**Las imágenes ya no son archivos aparte.** Están embebidas dentro de `index.html` como `data:image/jpeg;base64,...` en cada `<img>`. Esto se hizo para resolver que las imágenes no cargaban al desplegar — evita depender de que la carpeta `assets/` se suba completa y con las rutas relativas intactas: con `index.html` solo, ya funciona todo. La carpeta `assets/` se deja en el repo únicamente como respaldo de las fotos originales, por si necesitas reemplazar alguna en el futuro.

Contrapartida: al embeber las imágenes, `index.html` pesa ~2.3 MB (antes pesaba ~46 KB y las fotos se cargaban aparte). Caddy comprime con gzip/zstd en tránsito, así que el peso real que viaja por la red es menor, pero si más adelante quieres aligerar el sitio, se puede optimizar comprimiendo las fotos o volviendo a servirlas como archivos aparte una vez confirmemos que las rutas quedan bien configuradas.

## Por qué hay un Caddyfile

Railway detecta automáticamente un sitio estático cuando encuentra un `index.html` en la raíz del repo (cero configuración) y lo sirve con **Caddy** por debajo. Ese `Caddyfile` es la única forma de fijar cabeceras de seguridad HTTP reales (un archivo HTML no puede hacerlo por sí solo) — reemplaza lo que en Netlify sería `_headers` o en Vercel `vercel.json`, para que todo quede alineado con Railway.

## Seguridad aplicada

Dentro de `index.html`:
- **Content-Security-Policy** vía meta tag: bloquea scripts/estilos de terceros no autorizados, solo permite el iframe del mini brief desde `docs.google.com`.
- Todos los enlaces que abren en pestaña nueva (`target="_blank"`) llevan `rel="noopener noreferrer"` — evita el hueco de seguridad conocido como *tabnabbing*.
- Se quitó del contenido visible la nota interna del fee de diseño (antes se veía en la página pública); ahora es un comentario HTML, invisible para los visitantes.
- `object-src 'none'` y `frame-ancestors 'self'` reducen superficie de ataque.

En el `Caddyfile` (cabeceras que solo se pueden fijar desde el servidor, no desde el HTML):
- `X-Frame-Options` + `frame-ancestors`: evita que el sitio se cargue dentro de un iframe ajeno (clickjacking).
- `X-Content-Type-Options: nosniff`: evita que el navegador "adivine" tipos de archivo.
- `Strict-Transport-Security`: fuerza HTTPS en todas las visitas futuras.
- `Referrer-Policy` y `Permissions-Policy`: limitan qué información se comparte al salir del sitio y qué permisos del navegador puede pedir la página (cámara, micrófono, ubicación — ninguno).
- El mismo `Content-Security-Policy` del meta tag, para que ambas capas queden alineadas.
- `hide .git`, `hide Caddyfile`, `hide README.md`: nadie puede descargar esos archivos desde el navegador, solo se sirve lo público (`index.html` y `assets/`).

## Escalabilidad / rendimiento

- Imágenes con `width`/`height` fijos (evita saltos de layout) y `loading="lazy"` en todo lo que no sea la primera pantalla — la imagen del hero carga con prioridad alta por ser lo primero que se ve.
- Estructura semántica (`header`, `main`, `footer`) para accesibilidad y mejor indexación en buscadores.
- Meta tags de SEO (`description`, `robots`) y de redes sociales (Open Graph, Twitter Card) para que el link se vea bien al compartirlo por WhatsApp, Instagram o LinkedIn.
- Favicon incluido como SVG embebido (no necesita un archivo aparte).
- Compresión gzip/zstd activada en el `Caddyfile`.
- Railway sirve el sitio con build y deploy automáticos en cada push, SSL automático, y escala sin configuración extra.

## Cómo subir esto a GitHub

1. Crea un repositorio nuevo y vacío en https://github.com/new (sin README, sin .gitignore).
2. En tu computador, abre una terminal dentro de esta carpeta (`neomech-landing`) y ejecuta:

```bash
git remote add origin https://github.com/TU-USUARIO/TU-REPO.git
git branch -M main
git push -u origin main
```

Este proyecto ya viene con `git init` hecho y los commits listos — solo falta conectarlo a tu repo de GitHub con esos tres comandos.

Si prefieres no usar la terminal: en GitHub, entra al repo vacío → "uploading an existing file" → arrastra `index.html`, `Caddyfile` y la carpeta `assets/`.

## Cómo desplegarlo en Railway

1. Entra a [railway.com/new](https://railway.com/new) con tu cuenta.
2. Elige **"Deploy from GitHub repo"** y selecciona este repositorio.
3. Railway detecta automáticamente que es un sitio estático (por el `index.html` en la raíz) y lo despliega — no hay que configurar build command ni start command.
4. En el servicio ya desplegado, ve a **Settings → Networking → Custom Domain**, agrega tu dominio propio y configura el CNAME que te indique Railway en tu proveedor de DNS. El certificado SSL se emite automáticamente.

## Qué falta completar antes de publicar

Busca "TODO" dentro de `index.html`:

- **Fee de diseño** (sección "Cómo se cotiza tu proyecto"): definir el monto o rango real antes de publicar.
- **Mini brief**: el bloque `brief-frame-placeholder` debe reemplazarse por el `<iframe>` de inserción de tu Google Form real.
- **og:url / canonical**: agregar la URL final una vez el dominio esté conectado en Railway.

## Historial de cambios

1. Versión inicial con contenido real del portafolio (fotos, casos, FAQ) y WhatsApp como CTA principal.
2. Fondo animado tipo grid generativo en el Hero (primera impresión al entrar).
3. Endurecimiento de seguridad y escalabilidad: CSP, `noopener noreferrer`, semántica, lazy loading, SEO/OG, favicon embebido, remoción de la nota interna visible.
4. Alineado con Railway: se reemplazan `_headers`/`vercel.json` (Netlify/Vercel) por un `Caddyfile` — el mecanismo real que usa Railway para servir sitios estáticos y fijar cabeceras de seguridad.
5. Imágenes embebidas como `data:` URI directamente en `index.html`, para que dejen de depender de que la carpeta `assets/` se suba/despliegue correctamente por separado.
