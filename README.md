# Main Infrastructure: weyma-talos

**Production Kubernetes infrastructure with disaster recovery capabilities**

This repository contains the foundational infrastructure for my Kubernetes homelab, designed with reliability and rapid recovery as core principles.

## Architecture

My infrastructure follows a layered "black start" approach - essential services run outside the Kubernetes cluster to enable cluster bootstrapping and recovery from total failures.

### Black Start Layer
Static services (Docker Compose on TrueNAS/Proxmox) that provide cluster dependencies:
- Image cache for faster deployments and offline capability
- Talos discovery server for node bootstrapping  
- HashiCorp Vault for secrets management (external to cluster)
- Future: Self-hosted Sidero Omni server (migrating from SaaS)

### System Apps Layer
Applications running within Kubernetes that provide core cluster functionality, managed via ArgoCD with GitOps principles.

## Repository Structure

- **`black-start/`** - Docker Compose services for cluster dependencies
- **`config-patches/`** - Talos Linux configuration patches for cluster and individual machines
- **`omni/`** - Sidero Omni [cluster template](https://docs.siderolabs.com/omni/reference/cluster-templates)
- **`system-apps/`** - System applications (ArgoCD projects) - monitoring, ingress, certificates, storage

## Tech Stack

**OS:** Talos Linux | **Orchestration:** Kubernetes | **GitOps:** ArgoCD | **Secrets:** Vault | **Storage:** Rook-Ceph

## Recovery Process

The "black start" architecture enables ~15-20 minute automated recovery from complete infrastructure failure:
1. Start black-start services → 2. Bootstrap Talos → 3. Deploy system apps → 4. Deploy core apps

For application deployments, see [core-apps](https://git.dubyatp.xyz/core-apps).