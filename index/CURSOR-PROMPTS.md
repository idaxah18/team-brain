---
tags: [meta, cursor]
status: evergreen
---

# Prompts para Cursor

## Otro dev: cargar contexto (sin Obsidian)

1. `git pull` en repo **team-brain**.
2. Cursor → **Open Folder** → carpeta `team-brain` (debe verse `AGENTS.md` en la raíz).
3. Agent:

```text
Palabra clave: formulario.
Sigue AGENTS.md: lee KEYWORDS y solo las notas enlazadas.
```

**Obsidian no es obligatorio**; solo hace falta el clone y Cursor.

## Guardar contexto de un chat para el equipo

```text
Resume este chat en projects/<slug>/contexto-FECHA.md (estado, variables, siguiente paso).
Actualiza projects/<slug>/overview.md.
En index/KEYWORDS.md deja una fila con la palabra clave del equipo (ej. formulario).
git commit y push en team-brain. El otro dev hace git pull y usa la misma palabra clave.

**Lucy webview:** `Palabra clave: lucy-webview. Sigue AGENTS.md.`
No incluyas secretos ni datos personales reales.
```

## Prueba actual — continuar backend del formulario

```text
Palabra clave: formulario.
Sigue AGENTS.md. Lee las variables nombres, correo, cedular y propón/implementa el backend en PyR/apps/.
```
