# Protocolos de comunicacion

Los agentes de Arguz se comunican por HTTPS usando credenciales acotadas al cluster.

## Autenticacion

- autenticacion por header con cluster token
- un token por cluster
- rotacion desde la Consola de administracion

## Flujos principales

| Flujo | Destino | Objetivo |
|---|---|---|
| Sync de inventario | `api-discover.arguz.io` | deployments, namespaces, revisiones, imagenes, CronJobs, nodos |
| Heartbeat del cluster | `api-discover.arguz.io` | salud y cloud metadata |
| Reconciliacion de scaling | `api-scaling-rules.arguz.io` | reglas activas, rollback y ejecuciones |
