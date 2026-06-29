# Clusters y nodos

Los clusters son el punto de union entre tu estate de Kubernetes y Arguz. La experiencia actual incluye registro, cloud metadata, inventario de namespaces y una vista dedicada de nodos.

## Datos del cluster

- nombre y descripcion
- proyecto asociado
- contexto del provider
- region y zona
- version de Kubernetes
- cantidad de namespaces y deployments
- ultimo heartbeat

## Cloud metadata

El Discovery Agent intenta poblar:

- provider
- scope del cluster
- identificadores de proyecto, cuenta o suscripcion
- region y zona
- nombre del cluster

Esta metadata tambien se usa para construir links a consolas cloud. En AWS se usa el `cluster_name` almacenado por Arguz. En GCP se usa el nombre del cluster reportado por la metadata cloud.

## Pagina de nodos

La pagina **Nodes** muestra:

- readiness y condiciones de presion
- CPU y memoria capacidad vs asignable
- capacidad schedulable de pods
- cluster, proyecto y zona
- instance type, kubelet y runtime
- timestamp de captura
