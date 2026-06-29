# Instalacion

La instalacion de Arguz tiene dos partes:

1. registrar el cluster en la Consola de administracion
2. instalar el chart **Arguz Agent** en ese cluster

El chart publico actual despliega:

- `Discovery-Agent`
- `Scaling-Rules-Agent`

## 1. Registrar el cluster

Desde `app-admin.arguz.io`:

1. entra a la organizacion y proyecto
2. abre **Clusters**
3. crea el registro del cluster
4. copia:
   - `PROJECT_ID`
   - `CLUSTER_ID`
   - `CLUSTER_TOKEN`

## 2. Instalar el chart

```bash
helm repo add arguz-agent https://Arguz-Labs.github.io/Arguz-Agent-Chart
helm repo update
helm upgrade --install arguz-agent arguz-agent/arguz-agent \
  --version 0.4.0 \
  -n arguz-agent \
  --create-namespace \
  --set-string global.credentialsSecretName=arguz-credentials \
  --set-string global.clusterCredentials.projectId=<PROJECT_ID> \
  --set-string global.clusterCredentials.clusterId=<CLUSTER_ID> \
  --set-string global.clusterCredentials.clusterToken=<CLUSTER_TOKEN>
```

## Opcional: deshabilitar Scaling Rules Agent

```bash
helm upgrade --install arguz-agent arguz-agent/arguz-agent \
  -n arguz-agent \
  --create-namespace \
  --set Scaling-Rules-Agent.enabled=false
```

## Que hacen los agentes

### Discovery Agent

- envia heartbeats
- sincroniza namespaces, deployments, imagenes y revisiones
- captura snapshots de nodos
- detecta cloud metadata del cluster
- sincroniza CronJobs y ejecuciones
- guarda las ultimas 100 lineas de logs de ejecuciones fallidas

### Scaling Rules Agent

- aplica cambios temporales de HPA
- hace rollback al terminar la ventana
- tambien hace rollback si la regla se deshabilita antes
