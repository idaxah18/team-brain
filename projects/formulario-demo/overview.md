---
tags: [project, formulario]
status: growing
---

# Proyecto — Formulario demo

## Estado actual

| Fase | Estado |
|------|--------|
| Frontend (HTML/CSS/JS) | **Listo** — repo PyR `apps/formulario-demo/` |
| Backend (recibir datos del form) | **Pendiente** — siguiente paso del equipo |

## Variables del formulario (nombres exactos)

Usar **estos nombres** en frontend, backend y base de datos. No renombrar sin actualizar esta nota y `KEYWORDS`.

| Variable | Tipo en HTML | Descripción |
|----------|--------------|-------------|
| `nombres` | `text` | Nombre completo del usuario |
| `correo` | `email` | Correo electrónico |
| `cedular` | `tel` | Teléfono / cédula de contacto (campo numérico en UI) |

Atributos `name` e `id` en el formulario: `nombres`, `correo`, `cedular`.

## Siguiente trabajo (backend)

1. Endpoint que acepte `nombres`, `correo`, `cedular` (POST; JSON o `application/x-www-form-urlencoded`).
2. Validar en servidor (longitud, formato correo, patrón `cedular`).
3. Sustituir el “registro simulado” en `app.js` por llamada al API.
4. No guardar datos sensibles de prueba reales en notas; ver [[domains/ops/secrets-policy]].

## Código

- Frontend: `PyR/apps/formulario-demo/`
- Backend: por definir en `PyR` (ej. Laravel más adelante).

## Palabra clave en Cursor

`formulario` → lee esta nota y [[projects/formulario-demo/contexto-backend-pendiente]].
