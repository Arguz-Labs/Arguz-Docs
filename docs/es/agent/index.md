# Arguz Agent

El bundle de agentes de Arguz es el conjunto de componentes in-cluster usados por la plataforma. Hoy el chart publico instala:

- **Discovery Agent**
- **Scaling Rules Agent**

## Responsabilidades

### Discovery Agent

- detecta cambios en Deployments y crea revisiones
- descubre namespaces, services, jobs e ingresses
- captura snapshots de HPA
- captura cloud metadata del cluster
- envia snapshots de nodos
- sincroniza definiciones de CronJobs y sus ejecuciones
- guarda las ultimas 100 lineas de logs de ejecuciones fallidas

### Scaling Rules Agent

- aplica cambios temporales de HPA desde plantillas
- mantiene estado de rollback
- restaura los valores anteriores cuando la regla termina o se deshabilita
