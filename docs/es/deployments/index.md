# Deployments e imagenes

Los deployments son la base de la inteligencia de cambios en Arguz. Cada rollout se vuelve una revision enriquecida con contexto de imagen, cluster y errores.

## Que guarda Arguz por deployment

- nombre y alcance
- imagenes y tags
- timestamps de rollout
- historial de revisiones
- contexto de Helm si aplica
- incidentes y resultados de politicas asociados

## Flujos principales

- revisar un rollout
- buscar imagenes por tag o nombre
- validar workloads manejados por Helm
