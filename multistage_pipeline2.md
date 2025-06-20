# 🌊 Multistage Pipeline in DevOps

This README explains the key concepts behind building a multistage pipeline in Azure DevOps for deploying a static web application to Azure Kubernetes Service (AKS) using Docker and Kubernetes.

---

## 1) What is a multistage pipeline?

A multistage pipeline is a DevOps practice where a single YAML file defines **multiple distinct stages** of a CI/CD process. Each stage represents a logical phase, such as:

- **Build** → Compiles code, builds container images
- **Test** → Runs unit/integration tests
- **Deploy** → Pushes code to testing or production environments

Each stage can run jobs in sequence or in parallel. This structure brings better organization, control, and visibility to complex deployments — especially across environments (e.g., testing, staging, production).

---

## 2) What is a service principal?

A service principal is a **security identity** used by applications, services, or automation tools (like Azure DevOps) to access Azure resources. It behaves like a “user” but is meant for non-human tasks.

When you create an Azure Resource Manager or Kubernetes service connection in Azure DevOps, a service principal is often used under the hood to:

- Authenticate with Azure
- Perform operations like deployments, scaling, etc.

---

## 3) What is Kubernetes manifests?

Kubernetes manifests are **YAML configuration files** that describe the desired state of objects in your cluster. Examples include:

- Deployments (define app containers, replicas, etc.)
- Services (expose apps internally or externally)
- Ingress (routes traffic)
- ConfigMaps, Secrets, Volumes, etc.

These manifests are applied to the cluster using tools like `kubectl` or the `Kubernetes@1` task in Azure DevOps.

---

## 4) When creating AKS in Azure Portal, it gives so many options. Explain all.

When creating an AKS cluster via Azure Portal, you'll see multiple configuration sections:

- **Basics**: Choose subscription, resource group, cluster name, region, and Kubernetes version
- **Node pools**: Define VM size, scaling settings, and OS type for worker nodes
- **Authentication**: Configure admin access, Azure AD integration, or RBAC
- **Networking**: Set up network plugins (Azure/CNI), DNS, IP ranges, and virtual networks
- **Integrations**: Options to enable Azure Monitor, Container Insights, Defender, and log analytics
- **Tags**: Add metadata for management and billing

Each setting plays a role in how your cluster performs, scales, and integrates with the rest of Azure.

---

## 5) What is nginx, alpine?

- **nginx** is a lightweight, high-performance web server often used to serve static files like HTML, CSS, JS, or act as a reverse proxy.
- **alpine** refers to Alpine Linux — a super minimal, secure, and fast Linux distribution commonly used as a base image in Docker. The image `nginx:alpine` is simply NGINX running on Alpine Linux, making it ideal for fast, efficient containerization.

---

## 6) Why do we create deployment.yaml and service.yaml in manifests?

- **deployment.yaml** defines the **Kubernetes Deployment**, which:
  - Describes how many replicas (pods) to run
  - Specifies the container image and port
  - Ensures pods are self-healing and updated when configurations change

- **service.yaml** defines the **Kubernetes Service**, which:
  - Exposes the deployment to the network
  - Can be internal (ClusterIP) or external (LoadBalancer)
  - Routes traffic to pods based on labels

Keeping them in a `manifests/` folder keeps your repo organized and makes the structure consistent across environments.

---

## 7) What do we mean by Expose AKS App with LoadBalancer?

Exposing with a LoadBalancer means:

- Creating a Kubernetes Service of type `LoadBalancer`
- Azure provisions an external IP and routes HTTP traffic to your pods
- This is how you access the app via a public URL (e.g., http://<external-ip>)

It acts as a bridge between your private Kubernetes workload and the public internet.

---

## ✅ Recommended Project Structure

Here’s how your repo should look:

/
├── site/                      # Your static HTML files (already exists)  
│   └── index.html  
│  
├── manifests/                 # New folder to store K8s YAML files  
│   ├── deployment.yaml        # K8s Deployment file  
│   └── service.yaml           # K8s Service file  
│  
├── Dockerfile                 # Dockerfile for AKS deployment  
├── azure-pipelines.yml        # Your DevOps pipeline  

- The first Kubernetes@1 task deploys the container (from Docker Hub)  
- The second Kubernetes@1 task exposes the app using service.yaml  

---

## 🧠 Bonus Learnings (Related to Practical Execution)

- We used the `latest` tag in Docker images for simplicity
- Faced an image pull error due to invalid tag formatting — resolved by switching to `latest`
- Used Azure DevOps service connections to automate secure deployments
- Verified deployment success via AKS External IP and pod logs

---

What I Already Have (Covered Perfectly):
✔️ Defined multistage pipeline purpose

✔️ Explained service principal

✔️ Explained Kubernetes manifests

✔️ Covered all key AKS setup options

✔️ Defined nginx and alpine

✔️ Explained deployment.yaml & service.yaml

✔️ LoadBalancer purpose clearly stated

✔️ Project structure diagram ✅

✔️ Related practical challenges ✅

🧩 Optional Additions:
1. What is a Dockerfile (Basic Explanation)?
A Dockerfile is a text file with instructions to build a Docker image that packages your app and its environment.

2. Mention Azure Repos Briefly
You could note that your code was stored and version-controlled in Azure Repos, the Git system within Azure DevOps.

3. Explain Why We Use DockerHub
Quick context on why you chose DockerHub:

DockerHub is a public container registry used to store and distribute Docker images. Azure DevOps pushes the built image here so AKS can pull it later.

4. Future Enhancements (Optional Section)
This can be motivating and useful if shared publicly:

## 🔮 Future Enhancements
- Add custom domain and HTTPS via Ingress
- Use Helm for better K8s config management
- Add rollback support for failed deployments
- Separate dev/stage/prod environments
---
