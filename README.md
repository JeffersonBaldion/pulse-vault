# Pulse Knowledge Vault

Bóveda de conocimiento del ecosistema Pulse. Documenta errores, decisiones y aprendizajes para que ningún agente repita los mismos fallos.

## Estructura

| Directorio | Propósito |
|---|---|
| `errores/` | Errores encontrados, su causa raíz y solución |
| `decisiones/` | Decisiones de arquitectura y su justificación |
| `aprendizajes/` | Lecciones aprendidas que no son errores |
| `plantillas/` | Templates para nuevos documentos |

## Sincronización con Obsidian

Esta carpeta está diseñada para sincronizarse con tu vault de Obsidian:

1. Copiá o symlinkeá esta carpeta a tu vault local
2. Los `[[wikilinks]]` funcionan nativamente en Obsidian
3. Los tags y frontmatter YAML son compatibles con Dataview

## Integración con Agentes

Todos los agentes de Pulse usan el skill `error-logger` (`SKILLS/error-logger/SKILL.md`) para documentar errores automáticamente.
