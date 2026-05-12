---
tags: [error, resuelto, proceso, pr]
date: 2026-05-12
agent: "🗄️ Backend KP"
severidad: "🔴"
story: "STORY-016, STORY-019"
---

# Error: Agente completó commits pero no creó PR

## 🔴 Síntoma
Backend KP ejecutó las tareas correctamente (commits en ramas `feat/story-016-schema` y `feat/story-019-inventory-schema`) pero **no creó los Pull Requests**. Sous tuvo que crear los PRs manualmente (#8 y #9).

## 🔍 Causa Raíz
1. El `task-executor` skill define 6 fases, la fase 6 es "PR — Pull Request hacia develop"
2. El agente completó fases 1-5 (recibir, analizar, ejecutar, revisar, commit) pero no la fase 6
3. El agente no verificó que la tarea estuviera 100% completa antes de terminar
4. No hay un checkpoint automático que verifique "¿el PR fue creado?"

## 🛠️ Solución
1. Reforzado en `task-executor/SKILL.md`: la fase 6 NO es opcional
2. Agregado al checklist de self-review: "¿PR creado en GitHub?"
3. Actualizado SOUL.md de todos los sub-agentes: "Al terminar, verificá que el PR existe"

## 📚 Lección
**Un commit sin PR es una tarea incompleta.** El ciclo solo termina cuando el PR está abierto y el Director KP fue notificado. La fase 6 del task-executor (crear PR) es tan obligatoria como las anteriores.

## 🔗 Relacionado
- [[ts2307-clerk-express-no-instalado]]
- [[ts6133-build-vercel-sessionData]]
