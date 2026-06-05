# Platform Engineer Playground

## 🗳️ Distributed Microservices Architecture Playground

This repository serves as an engineering playground for testing **horizontal autoscaling (HPA)**, **traffic ingestion bottlenecks**, and **asynchronous data pipelines** in a containerized environment. 

The entire infrastructure is managed using a modern **GitOps** philosophy, ensuring the cluster state is automatically synchronized with this repository.

---

## 🚀 Infrastructure & GitOps Stack

* **Local Cluster Engine:** `k3d` (Lightweight Kubernetes running inside Docker)
* **GitOps Controller:** `FluxCD` (Automatically watches this repository and reconciles state)
* **Manifest Management:** `Kustomize` (Organized via localized component layers under `apps/`)

---

## 🧩 Application Components

The playground replicates the classic multi-tier Voting Application, broken down by domain boundaries:

### 1. `vote-frontend` (Python Web App)
* **Role:** The client-facing ingestion tier where users cast their votes.
* **Behavior:** Highly sensitive to concurrent connection load. Built using a synchronous processing paradigm, meaning heavy HTTP traffic bursts cause fast CPU utilization spikes due to thread context-switching.

### 2. `redis` (In-Memory Buffer)
* **Role:** The asynchronous queueing layer separating ingestion from database processing.
* **Behavior:** Extremely lightweight and I/O optimized. Acts as a shock absorber, holding raw voting data strings securely in memory without burning significant compute cycles.

### 3. `vote-worker` (.NET Background Processor)
* **Role:** The background worker engine.
* **Behavior:** Continually polls Redis for new items in a single-threaded loop, batches or processes the raw data, and writes the finalized state down to the persistent database.

### 4. `postgres` / `result-frontend` (Data & Presentation)
* **Role:** Persistent storage and the live analytics dashboard displaying voting tallies.

---

## 📈 Elasticity & Autoscaling (HPA)

To protect the platform against traffic surges (simulated via an Alpine-based load generator), independent **Horizontal Pod Autoscalers** are bundled directly inside their respective microservice directories:
```bash
apps/
├── vote-frontend/
│   ├── hpa.yaml  # Configured to scale up to 10 replicas
└── vote-worker/
├── hpa.yaml    # Configured to scale up to 5 replicas
```