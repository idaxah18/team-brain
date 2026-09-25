---
tags: [project, lucy-webview, lucy_webview, git]
updated: 2026-09-25
keyword: lucy_webview
---

# Subir `formulario_fe_lucy` a GitHub (repo vacío ya creado)

El repo en GitHub **ya existe pero está vacío**. En local hay historial en `main` **más muchos cambios sin commit** (fases 0–4, integraciones, teléfono QA, cabina citas).

## Antes de push — comprobar que no subes secretos

```powershell
cd C:\Users\alvarezdx\Documents\CURSOR\formulario_fe_lucy

git status
# NO debe aparecer .env ni Credenciales.txt (están en .gitignore)

git check-ignore -v .env
# debe listar .gitignore
```

Si `.env` apareciera en `git status`, **no hagas commit** — quítalo del índice: `git rm --cached .env`.

---

## Paso 1 — Commit de todo el código (carpeta del proyecto)

```powershell
cd C:\Users\alvarezdx\Documents\CURSOR\formulario_fe_lucy

git add -A
git status
# Revisar que solo entren archivos de código/docs, no .env

git commit -m "$(cat <<'EOF'
lucy_webview: fases 0-4, integraciones .env, teléfono QA y cabina citas.

Centraliza credenciales en .env.example, middleware de integraciones, flujos comercial/IA/ASAP, paso auth_telefono y ajuste cabina para agendar médico/dental.
EOF
)"
```

En **PowerShell** sin heredoc bash, usa mensaje en una línea:

```powershell
git commit -m "lucy_webview: fases 0-4, integraciones, teléfono QA y cabina citas médicas"
```

---

## Paso 2 — Enlazar remoto (solo la primera vez)

Sustituye `TU_USUARIO` y `TU_REPO` por la URL real del repo vacío en GitHub.

```powershell
git remote add origin https://github.com/TU_USUARIO/TU_REPO.git
```

Si ya tenías un `origin` mal configurado:

```powershell
git remote remove origin
git remote add origin https://github.com/TU_USUARIO/TU_REPO.git
```

Comprobar:

```powershell
git remote -v
```

---

## Paso 3 — Subir a GitHub

```powershell
git branch -M main
git push -u origin main
```

Si GitHub pide login: PAT (token) o `gh auth login`.

---

## Opción con GitHub CLI (crear repo + push en uno)

Si **aún no** creaste el repo en la web:

```powershell
winget install GitHub.cli
gh auth login
cd C:\Users\alvarezdx\Documents\CURSOR\formulario_fe_lucy

# Tras git add + git commit (paso 1):
gh repo create TU_REPO --private --source=. --remote=origin --push
```

Si el repo **ya existe vacío**, no uses `repo create`; solo `remote add` + `push` (pasos 2–3).

---

## Qué NO va al repositorio

| Ignorado | Motivo |
|----------|--------|
| `.env` | API keys Omniax, LOPDP, Jelou, Pay, IA |
| `vendor/`, `node_modules/` | Dependencias (`composer install`, `npm ci`) |
| `public/build/` | Se genera con `npm run build` en deploy |
| `Credenciales*.txt` | Secretos locales |

Quien clone debe: `copy .env.example .env` y pedir credenciales (ver [[projects/lucy-webview/HANDOFF-CONTINUATION#Secretos]]).

---

## Después del push

1. Actualizar README del repo con URL clone.
2. En GitHub → Settings → Secrets (opcional) para CI/CD futuro — **no** pegar `.env` en el código.
3. Documentar en Jelou la URL HTTPS del VPS + query `telefono`.
