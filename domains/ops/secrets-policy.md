---
created: 2026-03-24
tags: [ops, security]
status: evergreen
---

# Política de secretos

## Reglas

- **Prohibido** API keys, tokens JWT de prod, contraseñas en notas del vault.
- Usar variables de entorno locales (`.env` en `.gitignore`) y gestor del equipo en VPS.
- En notas solo: nombres de variables (`OPENAI_API_KEY`), dónde configurarlas, rotación.

## Si el agente pide un secreto

Indicar al usuario que lo configure en `.env` local, nunca en Markdown.
