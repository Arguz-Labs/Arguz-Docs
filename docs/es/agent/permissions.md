# Permisos requeridos

El chart necesita acceso de lectura para discovery y permisos de actualizacion de HPA cuando el Scaling Rules Agent esta habilitado.

## Discovery Agent

Lectura sobre:

- Deployments
- ReplicaSets
- Pods
- Jobs
- CronJobs
- HPAs
- Namespaces
- Services
- Nodes
- Ingresses
- ConfigMaps

## Scaling Rules Agent

Necesita:

- leer HPAs
- patch de HPAs
- restaurar valores previos de HPA
