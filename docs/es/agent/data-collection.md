# Recoleccion de datos

Este documento describe que capturan los agentes de Arguz desde el cluster y como se representa en el producto.

## Categorias de datos

### Metadata de deployment

- nombre y namespace
- imagenes y tags
- replicas deseadas y disponibles
- estrategia de rollout
- labels y annotations
- timestamp del cambio
- metadata de Helm si existe

### Senales de pods

- `CrashLoopBackOff`
- `OOMKilled`
- restart counts
- cambios de fase

### Configuracion de HPA

- min y max replicas
- metricas objetivo
- behavior
- estado al momento de la captura

### Definiciones de CronJobs

- expresion cron raw
- horario legible
- timezone
- politica de concurrencia
- estado suspendido
- ultimo schedule
- ultimo exito

### Historial de ejecuciones

Para cada Job hijo de un CronJob:

- nombre del Job
- estado
- inicio y fin
- duracion
- motivo de falla
- ultimas 100 lineas de logs si falla

### Snapshots de nodos

- nombre del nodo
- CPU, memoria, storage y pods en capacidad y asignable
- condiciones del nodo
- runtime, OS, arquitectura, instance type, region y zona

!!! note
    Los valores de CPU y memoria en Arguz Nodes son snapshots de inventario, no uso en tiempo real estilo `kubectl top`.
