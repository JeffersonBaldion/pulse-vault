---
tags: [error, pendiente]
date: 2026-05-11
agent: "⚙️ Operator"
severidad: "🟡"
story: "STORY-008"
---

# Error: Tareas background completadas sin notificar a Jeff

## 🔴 Síntoma
El Director KP y sub-agentes completaron tareas en background (STORY-007, STORY-008, STORY-009) pero Sous nunca notificó a Jeff. Las tareas quedaron sin seguimiento.

## 🔍 Causa Raíz
Sous no hacía polling de los procesos background ni verificaba los logs de los agentes spawnheados. No existía un mecanismo automático de notificación.

## 🛠️ Solución
1. Se creó el agente ⚙️ Pulse Operator para monitoreo automático
2. El Operator lee sesiones de OpenClaw cada 2 minutos y actualiza Mission Control
3. Detecta cambios de estado (active → standby = completado) y notifica a Sous

## 📚 Lección
**Las tareas background necesitan monitoreo automático.** No se puede depender de que Sous recuerde hacer polling manual. El Operator es esencial.

## 🔗 Relacionado
- [[sous-ejecuta-codigo-directo]]
