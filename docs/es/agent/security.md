# Modelo de seguridad

Arguz busca mantener a los componentes del cluster con alcance acotado y auditable.

## Credenciales del cluster

- un token por cluster
- secreto compartido entre los agentes del chart
- rotacion sin re-registrar el cluster

## Sanitizacion de manifests

Cuando Arguz almacena metadata derivada de manifests:

- los valores sensibles se redaccionan
- las identidades de recursos permanecen visibles

Esto incluye conservar nombres como:

- Secrets
- ConfigMaps
- workloads
- namespaces

Asi se puede depurar sin exponer payloads sensibles.
