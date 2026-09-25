---
tags: [project, lucy-webview, lucy_webview, arquitectura, ui]
status: growing
updated: 2026-09-25
keyword: lucy_webview
---

# lucy_webview — cómo está hecho

## Arquitectura en una frase

WhatsApp (Jelou) abre **webview** → Laravel sirve SPA → **Vue chat** ejecuta grafo de nodos → llamadas **`/api/v1/omniax/*`** → **Omniax** (GEA).

```text
Navegador / Webview Jelou
  → Laravel routes/web.php (fallback SPA)
  → resources/views/app.blade.php
  → Vite → ChatView.vue
  → lucyChatEngine.js + lucyFlowGraph.js (~267 nodos)
  → omniax*Api.js → Laravel Omniax*Controller → OmniaxClient
```

## Motor de conversación (frontend)

| Archivo | Rol |
|---------|-----|
| `lucyFlowGraph.js` | Nodos `say`, `actions`, `input`, `jelou`, handlers GEA/Omniax |
| `lucyChatEngine.js` | Reducer + eventos `geaEnter` / `geaResult` / `geaError` |
| `flowHelpers.js` | Cadenas asistencia (ubicación → dirección → POST GEA) |
| `flows/lucy/omniax/*` | Runners médico/dental (agendar, reagendar, crear) |
| `flows/lucy/gea/*` | GEA: `geaRunner`, `geaNodes`, `geaServiceIds`, `geaEncuesta` |
| `hsmCatalog.js`, `uiMeta.js`, `flowStepper.js` | HSM simulado, dock, stepper |

## APIs JS → Laravel

| Cliente | Prefijo API |
|---------|-------------|
| `omniaxMedicoApi.js` | `/api/v1/omniax/medico` |
| `omniaxDentalApi.js` | `/api/v1/omniax/dental` |
| `omniaxGeaApi.js` | `/api/v1/omniax/gea` |

## Backend

| Archivo | Rol |
|---------|-----|
| `OmniaxClient.php` | Token, GET/POST/PUT, refresh 401 |
| `OmniaxGeaController.php` | PDF GEA chatbot |
| `OmniaxMedicoController.php` / `OmniaxDentalController.php` | PDF servicios 1–12 |
| `WebviewController.php` | Shell SPA |
| `config/services.php` | `gea_omniax.*` |

`routes/api.php`: health + tres prefijos Omniax.

## UI

Componentes en `resources/js/components/chat/` — header Lucy/GEA, stepper, auth tabs, dock menú, overlay menús largos. Ver notas UX en versión anterior del doc (sin cambio de principio).

## Stubs vs real

~**44** hojas `stubRegistered` (IA, siniestros, VIP, E-Doctor, etc.). Flujos con API: dental/médico Omniax, crear GEA hogar/vial/ambulancia/médico domicilio/aseguradora, cancelar, en curso, encuesta/ubicación/utilidades GEA.

## Jelou referencia

`jelou-lucy-ecuador-observe` — 117 workflows; skill crear GEA **4220**. Webview **no** usa tool Auth 8505.

## Continuación

[[projects/lucy-webview/HANDOFF-CONTINUATION]] y `docs/HANDOFF.md` en el repo.
