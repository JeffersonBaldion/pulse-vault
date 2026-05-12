---
tags: [errores, índice]
date: 2026-05-12
---

# Registro de Errores

## 🟢 Resueltos
```dataview
TABLE severity AS "Severidad", story AS "Story", date AS "Fecha"
FROM "errores"
WHERE severity != "🔴"
SORT date DESC
```

## 🔴 Pendientes
```dataview
TABLE severity AS "Severidad", story AS "Story"
FROM "errores"
WHERE severity = "🔴"
SORT date DESC
```

---

## 📊 Estadísticas
- **Total de errores:** `$= dv.pages('"errores"').length`
- **Errores por agente:** 
  - 🍳 Director KP: errores donde ejecutó código en vez de delegar
  - 🧠 Sous: errores donde hice commits directos rompiendo el flujo
  - 🅰️ Frontend KP: conflictos entre ramas
  - 🗄️ Backend KP: EDA no respetada
