---
tags: [error, resuelto, build, typescript]
date: 2026-05-12
agent: "🧪 QA KP"
severidad: "🔴"
story: "STORY-014"
---

# Error: TS6133 en build de Vercel — variable no usada en test

## 🔴 Síntoma
El deploy del backend falló en Vercel con error de TypeScript:
```
src/__tests__/supabase-connection.test.ts(95,19): error TS6133: 'sessionData' is declared but its value is never read.
Error: Command "npm run build" exited with 2
```

## 🔍 Causa Raíz
1. **🧪 QA KP** creó el test `supabase-connection.test.ts` con destructuring completo:
   ```typescript
   const { data: sessionData, error: sessionError } = await supabase.auth.getSession();
   ```
   Solo usaba `sessionError` — `sessionData` nunca se leía.

2. El `tsconfig.json` del backend tiene `noUnusedLocals: true` (strict mode), que convierte variables no usadas en errores de compilación.

3. **🟡 Fallo en CI/CD:** El error llegó a deploy en Vercel. DevOps no configuró el pipeline para que el build falle en CI antes de deploy.

4. **🟡 Fallo en QA:** QA no ejecutó `npm run build` antes de hacer el PR.

## 🛠️ Solución
- Removido `data: sessionData` del destructuring → solo `{ error: sessionError }`
- Instalado `@supabase/supabase-js` (faltaba en dependencias)
- PR #5 en `KitchenPulse-Back`

## 📚 Lección
1. **QA debe ejecutar `npm run build` antes de cada PR.** Es parte del checklist de self-review en `task-executor`.
2. **DevOps debe configurar CI para que el build falle en GitHub Actions**, no en Vercel. El pipeline de CI debe incluir `npm run build`.
3. **TypeScript strict mode es correcto.** El error no está en `noUnusedLocals`, está en que no se validó antes de pushear.
4. **El Operator debe monitorear los builds de Vercel** y notificar fallos.

## 🔗 Relacionado
- [[director-kp-ejecuta-codigo]]
- [[sous-ejecuta-codigo-directo]]
- [[pipeline-ci-faltante]]
