---
tags: [project, lucy-webview, lucy_webview, backlog]
status: active
updated: 2026-09-25
keyword: lucy_webview
---

# lucy_webview — Pendiente (post fases 0–4)

## Hecho recientemente

- Integraciones en `.env` + `check_integrations.php` / `/api/v1/integrations/status`.
- Paso **teléfono** en web (no default `0999999999` en UI).
- Cabina: citas médico/dental no vuelven al menú en `asistencia_vigente` (hogar/vial siguen bloqueando).

## Bloqueantes go-live

1. **Deploy VPS** HTTPS + `.env` producción (`/ec` Omniax).
2. **Skill Jelou** webview URL + listener `jelou:webview:close`.
3. **Secretos** en vault del servidor — repo sin `.env`.

## Paridad Jelou (no crítico MVP)

- VIP verify Omniax, fotos siniestro, afiliación post-pago.
- Proxy opcional Jelou Function (`GEA_JELOU_FUNCTION_*`) si exigen tool legacy.
- Reducir stubs de menú (PMA, hojas informativas).

## Infra checklist

- [ ] `composer install --no-dev`, `npm run build`, `php artisan config:cache`
- [ ] `APP_URL` público, `APP_DEBUG=false`
- [ ] Permisos `storage/`, `bootstrap/cache/`
- [ ] Probar `core_ready` en `/api/health`

Ver [[projects/lucy-webview/HANDOFF-CONTINUATION]].
