# IITM Kubernetes Assignment 3

This repository contains the solution for Kubernetes Assignment 3. The objective is to demonstrate deploying an application inside Kubernetes Pods, managing replicas with a ReplicaSet, performing zero-downtime updates using Deployments, and exposing the application through a Service.

## Repository Structure

- `app/` - Contains the source code (HTML/CSS) and `Dockerfile` for versions `v1` and `v2`.
- `k8s-manifests/` - Contains the Kubernetes configuration files (`pod.yaml`, `rs.yaml`, `deployment.yaml`, `service.yaml`).
- `docs/` - Contains the assignment documentation formatted for PDF export.
- `video/` - Contains the screencast demonstrating the assignment execution and rolling updates.

## Quick Start
1. Apply the Deployment and Service:
   ```bash
   kubectl apply -f k8s-manifests/deployment.yaml
   kubectl apply -f k8s-manifests/service.yaml
   ```
2. Verify the pods are running and expose the service locally (e.g. via `minikube service web-dashboard-service`).
3. To trigger the rolling update to version 2:
   ```bash
   kubectl set image deployment/web-dashboard-deployment web-dashboard=shourjya001/iitm-app:v2
   ```

## Requirements Completed
- [x] Application Setup (Glassmorphism Frontend UI)
- [x] Docker Image Versioning (v1 & v2)
- [x] Kubernetes YAML Files
- [x] Deployment Requirements (Rolling Update, Liveness/Readiness/Startup Probes)
- [x] Service Exposure
- [x] Documentation (`docs/Assignment_Documentation.md`)
