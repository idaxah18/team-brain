---
tags: [project, formulario, contexto-chat]
status: seed
source: equipo
updated: 2026-03-24
---

# Contexto — nos quedamos en el backend

## Resumen de la conversación con IA

Estamos haciendo el **formulario de registro**. El frontend de demostración ya está hecho (tres campos). **Nos quedamos** en implementar el **backend** para **recibir y procesar** las variables enviadas desde el formulario.

## Variables acordadas

```text
nombres   → string, requerido
correo    → string email, requerido
cedular   → string teléfono/cédula, requerido (patrón similar al frontend)
```

Ejemplo cuerpo POST (referencia para el dev que continúa):

```json
{
  "nombres": "María López",
  "correo": "maria@ejemplo.com",
  "cedular": "3001234567"
}
```

## Qué debe hacer quien continúe

1. `git pull` en **team-brain** y **PyR**.
2. Abrir Cursor en `PyR` o `PyR.code-workspace`.
3. Prompt: `Palabra clave: formulario. Sigue AGENTS.md. Implementa el backend para las variables nombres, correo, cedular.`
4. Conectar `apps/formulario-demo/app.js` al endpoint cuando exista.

## Frontend actual

- Envío simulado en cliente; **no** hay servidor aún.
- Campos alineados: `name="nombres"`, `name="correo"`, `name="cedular"`.

## Relacionado

- [[projects/formulario-demo/overview]]
