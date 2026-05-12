---
tags: [decision, ci-cd, secrets, clerk, supabase]
date: 2026-05-12
agent: "devops-kp"
story: "STORY-015"
---

# Decisión: Configuración de Secrets y CI/CD para Clerk Auth

## 📋 Contexto

STORY-015 integra Clerk como proveedor de autenticación (además de Supabase Auth). Esto requiere que `CLERK_SECRET_KEY` y `CLERK_PUBLISHABLE_KEY` estén disponibles tanto en CI (GitHub Actions) como en producción (Vercel).

## 🎯 Decisión Tomada

### 1. Secrets Management

**GitHub Actions:**
- Todos los secrets se almacenan como GitHub Repository Secrets (`gh secret set`)
- Referenciados en CI via `${{ secrets.SECRET_NAME }}`
- Repos: `JeffersonBaldion/KitchenPulse-Back` (backend), `JeffersonBaldion/KitchenPulse` (frontend)
- Script de setup: `backend/scripts/setup-github-secrets.sh`

**Vercel:**
- Variables de entorno configuradas via Vercel Dashboard o `vercel env add`
- Mismos nombres que en CI para consistencia
- Requiere redeploy para aplicar cambios
- Script de setup: `backend/scripts/setup-vercel-env.sh`

### 2. Secrets Requeridos por Entorno

| Secret | GitHub Actions | Vercel (prod) | Vercel (preview) |
|---|---|---|---|
| SUPABASE_URL | ✅ | ✅ | ✅ |
| SUPABASE_ANON_KEY | ✅ | ✅ | ✅ |
| SUPABASE_SERVICE_ROLE_KEY | ✅ | ✅ | ✅ |
| CLERK_SECRET_KEY | ✅ | ✅ | ✅ |
| CLERK_PUBLISHABLE_KEY | ✅ | ✅ | ✅ |

### 3. Validación en Runtime

`src/config/env.ts` valida todas las variables con Zod al iniciar la app. Si falta alguna, el proceso hace `exit(1)` con mensaje claro:

```typescript
CLERK_SECRET_KEY: z.string().min(1),
CLERK_PUBLISHABLE_KEY: z.string().min(1),
```

### 4. CI Guard: TS2307 / TS7016

Se agregó un step adicional en CI (`npm run typecheck:strict`) que falla explícitamente si detecta TS2307 (Cannot find module) o TS7016 (Missing declaration). Esto previene que imports de Clerk (u otros módulos) rotos lleguen a producción.

**Justificación:** Este tipo de error ya había ocurrido antes (ver [[ts2307-clerk-express-no-instalado]]). El step extra da visibilidad inmediata con un mensaje claro en el log de CI.

## 🔄 Alternativas Consideradas

1. **Solo usar secrets de Vercel** → Descartado: CI necesita las vars para build y test
2. **Hardcodear en CI yml** → Descartado: riesgo de seguridad, los secrets se exponen en logs
3. **Usar solo `tsc --noEmit`** → Insuficiente: no da mensaje específico cuando es TS2307/TS7016

## 📚 Reglas Derivadas

1. Todo PR que agregue imports nuevos DEBE pasar `npm run typecheck:strict`
2. Secrets nuevos se documentan primero en vault, luego se configuran
3. Scripts de setup en `scripts/` son la fuente de verdad para configurar entornos

## 🔗 Relacionado
- [[ts2307-clerk-express-no-instalado]]
- [[ts6133-build-vercel-sessionData]]
- Story: STORY-015 (Clerk Auth Integration)
- Rama: `feature/STORY-015-devops-secrets`
