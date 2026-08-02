# JYStreaming en Vercel

## 1. Despliega Code.gs como Web App (sin tocarlo)
1. Abre el proyecto en script.google.com
2. Implementar > Nueva implementación > tipo "Aplicación web"
3. Ejecutar como: **Yo (tu cuenta)**
4. Quién tiene acceso: **Cualquier usuario**
5. Copia la URL que termina en `/exec`

## 2. Pega la URL en index.html
Abre `index.html` y reemplaza:
```js
const API_URL = "PEGA_AQUI_TU_URL_DE_APPS_SCRIPT/exec";
```
por tu URL real.

## 3. Sube a Vercel
- Opción A (rápida): arrastra esta carpeta a vercel.com/new
- Opción B (CLI):
  ```
  npm i -g vercel
  vercel
  ```
No hace falta build ni framework: es HTML estático puro.

## Notas técnicas
- El index.html adaptado ya NO usa `google.script.run` (eso solo funciona
  dentro del iframe de Apps Script). En su lugar usa `fetch()` contra
  `doGet`/`doPost` de tu Code.gs, que ya tenían el modo REST con `?action=`.
- Los POST se envían con `Content-Type: text/plain` a propósito: así el
  navegador NO dispara una petición preflight (OPTIONS), que Apps Script
  no sabe responder y rompería todo con error de CORS.
- El admin token se sigue guardando en sessionStorage y viajando en el
  body de cada petición, igual que antes.
