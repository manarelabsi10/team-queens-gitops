# Team Queens GitOps — Kubernetes & GitOps Deployment
This repository contains all Kubernetes, GitOps, monitoring, and deployment configuration files for the **ML Sentiment App** project.

The project demonstrates a complete GitOps workflow using Kubernetes, ArgoCD, Argo Rollouts, Prometheus, and Grafana on a k3d cluster.

---

# Project Objectives
This repository was created for **Part B: Kubernetes + GitOps** of the DevOps assignment.

Main goals:

- Deploy the ML Sentiment App to Kubernetes
- Manage infrastructure using GitOps principles
- Use Kustomize overlays for environment configuration
- Automate synchronization using ArgoCD
- Implement canary deployments using Argo Rollouts
- Monitor the application using Prometheus
- Visualize metrics using Grafana dashboards

---

# Technologies Used
- Kubernetes (k3d)
- ArgoCD
- Argo Rollouts
- Kustomize
- Docker
- Prometheus
- Grafana
- FastAPI
- TextBlob

---

# Repository Structure
```text
team-queens-gitops/
│
├── apps/
│   ├── base/
│   │   ├── configmap.yaml
│   │   ├── service.yaml
│   │   ├── canary-service.yaml
│   │   ├── rollout.yaml
│   │   └── kustomization.yaml
│   │
│   └── overlays/
│       └── production/
│           └── kustomization.yaml
│
├── monitoring/
│   ├── servicemonitor.yaml
│   └── grafana-dashboard-configmap.yaml
│
└── argocd-application.yaml
```

---

# Part B Tasks Overview
## 1. Kubernetes Manifests
Implemented Kubernetes manifests for:

- Deployment/Rollout
- Service
- ConfigMap

Features included:

- Resource requests and limits
- Readiness probe
- Liveness probe
- Environment variables via ConfigMap
- ClusterIP service exposure

Application endpoints:

- `GET /health`
- `POST /predict`
- `GET /metrics`

---

# 2. Kustomize Overlays
Implemented Kustomize using:

- `base/`
- `overlays/production/`

Production overlay customizes:

- Replica count
- Resource limits
- Image tags
- Labels

Deployment command:

```bash
kubectl apply -k overlays/production
```

---

# 3. ArgoCD GitOps Workflow
Configured an ArgoCD Application for automatic synchronization between:

- Git repository
- Kubernetes cluster

Features enabled:

- Automated sync
- Self-healing
- Pruning unused resources

ArgoCD continuously watches the repository and applies any manifest changes automatically.

---

# 4. Canary Deployment with Argo Rollouts
Implemented progressive delivery using Argo Rollouts.

Canary traffic shifting strategy:

```text
20% → 40% → 60% → 80% → 100%
```

Features:
- Stable service
- Canary service
- Automated rollout steps
- Pause durations between rollout stages
- Traffic distribution between old and new ReplicaSets

This allows safe production deployments with reduced deployment risk.

---

# 5. Prometheus Monitoring
Added Prometheus metrics to the FastAPI application.

Metrics include:

- Total prediction requests
- Predictions grouped by sentiment label
- Request latency histogram

A ServiceMonitor resource was configured so Prometheus automatically scrapes the `/metrics` endpoint.

Metrics endpoint:

```text
/metrics
```

---

# 6. Grafana Dashboard
Created a Grafana dashboard with the following monitoring panels:

- Request rate
- Error rate (HTTP 5xx)
- Prediction latency (p50 / p95 / p99)
- CPU usage per pod
- Memory usage per pod

Dashboard is automatically provisioned through a ConfigMap.

---

# GitOps Workflow
The deployment lifecycle works as follows:

1. Push manifest changes to Git repository
2. ArgoCD detects changes automatically
3. Kubernetes cluster syncs automatically
4. Argo Rollouts manages deployment strategy
5. Prometheus scrapes metrics
6. Grafana visualizes application health

---

# Monitoring Stack
| Component | Purpose |
|---|---|
| Prometheus | Metrics collection |
| Grafana | Dashboard visualization |
| ServiceMonitor | Prometheus scraping configuration |
| ArgoCD | GitOps synchronization |
| Argo Rollouts | Canary deployments |

---

# Deployment Verification
The application was verified using:

```bash
curl /health
curl /predict
curl /metrics
```

---

# Future Improvements
- Blue/Green deployment strategy
- Horizontal Pod Autoscaling (HPA)
- AlertManager integration
- Distributed tracing
- Helm chart packaging
- Multi-environment GitOps workflows

---

# Team
Team Queens
DevOps • Kubernetes • GitOps • Observability
