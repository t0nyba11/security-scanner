# Containers And IaC Review

Use this reference when reviewing Dockerfiles, container images, Kubernetes manifests, Terraform, ARM/Bicep, CloudFormation, Helm, or other infrastructure-as-code.

## Focus Areas

* Docker: vulnerable base images, root users, secrets in images, unsafe build steps, missing digest pinning
* Kubernetes: excessive RBAC, privileged pods, host mounts, broad ingress, missing network policies, weak pod security
* Cloud/IaC: public exposure, excessive permissions, unencrypted storage/logs/queues/databases, weak identity boundaries
* Runtime hardening: missing resource limits, missing workload identity, permissive security contexts, disabled controls

## Review Notes

Connect each issue to a deployed resource, reachable exposure, or permission boundary when possible. Prefer specific least-privilege, network, encryption, and runtime-hardening changes that fit the existing deployment model.
