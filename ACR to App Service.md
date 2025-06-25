# 🚀 ACR to App Service Deployment & Monitoring Guide

Welcome! This README serves as a comprehensive guide for deploying applications from Azure Container Registry (ACR) to Azure App Service, setting up SSH access, securing VMs, and connecting Kubernetes clusters with Prometheus for monitoring.

---

## 📌 1) Why are the tasks numbered like `task@1`, `task@0`? What does this signify?

This format refers to the **version of a pipeline task** in Azure DevOps.

Example: task: Docker@2

The `@2` refers to **version 2** of the Docker task. These versions are important because:

- They define what features or bug fixes are available.
- It ensures consistency across environments.
- Different versions may have different input structures or behaviors.

Always consult the official documentation for the latest or most stable version.

---

## 🔐 2) If I want to SSH into my VM, should I use SSH or username/password authentication?

**SSH key authentication is strongly recommended** over username/password for security reasons.

- **SSH key-based login:**
  - Generate an SSH key pair using `ssh-keygen`.
  - Provide the public key during VM creation.
  - More secure and automation-friendly.
  
- **Username/Password login:**
  - Easier for beginners.
  - Less secure.
  - Susceptible to brute-force attacks.

**Recommendation:** Always use SSH keys unless there is a specific reason to use password-based authentication.

---

## 🔁 3) What is key rotation in SSH authentication?

**Key rotation** refers to periodically changing SSH key pairs to:
- Reduce the risk of key compromise.
- Enforce better access control.
- Meet security compliance standards.

Typical steps:
1. Generate a new key pair.
2. Add the new public key to `~/.ssh/authorized_keys`.
3. Remove the old public key after successful validation.
4. Update any automation scripts or CI/CD tools using the old key.

---

## 🌐 4) How to make VM reachable to Azure Pipelines?

There are a few options:

- **Public IP Access:**
  - Ensure the VM has a static public IP.
  - Open required ports like 22 (SSH), 80 (HTTP), etc., in the NSG (Network Security Group).

- **Private Access:**
  - Use a **self-hosted agent** inside the same virtual network as the VM.
  - Use **Azure Private Link** or a VPN to securely connect.

- **Allow List IPs:**
  - You can also allow Azure DevOps IP ranges in your NSG.

---

## 📤 5) What is SCP?

`scp` stands for **Secure Copy Protocol**.

It allows you to copy files between computers over SSH.

Example: scp file.txt user@remotehost:/path/to/destination/

Use cases:
- Copy deployment artifacts.
- Transfer logs or configuration files.
- Scripted backups and automation.

---

## 📦 6) Why do we need to install dependencies if the application code is already in Azure Repos?

Even though the code is stored in Azure Repos:

- **The runtime environment (e.g., build agent, VM, container) doesn't contain your dependencies by default.**
- Dependencies (like packages, modules, libraries) must be **installed** to ensure the code executes correctly.

For example: npm install
pip install -r requirements.txt

This step ensures your app has everything it needs to build and run properly.

---

## 🔄 7) What does it mean to checkout code from repos?

"Checkout" means **pulling the source code from the repository into your pipeline agent**.

Example in Azure DevOps YAML: checkout: self

This step ensures that:
- The agent has access to your application code.
- Later tasks (like build, test, deploy) can act on the codebase.

---

## 🔒 8) Explain this script: chmod 600 ~/.ssh/id_rsa
ssh-keyscan -H $(vmHost) >> ~/.ssh/known_hosts

**Explanation:**

- `chmod 600 ~/.ssh/id_rsa`:  
  - Restricts permissions on your private SSH key.
  - Prevents unauthorized access.
  - SSH refuses to use insecure keys.

- `ssh-keyscan -H $(vmHost) >> ~/.ssh/known_hosts`:  
  - Retrieves and adds the VM's SSH host key to `known_hosts`.
  - Prevents man-in-the-middle attacks.
  - Avoids interactive authenticity prompts in automation.

---

## 📈 9) What can be the reasons for unexpected CPU spikes in Prometheus?

- **Heavy scraping** of metrics.
- Inefficient or too many recording rules.
- High cardinality labels (e.g., unique combinations of labels).
- Bad queries from Grafana dashboards or users.
- Misconfigured exporters or apps generating excessive metrics.

Mitigation:
- Limit metrics collection frequency.
- Use recording rules to cache heavy queries.
- Avoid high cardinality metrics.
- Optimize alert rules and Grafana panels.

---

## 🧾 10) What are recording rules?

Recording rules are **predefined Prometheus rules** used to **compute and store query results** as new time series.

They help:
- Optimize performance.
- Reduce query load.
- Provide custom metrics.

Example: record: job:http_inprogress_requests:sum
expr: sum(http_inprogress_requests) by (job)

This creates a new metric `job:http_inprogress_requests:sum` that stores the result of the expression.

---

## 📊 11) What is kube-prometheus-stack or Prometheus Operator?

It is a collection of Kubernetes manifests, Helm charts, and CRDs that simplifies monitoring.

Includes:
- Prometheus
- Alertmanager
- Node Exporter
- kube-state-metrics
- Grafana

Prometheus Operator manages Prometheus instances declaratively using Kubernetes CRDs like:
- `ServiceMonitor`
- `PodMonitor`
- `PrometheusRule`

Install via Helm:
helm install prometheus-stack prometheus-community/kube-prometheus-stack

---

## 🧠 12) What is node-level CPU usage?

It refers to the **CPU usage on a specific Kubernetes node**.

Useful metrics:
- `node_cpu_seconds_total`
- `node_exporter` for system-level CPU stats

Use cases:
- Detect overutilized nodes.
- Decide on scaling or load distribution.
- Spot noisy neighbor workloads.

---

## 🧰 13) What is Helm?

Helm is a **Kubernetes package manager**.

Think of it like `apt` or `npm` for Kubernetes apps.

With Helm, you can:
- Install complex apps using pre-built charts.
- Manage configurations with `values.yaml`.
- Upgrade and rollback easily.

Example:
helm install my-app ./chart

---

## 📍 14) Where do we write prometheus.yaml?

You define Prometheus configuration in `prometheus.yaml`.

In Kubernetes, this is often embedded inside:
- A `ConfigMap`
- A Helm chart `values.yaml` (under `prometheus.prometheusSpec.additionalScrapeConfigs`)
- Or CRDs via Prometheus Operator

Manually:
apiVersion: v1
kind: ConfigMap
metadata:
name: prometheus-config
data:
prometheus.yaml: |
global:
scrape_interval: 15s
scrape_configs:
- job_name: 'node'
static_configs:
- targets: ['localhost:9100']

---

## ☁️ 15) Steps to Connect Kubernetes Cluster with Prometheus for Monitoring (Using EC2 Instance)

1. **Provision EC2 Instance**
   - Install Docker, kubectl, and Helm.
   - Ensure connectivity to the Kubernetes cluster.

2. **Access Kubernetes Cluster**
   - Copy the kubeconfig from your control plane.
   - Validate with `kubectl get nodes`.

3. **Install Helm**
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash

4. **Add Prometheus Helm Repo**
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

5. **Install kube-prometheus-stack**
helm install prometheus-stack prometheus-community/kube-prometheus-stack

6. **Expose Grafana**
- Use port-forward:
  ```
  kubectl port-forward svc/prometheus-stack-grafana 3000:80
  ```
- Access via `http://localhost:3000`

7. **Verify Metrics**
- Go to Prometheus UI.
- Check targets and ensure metrics are collected.

---

🎉 **You're all set!** This guide is designed to be beginner-friendly while helping you set up secure deployments and robust monitoring.
