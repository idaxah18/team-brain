---
tags: [project, lucy-webview, lucy_webview, laragon, onboarding]
status: active
updated: 2026-09-25
keyword: lucy_webview
---

# lucy_webview — Levantar el proyecto (Laragon + Windows)

Para quien **ya tiene Laragon** y va a clonar el repo de GitHub por primera vez.

## 0. Una vez en Laragon (clicks)

1. Abrir **Laragon** → **Start All** (Apache/Nginx no obligatorio si usas `artisan serve`).
2. Menú **Terminal** (o PowerShell en la carpeta del proyecto).
3. Verificar PHP: `php -v` (8.2+). Si no: Laragon → **PHP** → elegir 8.3.

## 1. Clonar y entrar

```powershell
cd C:\laragon\www
git clone https://github.com/idaxah18/formulario-fe-lucy.git
cd formulario-fe-lucy
```

*(Si la URL del repo es otra, sustituir la de `git clone`.)*

## 2. Variables de entorno

```powershell
copy .env.example .env
notepad .env
```

Completar **mínimo**:

| Variable | Dónde conseguirla |
|----------|-------------------|
| `GEA_OMNIAX_CLIENT_ID` | Credenciales Omniax (equipo, no git) |
| `GEA_OMNIAX_CLIENT_SECRET` | Igual |
| `APP_KEY` | Se genera en el paso 3 |

Opcional: `GEA_OMNIAX_BASE_URL` (test-ec por defecto en `.env.example`).

## 3. Dependencias (una sola terminal)

```powershell
composer install
php artisan key:generate
npm install
npm run build
```

Si `composer install` falla por **zip**: Laragon → PHP → `php.ini` → descomentar `extension=zip` → reiniciar terminal.

## 4. Correr (dos terminales)

**Terminal A**

```powershell
cd C:\laragon\www\formulario-fe-lucy
php artisan serve
```

**Terminal B**

```powershell
cd C:\laragon\www\formulario-fe-lucy
npm run dev
```

## 5. Probar en el navegador

| URL | Qué es |
|-----|--------|
| http://localhost:8000/?telefono=0999999999 | Chat Lucy (webview) |
| http://localhost:8000/api/health | Health API |

Cédula de prueba Omniax (QA): `0979461382`.

## 6. Verificación rápida (opcional)

```powershell
php scripts/omniax_probe_gea.php 0979461382 0999999999
node scripts/validate_lucy_graph.mjs
```

## Cursor / contexto

Palabra clave: **`lucy_webview`**. Leer [[projects/lucy-webview/HANDOFF-CONTINUATION]] y en el repo `docs/HANDOFF.md`.

## Actualizar código después

```powershell
cd C:\laragon\www\formulario-fe-lucy
git pull
composer install
npm install
npm run build
```

Si solo cambió frontend en dev: con `npm run dev` basta tras `git pull`.
