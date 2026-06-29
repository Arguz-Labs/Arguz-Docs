# Resumen del Discovery Agent

El Discovery Agent es un controlador de Kubernetes orientado a inventario y deteccion de cambios. Se apoya en informers, loops periodicos y envios autenticados por lotes para mantener actualizada la plataforma.

## Comportamiento en runtime

- leader election en `arguz-agent`
- dos replicas recomendadas
- diseno stateless

## Loops principales

| Loop | Objetivo |
|---|---|
| Heartbeat | Marcar el cluster como conectado |
| Heartbeat de metadata | Refrescar provider, region, zona y version de Kubernetes |
| Loop de nodos | Capturar capacidad, asignable y condiciones |
| Sync de deployments | Mantener inventario de deployments, imagenes y revisiones |
| Sync de CronJobs | Mantener horarios y ejecuciones de Jobs |

## Deteccion de provider

El agente intenta primero cloud metadata y luego labels de nodos si el metadata endpoint no es accesible desde el pod. Esto es especialmente util en entornos EKS restringidos.
