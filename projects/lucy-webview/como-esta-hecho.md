---
tags: [project, lucy-webview, arquitectura, ui]
status: growing
updated: 2026-09-25
---

# Lucy Webview — cómo está hecho

## Arquitectura en una frase

Laravel sirve un **shell HTML único**; Vue Router monta **un solo chat** que ejecuta un **grafo de nodos** en JavaScript (equivalente a pasos del bot), sin llamadas backend de negocio todavía.

```text
Navegador / Webview Jelou
  → Laravel `routes/web.php` (fallback SPA)
  → `resources/views/app.blade.php`
  → Vite → `resources/js/app.js` → `ChatView.vue`
  → `lucyChatEngine.js` + `lucyFlowGraph.js`
```

## Motor de conversación (frontend)

| Archivo | Rol |
|---------|-----|
| `resources/js/flows/lucy/lucyFlowGraph.js` | ~**198 nodos** (`say`, `actions`, `input`, `jelou: 'Nombre skill'`) |
| `resources/js/flows/lucy/lucyChatEngine.js` | Reducer: texto, botones, ubicación, comandos globales |
| `resources/js/flows/lucy/flowHelpers.js` | Cadenas asistencia (ubicación → dirección → stub), wizards |
| `resources/js/flows/lucy/hsmCatalog.js` | Catálogo simulador HSM |
| `resources/js/flows/lucy/uiMeta.js` | Textos dock, formularios, ocultar duplicados en historial |
| `resources/js/flows/lucy/flowStepper.js` | Pasos visibles en stepper (auth, asistencia, e-doctor, wizards) |

Cada nodo puede tener:

- **`say`**: mensajes del bot (en UI muchos se muestran solo en el panel inferior, no duplicados arriba).
- **`actions`**: menú de opciones.
- **`input`**: campo (cédula, nombre, placa, texto libre) con validación en cliente.
- **`jelou`**: nombre del skill Jelou de referencia.

## UI (ya no es WhatsApp)

Componentes principales en `resources/js/components/chat/`:

| Componente | Uso |
|------------|-----|
| `ChatHeader` | Banner GEA, **Lucy**, slogan, avatar (`/images/lucy-avatar.png` o SVG fallback), punto en línea |
| `ChatFlowStepper` | Debajo del header — progreso tipo Nuxt UI Stepper (inspirado, implementación propia) |
| `ChatAuthTabs` | Cédula + nombre con **tabs animados** (Enter / Continuar avanza pestaña) |
| `ChatFormPanel` | Formularios de un campo (placa, dirección, etc.) |
| `ChatDockIntro` + `ChatMenuList` | Pregunta + lista con iconos y colores GEA |
| `ChatNarrative` / `ChatUserChip` | Historial estilo app |
| `ChatMenuList` | Animación al elegir opción (~380 ms) antes de navegar |

Comportamiento UX acordado:

- Menú **muy largo** (> **75 %** alto viewport): panel inferior en **overlay** (sin “cachito” de chat scrolleable arriba).
- Mensaje del paso activo: **solo en el dock** (formulario/menú), historial superior sin repetir el mismo texto.
- Marca: bot **Lucy** (no “Lucy Ecuador Aseguradora” en cabecera).

## Layout

- `resources/js/layouts/ChatShell.vue` — header + slot stepper + contenido
- `resources/js/modules/chat/views/ChatView.vue` — estado, geolocalización, overlay menú
- Responsive: `max-w-md`, safe areas móvil (sin romper el shell)

## Backend Laravel (hoy)

- `app/Http/Controllers/WebviewController.php` — sirve SPA
- `routes/api.php` — solo **`/api/health`**; rutas v1 comentadas como ejemplo
- Sesión/caché en archivo + SQLite local (`.env` típico del proyecto)

## Observación Jelou (local, opcional)

Carpeta de referencia usada para mapear skills (pull CLI, **no push** al bot):

`jelou-lucy-ecuador-observe` — ~117 workflows.

## Qué **no** está cableado aún

- Crear asistencia real, PMA, pagos, fotos, biometría, HSM envío real.
- Auth cédula/nombre contra servicio GEA (solo validación formato en cliente).
- Sustituir `stubRegistered()` por respuesta API + estado en sesión.

Ver [[projects/lucy-webview/pendiente-integracion-apis]].
