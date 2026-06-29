# Workloads, servicios y CronJobs

Esta seccion agrupa las vistas operativas mas usadas despues de instalar Arguz: servicios, imagenes y CronJobs.

## Visibilidad de servicios

Desde las vistas de deployments y workloads puedes revisar:

- alcance por proyecto, cluster y namespace
- imagenes y revisiones
- contexto de HPA
- incidentes relacionados

## CronJobs

Los CronJobs son recursos de primera clase en el producto actual.

Para cada CronJob, Arguz muestra:

- expresion cron raw
- horario legible
- timezone
- politica de concurrencia
- estado suspendido
- ultimo schedule
- ultimo exito
- ejecuciones recientes

## Historial de ejecuciones

Cada ejecucion puede incluir:

- inicio y fin
- duracion
- estado
- Job asociado
- motivo de falla
- ultimas 100 lineas de logs si fallo
