---
tags: [error, resuelto]
date: 2026-05-11
agent: "🧠 Sous"
severidad: "🔴"
story: "STORY-008"
---

# Error: Sous ejecutó código directamente en vez de delegar

## 🔴 Síntoma
Sous hizo commits y PRs directamente en las stories STORY-007, STORY-008, STORY-009, STORY-011. Los commits aparecían con autor `Sous - Director Pulse` en vez del sub-agente correspondiente.

## 🔍 Causa Raíz
Sous usaba `write`, `edit` y `exec` directamente para modificar archivos y crear PRs. No respetaba la cadena de delegación: Sous → Director KP → Sub-agente.

## 🛠️ Solución
1. Se actualizó `SOUL.md` con **Regla Suprema**: la cadena de delegación NO se rompe
2. Se agregó: "Los directores NUNCA hacen commits ni PRs. Los sub-agentes son los ÚNICOS que tocan código"
3. Se reforzó en `AGENTS/DIRECTOR/SOUL.md`: "NUNCA hago commits. NUNCA creo PRs. NUNCA toco archivos de código"

## 📚 Lección
**La cadena de delegación es innegociable.** Sous no codea. Directores no codean. Solo sub-agentes tocan código. Si no se puede delegar, se notifica el bloqueo, pero no se ejecuta directamente.

## 🔗 Relacionado
- [[error-director-kp-ejecuta-codigo]]
- [[delegacion-sin-spawn]]
