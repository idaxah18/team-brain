# Segundo cerebro del área — guía para presentar

## Qué es (en una frase)

Un **carpetazo inteligente** en Markdown: el equipo escribe y enlaza notas; **Cursor** (con la IA de cada persona) lee las mismas reglas y **palabras clave** para no repetir contexto en cada chat.

**Obsidian** = ver, enlazar y explorar. **Git** = compartir entre PCs. **No hace falta** compartir cuenta de Cursor.

---

## Dos carpetas, dos roles

```text
team-brain/     →  QUÉ sabemos, QUÉ decidimos (notas)
PyR/            →  CÓMO lo programamos (código Laravel, etc.)
```

| Pregunta | Dónde mirar |
|----------|-------------|
| ¿Qué acordamos del formulario? | `team-brain` → `projects/...` |
| ¿Dónde está el código? | `PyR` |
| ¿Cómo le pido contexto a la IA? | Palabra clave + `KEYWORDS.md` |

En el repo de código, el archivo **`CONTEXT.md`** es la “tarjeta” que dice qué palabras clave y qué notas del vault usar.

---

## Mapa del vault (team-brain)

```text
team-brain/
  MOC-Home.md           ← puerta de entrada (índice humano)
  index/
    KEYWORDS.md         ← “router”: palabra → notas a leer
    CURSOR-PROMPTS.md   ← frases listas para pegar en Cursor
  inbox/                ← apuntes rápidos (cualquiera)
  projects/             ← un subcarpeta por producto (ej. formulario web)
  domains/              ← reglas estables (seguridad, forma de trabajar)
  templates/            ← plantilla de nota nueva
```

### Analogía para la audiencia

| Carpeta | Como si fuera… |
|---------|----------------|
| `inbox/` | Bandeja de ideas sin clasificar |
| `projects/` | Expediente de cada entrega |
| `domains/` | Manual del área (no cambia cada semana) |
| `KEYWORDS.md` | Índice del diccionario: “si dices X, lee estas páginas” |

---

## Demo en 5 minutos (guión)

### 1 — Obsidian (humanos)

1. Abrir **MOC-Home**.
2. Clic en **KEYWORDS** → mostrar una fila (ej. nombre del proyecto).
3. Abrir **Graph view**: se ven enlaces entre notas (el “mapa” del conocimiento).

### 2 — Registrar algo nuevo (sin magia)

1. Crear nota en `projects/mi-proyecto/overview.md` (objetivo y decisiones).
2. Añadir **una línea** en `index/KEYWORDS.md` con palabras que usará el equipo.
3. `git commit` + `git push` en **team-brain**.

### 3 — Otro compañero (otra cuenta Cursor)

1. `git clone` del repo **team-brain** (no necesita el código aún).
2. Abrir carpeta en **Cursor**.
3. En el Agent escribir:

```text
Palabra clave: <nombre-del-proyecto>. Sigue AGENTS.md.
```

4. La IA responde con lo que está en las notas — **sin** que le pasen PDFs por chat.

### 4 — Quien programa

1. Clona **PyR** + **team-brain** (o abre `PyR.code-workspace`).
2. Escribe código en `PyR`; actualiza decisiones en **team-brain**.

---

## Palabras clave (lo más importante)

No hay botón “guardar contexto del chat” en Cursor para el equipo. El acuerdo es:

1. **Resumir** el chat en una nota (`projects/.../contexto-....md`).
2. **Registrar** sinónimos en **KEYWORDS.md**.
3. **Subir** con Git.

Así el “cerebro” queda **visible y versionado**, no dentro de un chat privado.

---

## Seguridad (proyectos sensibles)

- **No** contraseñas ni datos personales en notas → ver `domains/ops/secrets-policy.md`.
- Repos **privados** en Git.
- Quien solo necesita contexto clona solo **team-brain**; quien codea clona **PyR**.

---

## Qué pedimos al área

| Sí | No |
|----|-----|
| Notas cortas y enlazadas | Un solo archivo de 50 páginas |
| KEYWORDS actualizado | “Está en el chat de ayer” |
| `git pull` al empezar el día | Editar sin sincronizar |

---

## Próximo paso técnico

- Proyecto **formulario web (Laravel)** en `PyR`.
- Contexto del formulario en `team-brain/projects/<nombre>/`.
- Compartir con otra máquina: `git remote` en VPS o GitHub + `clone` / `pull`.

Documento técnico mínimo para devs: `README.md` en la raíz de cada repo.
