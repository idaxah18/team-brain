---
tags: [project, lucy-webview, apis, backlog]
status: seed
updated: 2026-09-25
---

# Lucy Webview — pendiente para cerrar el flujo real

## Resumen

El **recorrido UI** del bot está **navegable al 100 % en simulación**. Falta **conectar cada tipo de paso** al mismo efecto que hoy tiene el bot en WhatsApp/Jelou (APIs GEA, webhooks, sesión webview).

## Prioridad sugerida (integración)

### 1. Fundación API (Laravel)

- [ ] Definir prefijo `api/v1` y autenticación (token de webview / sesión Laravel).
- [ ] Servicios `app/Services/Jelou/` o `app/Services/Gea/` — **no** lógica de negocio en controladores.
- [ ] Variables `.env`: `JELOU_PROJECT_ID`, credenciales API (ver [[domains/ops/secrets-policy]] — **no** pegar secretos en notas).
- [ ] Sustituir stubs del motor por llamadas HTTP desde Vue (`axios` → `/api/...`) o eventos server-driven.

### 2. Identificación y contexto de usuario

| Paso UI actual | Skill Jelou | Integración pendiente |
|----------------|-------------|------------------------|
| Cédula 10 dígitos | Proteccion datos cedula | Validar/consultar identidad en backend |
| Nombre | Proteccion datos nombre | Persistir en sesión / perfil |
| Contexto `skipToAseguradora` | Troncal | Reglas reales de línea de negocio |

### 3. Crear asistencia (patrón más repetido)

Cadena actual en UI: **menú servicio → ubicación (GPS) → dirección texto → confirmación simulada**.

| Skill ref. | API / acción real |
|------------|-------------------|
| V2 Crear asistencia | POST crear caso con coords + dirección + placa si aplica |
| asistencia ubicación | Equivalente al nodo LOCATION de WhatsApp |

Nodos generados por `crearAsistenciaChain()` en `flowHelpers.js` — un solo contrato API puede cubrir muchas etiquetas (grúa, plomero, etc.) con `serviceType` en payload.

### 4. Aseguradora y siniestros

- Placa (`GYE1234` validada en cliente).
- Ramas siniestro (colisión, robo, fotos, biometría) — hoy son textos/wizards simulados.
- Endpoints alineados a skills `V2 Aseguradora - Inicio`, `Siniestro`, etc.

### 5. Citas y wizards (`wiz_*`)

- Wizards multi-paso (agendar cita dental/médica): cada `input` debería mapear a campo API o a skill Jelou.
- Stepper UI ya refleja pasos; falta **persistir datos** entre pasos en `state.context` + backend.

### 6. Comercial y pagos

- E-doctor, Compra y viaja, Asistencia inmediata: pasos de registro/pago son **input simulado**.
- Integrar pasarela / solicitud de pago según skills `Activar e-doctor - ...`.

### 7. HSM

- Simulador en `/?flow=hsm` muestra plantillas y botones.
- Producción: disparo/recibir HSM vía API Jelou, no solo catálogo local `hsmCatalog.js`.

### 8. Operación y errores

- Mantenimiento, error general, derivación asesor: mensajes fijos hoy; pueden venir de feature flags o API de estado.

## Cambios de código esperados (orientación al otro dev)

1. **`lucyChatEngine.js`**: en lugar de solo `enterNode` local, nodos con flag `api: 'createAssistance'` (o tabla en grafo) disparan `fetch` y avanzan según respuesta.
2. **`routes/api.php`**: rutas por dominio (`aseguradora`, `asistencias`, `auth`).
3. **`ChatView.vue`**: loading/error global para llamadas async; mantener UX (stepper, overlay menú).
4. **Tests**: al menos contract tests en Laravel + pruebas manuales por `FLOW_MAP.md`.

## Criterio de “flujo completo” (aceptación)

Para un servicio piloto (ej. **Grúa aseguradora** o **Grúa 24/7**):

1. Usuario identificado (API real o sesión de prueba acordada).
2. Selección menú → ubicación real → dirección.
3. Backend crea solicitud y devuelve ID / mensaje de confirmación.
4. UI muestra confirmación **sin** texto “Simulación” de `stubRegistered`.
5. Opcional: consulta “asistencias en curso” con datos reales.

Repetir patrón para el resto de skills según prioridad de negocio.

## Cómo arrancar en Cursor (otro dev)

1. `git pull` en **team-brain**.
2. Clonar/abrir repo **formulario_fe_lucy** (misma carpeta padre `CURSOR/` que team-brain).
3. Prompt: `Palabra clave: lucy-webview. Sigue AGENTS.md en team-brain. Integra API para [servicio piloto].`
4. Leer `docs/FLOW_MAP.md` en el repo de código para el skill exacto.

## Relacionado

- [[projects/lucy-webview/overview]]
- [[projects/lucy-webview/como-esta-hecho]]
