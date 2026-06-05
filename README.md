# Platform Engineer Playground

## 💡 Purpose

This repository is a practical catalog of Kubernetes and GitOps best practices for platform engineering. It is built around a `k3d` + `FluxCD` + `Kustomize` stack and uses the Vote Polling App as a demo workload.

The root README serves as an index for the repository's scenarios and design examples.

---

## 🧩 Stack Overview

* **Kubernetes runtime:** `k3d` (local Kubernetes in Docker)
* **GitOps controller:** `FluxCD` (cluster state synchronized from git)
* **Manifest orchestration:** `Kustomize` (modular overlays and reusable components)
* **Demo application:** Vote Polling App with frontend, queue, worker, database, and results dashboard

---

## 📚 Feature Catalog

| Feature | Description |
|---|---|
| [Application architecture](./apps/README.md) | Vote Polling App components, responsibilities, and GitOps layout |

---

## ✨ Planned scenario categories

* Autoscaling and load shaping
* Autoscaling over custom metrics
* Observability and monitoring
* Networking and service topology
* Resilience and failure recovery
