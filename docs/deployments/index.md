# Deployments & Images

Deployments are the backbone of Arguz change intelligence. Every rollout becomes a searchable revision enriched with image, cluster and error context.

## What Arguz stores per deployment

- deployment name and scope
- image names and tags
- rollout timestamps
- revision history
- Helm release context when available
- linked incidents and policy outcomes

## Main workflows

### Review a rollout

Open a deployment to inspect:

- latest revision
- image set
- rollout status
- related errors
- HPA context

### Search images

Arguz indexes images across the organization so teams can:

- locate every deployment using a tag
- assess blast radius for a vulnerable image
- compare clusters running different versions

### Validate Helm-managed workloads

If the deployment comes from Helm, Arguz can show release metadata and a sanitized representation of the manifest context without hiding resource identities such as Secret names.

## Frontend views tied to deployments

- Overview dashboard
- Deployments table
- Revision history
- Errors
- Alert policies scoped to a deployment

## Best practices

1. Use stable image tags for production rollouts.
2. Review revisions after every deployment window.
3. Pair alert policies with deployment scope when only a critical workload needs special handling.
