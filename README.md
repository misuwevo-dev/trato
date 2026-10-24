# trato.

**Empleo honesto. Sin vueltas.**

Plataforma de empleos para Ecuador, con foco inicial en Guayaquil. V1 estática: un solo `index.html` con CSS y JavaScript vanilla integrados, sin frameworks, sin backend y sin base de datos externa.

## Archivos

```
.
├── index.html          # Aplicación completa (CSS + JS inline)
├── manifest.json       # Manifest PWA
├── sw.js               # Service worker (app shell)
├── vercel.json         # Headers para sw.js y manifest.json
├── .gitignore
├── icons/
│   ├── icon.svg
│   ├── icon-192.png
│   ├── icon-512.png
│   └── apple-touch-icon.png
└── app.js              # Fuente previa al build inline (no la carga index.html)
```

`index.html` es el entregable y el único archivo que necesita el navegador. `app.js` se conserva como referencia de desarrollo; si lo editas, vuelve a generar el HTML inline.

## Uso local

Abre `index.html` directamente o sirve la carpeta con cualquier servidor estático:

```bash
npx serve .
```

El service worker y la instalación PWA requieren `http://localhost` o HTTPS; con `file://` la app funciona, pero sin PWA.

## Funcionalidades V1

- **Salario obligatorio**: no se publica una oferta sin salario mínimo y máximo válidos.
- **Flash Jobs**: ofertas urgentes de 24–48 h, marcadas visualmente.
- **Oferta destacada**: checkbox `$5 (demo)`; aparece primero en el tablero.
- **Reportes**: botón `🚩 Reportar` en cada oferta, motivo por desplegable y contador por empresa.
- **Anti-tóxico**: al llegar a 3 reportes la empresa muestra `🚫 BLOQUEADA`, pierde el contacto y no puede publicar.
- **Karma de empresa**: inicia en 100, +2 al publicar, −15 por reporte.
- **Perfil de talento**: nombre, nivel 1–10, habilidades, experiencia y CV opcional (PDF/DOC, máx. 1.5 MB).
- **Verificación**: al subir CV se muestra `✅ Verificado` y el karma del trabajador sube a 105.
- **Enlaces opcionales**: oferta original (LinkedIn/CompuTrabajo) y sitio web/Instagram de la empresa.
- **Contacto directo**: email o WhatsApp, sin intermediarios.
- **Publicidad**: franja de patrocinio local sobre el footer.
- **PWA**: instalable en móvil, con manifest, iconos y caché offline del app shell.

## Datos y privacidad

Todo se guarda en `localStorage` del navegador:

| Clave | Contenido |
| --- | --- |
| `trato.jobs.v1` | Ofertas publicadas |
| `trato.profile.v1` | Perfil del trabajador y CV en base64 |
| `trato.reports.v1` | Reportes por empresa |
| `trato.companyKarma.v1` | Karma por empresa |

Los reportes y el karma son locales a cada navegador. No hay moderación central ni cuentas de usuario en esta V1. El CV se guarda como data URL y no se sube a ningún servidor.

## Variables de entorno

No se necesitan. El proyecto es 100 % estático.

## Deploy en Vercel

1. Sube el proyecto a un repositorio de GitHub/GitLab/Bitbucket.
2. En Vercel, elige **Add New → Project** e importa el repositorio.
3. Framework Preset: **Other**. Build Command: vacío. Output Directory: `.`.
4. Deploy. Vercel publica `index.html`, `manifest.json`, `sw.js`, `vercel.json` e `icons/`.
5. Abre la URL HTTPS y verifica la instalación PWA desde el navegador móvil.

Si cambias archivos cacheados, sube el nombre de `CACHE_NAME` en `sw.js` (por ejemplo `trato-v2`) para invalidar la caché anterior.

## Limitaciones conocidas

- Sin backend: las ofertas, reportes y karma viven solo en el navegador.
- Los reportes no se deduplican por usuario en V1.
- El pago de ofertas destacadas es simulado (`$5 demo`).
- El CV se almacena en base64 y está limitado a 1.5 MB.
- No hay autenticación ni panel de administración.
