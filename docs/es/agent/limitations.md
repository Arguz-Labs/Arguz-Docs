# Limites y alcance

Esta seccion describe los limites intencionales del bundle actual de agentes.

## Discovery Agent

- los snapshots de nodos no son uso vivo como `kubectl top`
- la deteccion de cloud metadata es best effort
- el historial de CronJobs depende de que Jobs y CronJobs no esten filtrados
- los logs de falla guardan solo las ultimas 100 lineas
- namespaces o recursos excluidos se ignoran completamente

## Scaling Rules Agent

- solo aplica sobre workloads con HPA soportado
- el rollback depende del baseline previo almacenado
- cambios externos al HPA luego de capturar el baseline pueden alterar el resultado esperado
