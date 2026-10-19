# Welcome to ergoshear

Small cluster. Big "I can automate that" energy.

This is where cloud infrastructure, configuration management, and Kubernetes
meet, compare YAML, and agree that the next manual step should probably be code.

## Three repos, one tiny platform

| Repo | Job description |
| --- | --- |
| [aws](https://github.com/ergoshear/aws) | Builds the neighborhood: Terraform, Terragrunt, EC2, persistent Elastic IPs, networking, and Route 53. |
| [salt](https://github.com/ergoshear/salt) | Keeps the machines in line: packages, TuneD, the Salt master, and k3s installation. Highstate checks in every five minutes. |
| [k3s](https://github.com/ergoshear/k3s) | Runs the platform: Argo CD, Traefik, ExternalDNS, and cert-manager, deployed with Kustomize and GitHub Actions. |

## The map (not a treasure map, but there is YAML)

[![ergoshear architecture: three repositories deploying AWS infrastructure, Salt configuration, and a k3s platform](assets/cluster-architecture.svg)](assets/cluster-architecture.svg)

[Open the full-size diagram](assets/cluster-architecture.svg) ·
[Download the editable Excalidraw source](assets/cluster-architecture.excalidraw)

Architecture snapshot: October 18, 2026. The repositories are the source of truth.

## How the gears turn

- **Infrastructure:** pull requests get a Terraform plan; pushes to `main` apply
  the live environment through GitHub OIDC.
- **Machines:** Salt tests the local highstate before applying it. The master
  schedules highstate across its minions every five minutes.
- **Cluster:** a self-hosted runner applies Kustomize manifests when changes
  land on `main`. Argo CD is installed; an application auto-sync setup is not
  configured yet.
- **The front door:** ExternalDNS handles Route 53, cert-manager handles
  Let's Encrypt certificates, and Traefik gets HTTPS to Argo CD.

## Operating philosophy

Write it down. Put it in Git. Automate the boring bits.
Leave the interesting failures educational.

Built in public, one carefully indented adventure at a time.
