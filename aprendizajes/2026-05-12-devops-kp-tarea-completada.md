---
tags: [aprendizaje, tarea-completada, ci-cd, secrets]
date: 2026-05-12
agent: "devops-kp"
story: "STORY-015"
---

# TASK-DO-001: Configurar CI/CD y Secrets para STORY-015

**Evento:** tarea-completada
**Timestamp:** 2026-05-12 15:10 UTC

## Resultado

Completada la configuración de CI/CD y secrets para la integración de Clerk Auth (STORY-015):

### ✅ Secrets de GitHub Actions
- `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY` configurados en `JeffersonBaldion/KitchenPulse-Back`
- `CLERK_SECRET_KEY` y `CLERK_PUBLISHABLE_KEY` — referenciados en CI, a la espera de valores del Clerk Dashboard
- Script de setup: `backend/scripts/setup-github-secrets.sh`

### ✅ CI actualizado
- Clerk env vars agregadas al workflow (`feature/STORY-015-devops-secrets`)
- Nuevo step `typecheck:strict` que previene TS2307/TS7016
- Build pasa limpio: `tsc`, `typecheck:strict`, `vitest` (9/9 tests)

### ✅ Vercel
- Script de configuración: `backend/scripts/setup-vercel-env.sh`
- Pendiente: ejecutar con token de Vercel y valores reales de Clerk

### ✅ Validación runtime
- `env.ts` actualizado para validar `CLERK_SECRET_KEY` y `CLERK_PUBLISHABLE_KEY` con Zod

### 📋 Pendiente (requiere acción humana)
1. Obtener `CLERK_SECRET_KEY` y `CLERK_PUBLISHABLE_KEY` del Clerk Dashboard
2. Ejecutar `backend/scripts/setup-github-secrets.sh` para setear las keys de Clerk
3. Ejecutar `backend/scripts/setup-vercel-env.sh` (o configurar manualmente en Vercel Dashboard)
4. Hacer redeploy en Vercel para aplicar las nuevas variables

## Relacionado
- [[devops-secrets-setup-STORY-015]] — decisión de arquitectura
- [[ts2307-clerk-express-no-instalado]] — error que motivó el guard
- Rama: `feature/STORY-015-devops-secrets`
