---
tags: [project, lucy-webview, lucy_webview, omniax, apis]
status: growing
updated: 2026-09-25
keyword: lucy_webview
source: PDFs GEA Abr-2025 + diagramas agendar/reagendar + PDF GEA chatbot 2024-03-15
---

# Omniax — médico / dental (API test-ec)

## Autenticación

- **Base test:** `https://api.geainternacional.com/test-ec/`
- **Token:** `POST /v1/auth/token` body `{ client_id, client_secret }` → `data.access_token` (Bearer).
- Credenciales reales: **solo** en `.env` del repo `formulario_fe_lucy` (`GEA_OMNIAX_*`), nunca en este vault.

## Respuesta estándar

Casi todos los endpoints devuelven `{ estado, noticias: { titulo, mensaje }, data }`. Errores: 422 validación, 400 negocio (`mensaje_catch`).

## Catálogo de servicios (orden lógico)

| # | Nombre | Método | Ruta |
|---|--------|--------|------|
| 0 | Login | POST | `/v1/auth/token` |
| 1 | Asistencias en proceso | POST | `/v1/chatbot/medico-dental/asistencias/en-proceso` |
| 2 | Especialidades (solo médico) | GET | `/v1/chatbot/medico/especialidades` |
| 3 | Aplica asignación establecimiento (médico) | POST | `/v1/chatbot/medico/establecimientos/aplica-asignacion` |
| 4 | Aplica asignación establecimiento (dental) | POST | `/v1/chatbot/dental/establecimientos/aplica-asignacion` |
| 5 | Establecimientos (proximidad o zona) | POST | `/v1/chatbot/medico-dental/establecimientos` |
| 6 | Zonas por ciudad (“Otra ubicación”) | GET | `/v1/chatbot/zonas-por-ciudad?id_servicio={id}` |
| 7 | Días disponibles (médico) | POST | `/v1/chatbot/medico/establecimientos/disponibilidad-dias` |
| 8 | Días disponibles (dental) | POST | `/v1/chatbot/dental/establecimientos/disponibilidad-dias` |
| 9 | Horas disponibles (médico) | POST | `/v1/chatbot/medico/establecimientos/disponibilidad-horas` |
| 10 | Horas disponibles (dental) | POST | `/v1/chatbot/dental/establecimientos/disponibilidad-horas` |
| 11 | Crear asistencia | POST | `/v1/chatbot/medico-dental/asistencias` |
| 12 | Reagendar cita | POST | `/v1/chatbot/medico-dental/asistencias/reagendar` |

## Variables de contexto (chatbot)

| Variable | Origen |
|----------|--------|
| `identificacion_titular` | Auth cédula |
| `nombre_titular` | Auth nombre |
| `telefono` | Canal / webview (sin prefijo país) |
| `id_servicio` | Elección Médico vs Dental (ej. doc usa `297`; confirmar IDs en implementación) |
| `para_reagendar` | `true` solo flujo reagendar (servicio 1) |
| `aplica_seguimiento_dental` | Respuesta servicio 1 → rama ubicación + creación (servicio 11) |
| `identificacion_beneficiario`, `nombre_beneficiario`, `edad_beneficiario`, `sexo_beneficiario`, `parentesco_beneficiario` | Flujo “Para beneficiario” |
| `id_especialidad` | Servicio 2 / asistencia seleccionada (reagendar) |
| `id_establecimiento` | Servicio 5 |
| `latitud`, `longitud` | GPS (obligatorios si `id_zona` nulo) |
| `id_zona` | Servicio 6 (obligatorio si no hay lat/long) |
| `fecha` | `valor` del día elegido (servicios 7/8) |
| `hora` | Texto hora elegida (servicios 9/10) |
| `id_asistencia` | Servicio 1 / reagendar / opcional en 7–10 |

## Ramas clave (agendar — diagrama)

1. Servicio **1** (`para_reagendar: false`) → si hay asistencias en proceso: menú + “¿generar nueva?”; si `aplica_seguimiento_dental`: ubicación → servicio **11** con flags; si no: flujo asistencias en proceso legacy.
2. **Para mí / Beneficiario** → beneficiario: datos extra + parentesco/sexo.
3. Médico: servicio **2** especialidades → servicio **3** aplica asignación → si true: ubicación o zona (5/6) → fechas **7** → horas **9** (paginar 9 + “Más opciones”) → **11**.
4. Dental: servicio **4** → misma lógica con **8** y **10**.
5. Servicio **5** dos veces: con lat/long sin `id_zona`; segunda vez con `id_zona` tras “Otra ubicación”.
6. Sin fechas/horas: mensaje “No existen fechas disponibles” / elegir otro establecimiento.

## Ramas clave (reagendar — diagrama)

1. Servicio **1** con `para_reagendar: true` → listado asistencias (`detalle` como label).
2. Guardar `id_asistencia`, `id_especialidad`, `id_establecimiento` de la opción.
3. Fechas **7 u 8** (con `id_asistencia`) → horas **9 u 10** (paginar) → servicio **12**.
4. Array vacío en 9/10 = sin horarios → mensaje documentado en servicio 12.

## Pruebas QA (documento API)

- Cédula afiliado prueba: `0979461382`; otras cédulas para sin afiliación.
- Proveedor: **PROVEEDOR JELOU MEDICO DENTAL** (horario L–V 08–12, 13–17).
- Ubicación recomendada: `-2.164525162397198`, `-79.89574580754577`.
- Especialidad **MEDICINA GENERAL** como titular para citas sin límite de eventos.
- Flujo zona: ciudad **Guayaquil**.

## Implementación prevista (repo código)

1. Laravel: `GeaOmniaxClient` (token cacheado) + `routes/api/v1/omniax/*` proxy.
2. Vue: sustituir wizards simulados de agendar/reagendar médico/dental por llamadas API + menús dinámicos.
3. Paginación horas: máx. 9 + botón “Más opciones” / “Otro”.

## Relacionado

- [[projects/lucy-webview/overview]]
- [[projects/lucy-webview/pendiente-integracion-apis]]
