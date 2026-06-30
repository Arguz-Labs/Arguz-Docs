# Instalacion

La instalacion de Arguz tiene dos partes:

1. registrar el cluster en la **Consola de administracion**
2. instalar el chart **Arguz Agent** en ese cluster

El chart publico actual despliega ambos componentes in-cluster:

- `Discovery-Agent`
- `Scaling-Rules-Agent`

Recursos publicos:

- [Repositorio del chart Arguz Agent](https://github.com/Arguz-Labs/Arguz-Agent-Chart)
- [Repositorio Helm publico](https://Arguz-Labs.github.io/Arguz-Agent-Chart)

## 1. Registrar el cluster

Desde `app-admin.arguz.io`:

1. entra a la organizacion y proyecto objetivo
2. abre **Clusters**
3. crea un nuevo registro de cluster
4. copia:
   - `PROJECT_ID`
   - `CLUSTER_ID`
   - `CLUSTER_TOKEN`

Estos valores se guardan en el Secret compartido de credenciales del chart.

## 2. Instalar el chart

```bash
helm repo add arguz-agent https://Arguz-Labs.github.io/Arguz-Agent-Chart
helm repo update
helm upgrade --install arguz-agent arguz-agent/arguz-agent \
  --version 1.0.1 \
  -n arguz-agent \
  --create-namespace \
  --set-string global.credentialsSecretName=arguz-credentials \
  --set-string global.clusterCredentials.projectId=<PROJECT_ID> \
  --set-string global.clusterCredentials.clusterId=<CLUSTER_ID> \
  --set-string global.clusterCredentials.clusterToken=<CLUSTER_TOKEN>
```

## Opciones adicionales

### Deshabilitar el scaling rules agent

```bash
helm upgrade --install arguz-agent arguz-agent/arguz-agent \
  -n arguz-agent \
  --create-namespace \
  --set Scaling-Rules-Agent.enabled=false
```

### Valores por defecto del Scaling Rules Agent

```yaml
Scaling-Rules-Agent:
  enabled: true
  env:
    API_URL: https://api-scaling-rules.arguz.io
    LOG_LEVEL: info
    IGNORE_RESOURCES: ""
```

## Que hacen los agentes despues de instalar

### Discovery Agent

- envia heartbeats
- sincroniza namespaces, Deployments, imagenes y revisiones
- captura snapshots de nodos
- detecta cloud metadata del cluster
- sincroniza CronJobs y ejecuciones de Jobs
- guarda en la plataforma hasta las ultimas 200 lineas de logs de jobs fallidos

### Scaling Rules Agent

- consulta templates cada 30 segundos
- evalua ventanas manuales y programadas
- aplica cambios temporales de HPA
- los revierte cuando termina la ventana
- los revierte tambien si el template se deshabilita antes de expirar

## Verificar una instalacion sana

```bash
kubectl get pods -n arguz-agent
kubectl logs deploy/arguz-agent-discovery-agent -n arguz-agent
```

Busca:

- exito de leader election
- sincronizacion de namespaces y Deployments
- inicio del loop de snapshots de nodos
- heartbeat de metadata del cluster
- sincronizacion de CronJobs

## Deteccion de metadata del provider

Arguz intenta detectar el contexto del provider usando metadata del cloud y labels de Kubernetes. Hoy soporta:

- GKE
- EKS
- AKS
- entornos on-prem o desconocidos

Si los endpoints de metadata estan restringidos, el agente usa labels de nodos cuando es posible.
