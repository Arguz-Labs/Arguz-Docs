# Revisiones

Arguz guarda revisiones como checkpoints inmutables de cada rollout.

## Que contiene una revision

- lista de imagenes
- timestamp
- estado
- alcance del deployment
- incidentes asociados
- snapshot de HPA si existe
- contexto de Helm cuando el workload usa Helm

## Para que sirven

- comparar cambios entre versiones
- revisar impacto del deployment
- preservar contexto sanitizado de Helm manteniendo nombres de recursos
