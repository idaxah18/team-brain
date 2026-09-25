---
tags: [project, lucy-webview, gea, jelou]
status: growing
updated: 2026-09-25
---

# Proyecto — Lucy Webview (formulario_fe_lucy)

## Objetivo

Reemplazar la **experiencia visual de WhatsApp** por una **webview embebida** (estilo app profesional, marca GEA) manteniendo la **misma lógica de pasos** del bot **Lucy** en Jelou. El cliente no debe sentir que “salió” del canal; menús, formularios y confirmaciones se ven como mini-app.

## Estado actual (resumen)

| Fase | Estado |
|------|--------|
| UI / UX webview (menús, formularios, stepper, tabs auth) | **Hecho** — navegable en local |
| Motor de flujo en frontend (~198 nodos, sin API) | **Hecho** — simulación completa |
| Integración APIs Jelou / GEA (asistencias, auth real, pagos, etc.) | **Pendiente** — siguiente hito del equipo |
| Despliegue / contrato con webview Jelou | **Por definir** |

## Repo de código (fuente de verdad del cómo)

| Dato | Valor |
|------|--------|
| Carpeta local (referencia) | `C:\Users\alvarezdx\Documents\CURSOR\formulario_fe_lucy` |
| Stack | Laravel **12** + Vue 3 + Vite + Tailwind |
| Entrada UI | `GET /` → SPA chat (`ChatView.vue`) |
| Health API | `GET /api/health` → `{ "ok": true, "service": "lucy-webview" }` |
| Jelou producción (solo lectura CLI) | `JELOU_PROJECT_ID=01j5661e5gaf6330435zh3bzjx` |

**No modificar flujos en Jelou** desde este repo hasta cerrar contrato API/webhook.

## URLs de prueba (frontend)

| URL | Uso |
|-----|-----|
| `/` | Flujo desde identificación (cédula → nombre → troncal → menús) |
| `/?flow=aseguradora` | Tras auth, salto al menú **Aseguradora** |
| `/?flow=hsm` | Simulador **HSM** (plantillas WhatsApp) |

Comandos globales en chat: `Menú principal`, `Empezar`.

## Documentación en el repo de código

- `docs/ARCHITECTURE.md` — capas Laravel/Vue
- `docs/FLOW_MAP.md` — mapa de interacciones y skills Jelou
- `docs/DESIGN_SYSTEM.md` — colores Gotham / GEA
- `docs/SETUP.md` — Laragon, PHP 8.3, `composer`, `npm run dev`, `php artisan serve` (**puerto 8000** por defecto)

## Palabra clave en Cursor

`lucy-webview` → leer esta nota y [[projects/lucy-webview/como-esta-hecho]], luego [[projects/lucy-webview/pendiente-integracion-apis]].

## Relacionado

- [[projects/lucy-webview/como-esta-hecho]]
- [[projects/lucy-webview/pendiente-integracion-apis]]
