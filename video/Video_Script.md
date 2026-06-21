# Screencast Video Script

**Hello everyone! Today I will be walking you through my Kubernetes deployment assignment.**

## 1. Application and Version Differences
*Action: Open your browser and show the v1 application (the blue/purple Nebula theme).*
**Script:** "First, let's look at the application. For Step 1, I built a frontend web application using a modern glassmorphism design. This is Version 1 of the app, running on a sleek blue and purple theme to indicate a stable initial release."

*Action: Switch to a tab showing the v2 application (the neon green Cyberpunk theme).*
**Script:** "And here is Version 2 of the application. As you can see, the styling changes dramatically to a neon green theme to clearly demonstrate when the application has been updated. This visual change makes our rolling update easy to verify."

## 2. Docker Images (v1 vs v2)
*Action: Open your terminal or code editor to show the Dockerfiles in `app/v1/Dockerfile` and `app/v2/Dockerfile`.*
**Script:** "To containerize these applications, I created two Docker images. Both use the `nginx:alpine` base image for a lightweight web server. I've added custom labels for the maintainer and version to distinguish between v1 and v2. We copy our static HTML files into the default Nginx directory."

## 3. Kubernetes YAML Files
*Action: Open the `k8s-manifests/` folder in your code editor.*
**Script:** "Now, let's look at the Kubernetes YAML files. 
- In `pod.yaml`, we define a standalone pod as a baseline.
- In `rs.yaml`, we use a ReplicaSet to ensure 2 instances are always running.
- In `deployment.yaml`, we define a Deployment with 3 replicas. To meet the assignment requirements, I explicitly defined a `RollingUpdate` strategy with `maxUnavailable` set to 0 to ensure zero downtime. I've also configured three required probes: a Startup probe to ensure the app initializes, a Readiness probe to check when it can accept traffic, and a Liveness probe for ongoing health checks.
- Finally, in `service.yaml`, we expose the deployment using a NodePort on port 30080 so we can access it from our browser."

## 4. Deployment and Rolling Update
*Action: Open your terminal and show the v1 pods running (`kubectl get pods`).*
**Script:** "Let's see the deployment in action. Right now, the Version 1 pods are running perfectly."

*Action: Type the command to update the image: `kubectl set image deployment/web-dashboard-deployment web-dashboard=shourjya001/iitm-app:v2` and immediately run `kubectl rollout status deployment/web-dashboard-deployment`.*
**Script:** "Now, I am triggering a rolling update to upgrade our image to Version 2. As we watch the rollout status, you can see Kubernetes systematically terminating the old v1 pods and creating the new v2 pods."

*Action: Run `kubectl get pods` to show all pods are now running.*
**Script:** "The rollout is complete, and all our pods are now successfully running the v2 image."

*Action: Switch back to your browser and refresh the page on `localhost:30080` (or your Node IP) so it changes from the blue v1 theme to the green v2 theme.*
**Script:** "Finally, if we refresh our browser, we can see the frontend has instantly updated to Version 2 without any downtime. This confirms our rolling update was completely successful. Thank you for watching!"
