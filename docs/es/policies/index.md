# Politicas y gobernanza

La gobernanza actual en Arguz se centra en tres flujos:

1. alert policies
2. event notification policies
3. scaling rules

## Alert policies

Enrutan fallas operativas a los canales correctos usando alcance y umbrales.

## Event notification policies

Sirven para eventos operativos no basados en error, como cambios de deployment o eventos de plataforma.

## Scaling rules

Permiten aplicar cambios temporales de HPA con auditoria.

### Rollback automatico

El rollback ocurre cuando:

- termina la ventana de scaling
- la regla se deshabilita antes de tiempo

Eso devuelve el workload a su baseline anterior de HPA.
