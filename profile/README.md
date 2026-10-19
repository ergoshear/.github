# Welcome to ergoshear

Small cluster. Big "I can automate that" energy.

This is where cloud infrastructure, configuration management, and Kubernetes
meet, compare YAML, and agree that the next manual step should probably be code.

## Three repos, one tiny platform

| Repo | Job description |
| --- | --- |
| [aws](https://github.com/ergoshear/aws) | Builds the neighborhood: Terraform, Terragrunt, EC2, persistent Elastic IPs, networking, and Route 53. |
| [salt](https://github.com/ergoshear/salt) | Keeps the machines in line: Tailscale, Salt, TuneD, and k3s across the control plane and workers. Highstate checks in every five minutes. |
| [k3s](https://github.com/ergoshear/k3s) | Runs the platform: Argo CD, Traefik, ExternalDNS, and cert-manager, with automated GitOps sync and AWS IAM Identity Center SSO configured. |

## The map (not a treasure map, but there is YAML)

[![ergoshear architecture: three repositories deploying AWS infrastructure, Salt configuration, and a k3s platform](assets/cluster-architecture.svg)](assets/cluster-architecture.svg)

[Open the full-size diagram](assets/cluster-architecture.svg) ·
[Download the editable Excalidraw source](assets/cluster-architecture.excalidraw)

Architecture snapshot: October 19, 2026. The repositories are the source of truth.

## How the gears turn

- **Infrastructure:** pull requests get a Terraform plan; pushes to `main` apply
  the live environment through GitHub OIDC.
- **Machines:** Salt tests the local highstate before applying it, manages
  Tailscale connectivity for the Kubernetes nodes, and verifies cross-worker
  pod traffic, cluster DNS, and worker metrics after deployment. The master
  schedules highstate across its minions every five minutes.
- **Cluster:** GitHub Actions bootstraps Argo CD and applies its Application;
  Argo CD then auto-syncs the repository with pruning and self-healing enabled.
  Salt's deployment checks validate the Tailscale-backed worker network.
- **The front door:** ExternalDNS handles Route 53, cert-manager handles
  Let's Encrypt certificates, and Traefik gets HTTPS to Argo CD.
- **Identity:** Argo CD is configured for AWS IAM Identity Center SSO through
  Dex's SAML connector. Interactive login still needs verification; Dex marks
  that connector as unmaintained.

## Operating philosophy

Write it down. Put it in Git. Automate the boring bits.
Leave the interesting failures educational.

Built in public, one carefully indented adventure at a time.
