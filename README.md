# Neomech 3D — Landing page

Landing interactiva de Neomech 3D (diseño, ingeniería y fabricación digital con impresión 3D). Sitio estático de una sola página — sin frameworks, sin build step.

## Estructura

```
neomech-landing/
├── index.html      → toda la página (HTML + CSS + JS en un solo archivo)
└── assets/         → fotos reales del portafolio
```

## Cómo subir esto a GitHub

1. Crea un repositorio nuevo y vacío en https://github.com/new (sin README, sin .gitignore).
2. En tu computador, abre una terminal dentro de esta carpeta (`neomech-landing`) y ejecuta:

```bash
git remote add origin https://github.com/TU-USUARIO/TU-REPO.git
git branch -M main
git push -u origin main
```

Este proyecto ya viene con `git init` hecho y el primer commit listo — solo falta conectarlo a tu repo de GitHub con esos tres comandos.

Si prefieres no usar la terminal: en GitHub, entra al repo vacío → "uploading an existing file" → arrastra `index.html` y la carpeta `assets/`.

## Cómo publicarlo (dominio propio)

Con el repo en GitHub, conéctalo a **Netlify** o **Vercel** (los dos tienen plan gratis):

1. Crea cuenta en [netlify.com](https://netlify.com) o [vercel.com](https://vercel.com) con tu cuenta de GitHub.
2. "Add new site / project" → selecciona este repositorio.
3. No necesita build command ni carpeta especial — es un sitio estático, `index.html` está en la raíz.
4. Una vez publicado, ve a la configuración de dominio del proyecto y conecta tu dominio propio (agregando los registros DNS que te indique Netlify/Vercel).

## Qué falta completar antes de publicar

Busca estos puntos marcados dentro de `index.html`:

- **Fee de diseño** (sección "Cómo se cotiza tu proyecto"): hay una nota interna para definir el monto o rango real antes de publicar.
- **Mini brief**: el bloque `brief-frame-placeholder` debe reemplazarse por el `<iframe>` de inserción de tu Google Form real.

## Últimos cambios aplicados

- Fondo animado tipo grid generativo en el Hero (primera sección que ve el usuario al entrar), con efecto de pulso desde el centro y brillo que sigue el mouse — inspirado en el componente `DataGridHero`, adaptado a HTML/CSS/JS puro para no depender de React.
