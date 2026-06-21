# Assignment 3 – Kubernetes Deployment

**Name:** [Your Name]
**Batch:** Batch 03
**Email:** [Your Email]

---

## 1. Application Overview
The application consists of a modern, responsive Glassmorphism Frontend UI built with HTML and CSS. The frontend is served using an Nginx web server. No separate backend API was built as the focus was on the frontend visualization of the version changes.

- **Frontend v1**: Features a dark blue "Nebula" theme, indicating "Version 1" with a success status.
- **Frontend v2**: Features a dark "Cyberpunk" theme with a neon green layout and animations, indicating "VERSION 2.0 🚀".

---

## 2. Docker Image Creation
Two Docker images were built using `nginx:alpine` to serve the static frontend files. 

### Dockerfile (for v1)
```dockerfile
FROM nginx:alpine
LABEL maintainer="IITM Student"
LABEL version="1.0"
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### Dockerfile (for v2)
```dockerfile
FROM nginx:alpine
LABEL maintainer="IITM Student"
LABEL version="2.0"
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### Build and Push Commands
```bash
docker build -t shourjya001/iitm-app:v1 ./app/v1
docker push shourjya001/iitm-app:v1

docker build -t shourjya001/iitm-app:v2 ./app/v2
docker push shourjya001/iitm-app:v2
```
*[Insert Screenshot of Docker build and push here]*

---

## 3. Kubernetes YAML Files Explanation

The deployment is managed through the following manifests in the `k8s-manifests/` directory:

1. **`pod.yaml`**: Demonstrates the creation of a standalone pod running `shourjya001/iitm-app:v1`.
2. **`rs.yaml`**: Defines a ReplicaSet ensuring 2 instances of the application run simultaneously.
3. **`deployment.yaml`**: Defines a robust Deployment with 3 replicas. It enforces a **RollingUpdate** strategy with `maxUnavailable: 0` and `maxSurge: 1` to ensure zero downtime. It also defines three probes:
   - **Startup Probe**: Ensures the application has fully initialized before checking liveness or readiness.
   - **Readiness Probe**: Checks if the pod is ready to accept traffic (`/` on port 80).
   - **Liveness Probe**: Monitors the health of the container periodically and restarts it if the check fails.
4. **`service.yaml`**: Exposes the deployment to external traffic using a NodePort (`30080`).

*[Insert Screenshot of the YAML files locally]*

---

## 4. Deployment and Update Steps

1. Apply the initial v1 deployment:
```bash
kubectl apply -f k8s-manifests/deployment.yaml
kubectl apply -f k8s-manifests/service.yaml
```
2. Verify the Pods are running:
```bash
kubectl get pods
```
*[Insert Screenshot of v1 Pods Running]*

3. Access the service in the browser at `http://<Node-IP>:30080` to see **Version 1**.
*[Insert Screenshot of Browser showing v1 UI]*

---

## 5. Rolling Update Demonstration

To upgrade the application from `v1` to `v2` without downtime:

1. Update the image in the deployment:
```bash
kubectl set image deployment/web-dashboard-deployment web-dashboard=shourjya001/iitm-app:v2
```

2. Watch the rolling update in progress:
```bash
kubectl rollout status deployment/web-dashboard-deployment
```
*[Insert Screenshot of Transition Phase (Pods updating, Terminating old, creating new)]*

3. Verify final state with only v2 Pods running:
```bash
kubectl get pods
```
*[Insert Screenshot of Final State with only v2 Pods]*

4. Access the service in the browser again to see **Version 2.0 🚀**.
*[Insert Screenshot of Browser showing v2 UI]*

---
**End of Documentation**
