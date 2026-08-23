# La Roca RRHH — Contexto del proyecto

App de recursos humanos: expedientes, solicitudes (adelantos, vacaciones, permisos, préstamos, incapacidades), capacitaciones, administración de empleados.

**Producción (Railway):** https://laroca-rrhh-production.up.railway.app  
Repo: https://github.com/jorgedelarocacalix-tech/laroca-rrhh

## Arquitectura
- HTML único `index.html` (~1650 líneas), vanilla JS
- Supabase project: `upaenjotkocmdvfuobii` (distinto al de cobranza/suite)
- RPCs: `rrhh_login(p_codigo,p_pin)`, `rrhh_login_por_pin(p_pin)`, `rrhh_mi_expediente`, `rrhh_admin_dashboard`, `rrhh_admin_resolver_solicitud`
- Tabla principal: `rrhh_empleados` (codigo, nombre, nivel_admin, accesos_apps JSONB con flag cobranza/rrhh/etc)

## Auto-refresco (agregado 2026-08-22)
- Bloque `POLL_MS=20000` antes del RENDER ROUTER: cada 20s compara firma de pendientes (admin) o estados (empleado)
- Si hay cambios: actualiza estado + `render()` + toast 🔔 — se pausa si hay modal abierto (`#modal-bg`) o pestaña oculta
- Al accionar manualmente se sincronizan firmas para no dar falso aviso

## Push
- `notificarAdminsRRHH(title,body)` → Edge Function send-push rol `rrhh` (se dispara al crear solicitudes)

## Deploy
```
railway up --detach   # NO auto-despliega desde GitHub
```
