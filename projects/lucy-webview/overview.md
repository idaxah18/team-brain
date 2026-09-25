---
tags: [project, lucy-webview, lucy_webview, gea, jelou]
status: active
updated: 2026-09-25 (fases 0–4 + integraciones .env)
keyword: lucy_webview
---

# Proyecto — Lucy Webview (`lucy_webview`)

## Objetivo

WhatsApp (Jelou) abre **webview** → usuario completa el flujo en **SPA chat** (marca GEA) → cierra y vuelve a WhatsApp. Negocio vía **Omniax API** (proxy Laravel), no ejecutando skills Jelou dentro del navegador.

## Estado (2026-09-25)

| Área | Estado |
|------|--------|
| Fases 0–4 (tools, ASAP, comercial, IA, webview close en código) | **Hecho** — detalle en [[projects/lucy-webview/HANDOFF-CONTINUATION]] |
| Grafo ~324 nodos + integraciones `.env` | **Hecho** |
| Teléfono QA + cabina citas médico/dental | **Hecho** |
| Cierre webview ↔ skill Jelou en WA | **Pendiente** configuración Jelou |
| Deploy VPS + push GitHub | **Pendiente** — [[projects/lucy-webview/GITHUB-PUSH]] |
| Paridad total ~98 tools Jelou | **Parcial** — ver `JELOU_PARITY_ROADMAP.md` en repo |

## Repo

| Dato | Valor |
|------|--------|
| Carpeta | `C:\Users\alvarezdx\Documents\CURSOR\formulario_fe_lucy` |
| Git | Commit inicial `b222f90` en `main` (130 archivos) |
| GitHub | **Pendiente push** — pasos en [[projects/lucy-webview/GITHUB-PUSH]] (`gh` no instalado en máquina dev) |
| Stack | Laravel 12 + Vue 3 + Vite + Tailwind |
| Entrada | `GET /` → `ChatView.vue` |
| Health | `GET /api/health` |
| Handoff dev | `docs/HANDOFF.md` en el repo |

## Palabras clave Cursor

- **`lucy_webview`** (principal)
- `lucy-webview` (alias)

**Levantar en Laragon (otro dev):** [[projects/lucy-webview/LEVANTAR-LARAGON]] (copiar/pegar comandos).

Leer: [[projects/lucy-webview/HANDOFF-CONTINUATION]] → [[projects/lucy-webview/como-esta-hecho]] → [[projects/lucy-webview/pendiente-integracion-apis]].

## URLs prueba local

| URL | Uso |
|-----|-----|
| `/?telefono=0999999999` | Teléfono Omniax |
| `/?flow=aseguradora` | Menú aseguradora |
| `/?flow=hsm` | Simulador HSM |

Cédula QA Omniax: `0979461382`.

## Jelou

`JELOU_PROJECT_ID=01j5661e5gaf6330435zh3bzjx` — no modificar flujos prod desde este repo sin acuerdo.
