---
tags: [error, resuelto]
date: 2026-05-11
agent: "🍳 Director KP"
severidad: "🔴"
story: "STORY-006"
---

# Error: Director KP ejecutó código en vez de spawnear sub-agentes

## 🔴 Síntoma
El Director KP, al ser invocado vía `openclaw agent --local`, usaba sus herramientas de escritura para modificar archivos y crear PRs directamente. No delegaba a los sub-agentes.

## 🔍 Causa Raíz
`openclaw agent --local` le da al agente herramientas completas (write, edit, exec). El modelo priorizaba completar la tarea sobre seguir las reglas de "nunca ejecutar código" de su SOUL.md.

## 🛠️ Solución
1. Se reforzó `AGENTS/DIRECTOR/SOUL.md` con la instrucción explícita: "Mi ÚNICA función es refinar y delegar. NUNCA hago commits ni PRs"
2. Se documentó el comando exacto de spawn: `openclaw agent --agent frontend-kp --local -m "tarea" &`
3. Se agregaron variables de entorno para que cada sub-agente tenga su propia identidad Git

## 📚 Lección
**Las reglas en SOUL.md no son suficientes si el agente tiene herramientas de ejecución.** Se necesita un mecanismo de sandboxing que limite las herramientas disponibles para directores vs ejecutores.

## 🔗 Relacionado
- [[sous-ejecuta-codigo-directo]]
- [[sin-sessions-spawn]]
