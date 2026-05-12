---
tags: [error, resuelto, build, dependencias]
date: 2026-05-12
agent: "🗄️ Backend KP"
severidad: "🔴"
story: "STORY-015"
---

# Error: TS2307 — @clerk/express no instalado antes del PR

## 🔴 Síntoma
Build falló en CI/Vercel:
```
src/__tests__/register.test.ts(33,36): error TS2307: Cannot find module '@clerk/express'
src/middleware/auth.ts(1,49): error TS2307: Cannot find module '@clerk/express'
src/routes/register.ts(3,25): error TS2307: Cannot find module '@clerk/express'
```

## 🔍 Causa Raíz
**🗄️ Backend KP** creó código que importa `@clerk/express` pero **no ejecutó `npm install @clerk/express`** ni verificó que `npm run build` pasara antes del PR. El `package.json` no incluía la dependencia.

## 🛠️ Solución
- `npm install @clerk/express` → agregado a package.json
- Build pasa limpio
- PR #6 actualizado

## 📚 Lección
1. **Pattern recurrente:** agentes crean código con imports nuevos pero no instalan dependencias ni verifican build.
2. **CI lo atrapó** (bueno) pero llegó a Jeff (malo).
3. **Regla nueva para task-executor:** después de instalar cualquier dependencia nueva, ejecutar `npm run build` para confirmar que no hay TS2307.
4. **El checklist de backend-kp debe incluir:** ¿package.json refleja todas las dependencias nuevas?

## 🔗 Relacionado
- [[ts6133-build-vercel-sessionData]] — mismo patrón, diferente error
- [[pipeline-ci-faltante]]
