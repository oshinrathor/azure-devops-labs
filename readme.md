# Azure DevOps Labs

Welcome to the **Azure DevOps Labs** repository! This project is a practical guide to help you learn Azure DevOps tools, infrastructure automation, and monitoring techniques using real-world scenarios.

---

## 🚀 Monitoring Practical: Prometheus Setup on EC2

This practical focuses on setting up **Prometheus** on an EC2 instance using Terraform and basic DevOps concepts. It also includes answers to foundational cloud and DevOps questions to build core understanding.

🔗 **Live Prometheus Dashboard**: [http://43.205.211.137:9090/](http://43.205.211.137:9090/)

---

### 📘 Questions and Answers

#### 1. What is an EC2 instance? Why do we use it?
An **EC2 instance** is a virtual server provided by AWS to run applications. It's used for scalable computing power on-demand and is ideal for hosting applications, databases, or monitoring tools like Prometheus.

#### 2. What is a Security Group?
A **Security Group** acts like a virtual firewall for your EC2 instance. It controls inbound and outbound traffic using defined rules (e.g., allow port 22 for SSH, 9090 for Prometheus).

#### 3. What is a Virtual Machine?
A **Virtual Machine (VM)** is a software-based emulation of a physical computer. EC2 instances are VMs that run on AWS hardware.

#### 4. What is a resource provider block in Terraform?
In Terraform, a **resource block** defines **what to create** (e.g., `aws_instance`) and its configuration. A **provider block** (like `provider "aws"`) sets the context — region, credentials, etc.

#### 5. What is an agent in CI/CD?
An **agent** is a VM or container that runs CI/CD jobs (like build, test, deploy). Azure DevOps, Jenkins, and GitHub Actions all use agents to execute pipeline tasks.

#### 6. Standard Port Numbers:
- **SSH** – 22  
- **HTTP** – 80  
- **HTTPS** – 443  
- **Prometheus** – 9090  
- **Grafana** – 3000  

#### 7. Methods to connect to an EC2 instance:
- **SSH using `.pem` key**  
- **Session Manager (AWS Systems Manager)**  
- **EC2 Instance Connect (browser-based)**  
- **Third-party tools like PuTTY (Windows)**  

#### 8. Useful EC2 Linux Console Commands:
```bash
sudo yum update -y
sudo yum install wget unzip -y
curl --version
chmod +x <file>
./<binary_file>
```

#### 9. Difference Between `sudo yum update` and `sudo yum upgrade`

| Command                | Description                                                                 |
|------------------------|-----------------------------------------------------------------------------|
| `sudo yum update`      | Updates all packages to the latest compatible versions.                     |
| `sudo yum upgrade`     | Performs the same as `update`, but also removes obsolete packages if needed.|

> ✅ **Use `yum update`** for general updates and `yum upgrade` if you want to clean up deprecated packages too.

---

#### 10. Why Do We Use IPv4 Instead of IPv6?

- **IPv4** is simpler, widely adopted, and compatible with most networks, firewalls, and ISPs.
- **IPv6** provides more address space, but its adoption is still slower due to:
  - Lack of support in legacy infrastructure
  - Added complexity for administrators
  - Compatibility issues in some applications and devices

> ✅ While IPv6 is the future, **IPv4 is still dominant** in cloud and enterprise systems today.

---

#### 11. Explanation of Unzipped Prometheus Files

After downloading and unzipping Prometheus, you'll see the following files:

| File Name         | Description |
|-------------------|-------------|
| `LICENSE`         | Legal terms under which Prometheus is distributed. |
| `NOTICE`          | Notices about third-party components bundled in the software. |
| `prometheus`      | The main binary executable that runs the Prometheus server. |
| `prometheus.yml`  | Default configuration file used to define scrape jobs and targets. |
| `promtool`        | CLI tool for validating config files and testing rule expressions. |

> ✅ Run Prometheus using: `./prometheus --config.file=prometheus.yml`

---

#### 12. How to Write a YAML File for Any Basic Job and Run It

YAML (Yet Another Markup Language) is commonly used in CI/CD pipelines. Below is an example of a basic GitHub Actions workflow written in a `.yml` file to run a simple job:

### Example: `.github/workflows/basic-job.yml`

```yaml
name: Basic CI Job

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install

      - name: Run build
        run: npm run build
```
How to Run:
Save this YAML file inside the .github/workflows/ directory of your GitHub repository.

Commit and push the file to the main branch.

GitHub Actions will automatically trigger the workflow on every push to main.

#### 13. Difference Between Metrics and Targets

### ✅ Metrics:
Metrics are measurable values used to monitor the health, performance, or behavior of systems. They are collected over time and can be used to observe trends, detect anomalies, and trigger alerts.

### ✅ Targets:
Targets are the desired thresholds or goals for those metrics. They define what is considered acceptable performance. Targets help teams assess whether the system is operating within expected limits.

### 🔹 Examples of Metrics:
- **CPU Usage** – The percentage of CPU consumed by processes (e.g., 65%)
- **Memory Utilization** – The amount of RAM in use (e.g., 1.5 GB out of 4 GB)
- **Error Rate** – The ratio of failed requests (e.g., 0.5%)
- **Latency** – Time taken to serve a request (e.g., 220ms)
- **Disk I/O** – Number of read/write operations per second
- **Availability** – Uptime of a system or service (e.g., 99.95%)

### 🔸 Examples of Targets:
- CPU usage should remain **below 75%**
- System uptime should be **at least 99.9%**
- Latency should be **under 300ms**
- Error rate should be **below 1%**

> In short, **metrics measure performance**, while **targets define acceptable performance levels**.

---

#### 14. Different Types of Resource Providers

In Terraform and similar IaC tools, **resource providers** are plugins that allow the tool to interact with various platforms, services, and APIs. Different providers serve different purposes depending on the infrastructure or tools being managed.

### 1. Cloud Providers:
These allow you to provision and manage resources in cloud platforms. Examples include:
- **AWS** (`aws`)
- **Microsoft Azure** (`azurerm`)
- **Google Cloud Platform** (`google`)

You can use these to create resources like virtual machines, storage buckets, and networking components.

### 2. Container Providers:
These manage containerized workloads and orchestration platforms. Examples include:
- **Docker**: for managing local or remote containers and images.
- **Kubernetes**: for managing pods, deployments, services, etc.

These providers are useful for managing application infrastructure built on microservices.

### 3. Configuration & Secrets Management Providers:
These help manage configurations, Helm charts, or sensitive secrets. Examples include:
- **Helm**: for deploying Helm charts to Kubernetes.
- **Vault**: for managing secrets and sensitive information.
- **Consul**: for service discovery and configuration.

### 4. SaaS and API-based Providers:
These connect with third-party services and APIs to manage external tools or integrations. Examples include:
- **GitHub**: to manage repositories, workflows, teams, etc.
- **Datadog**: to monitor infrastructure and send metrics.
- **PagerDuty**: to configure on-call schedules and alerts.

### 5. On-Premise and Virtualization Providers:
These are used to manage infrastructure in local environments or data centers. Examples include:
- **vSphere**: to manage VMware virtual machines and networks.
- **Libvirt**: to manage KVM-based virtualization.

---

## 🎉 Thank You for Visiting!

🚀 This repository is your quick guide to essential **DevOps concepts** and practical **Infrastructure-as-Code** examples. Whether you’re a beginner or brushing up your skills, I hope you find this useful!

---

### 🔗 Stay Connected & Contribute!

If you found this helpful, consider:

- ⭐ **Starring** this repo to show your support
- 🐛 **Reporting issues** or requesting features
- 💡 **Submitting pull requests** with improvements or new examples

---

### 📚 Keep Learning!

Check out more resources on:

- [Terraform Official Docs](https://www.terraform.io/docs)
- [GitHub Actions](https://docs.github.com/en/actions)
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [Prometheus Metrics Guide](https://prometheus.io/docs/practices/naming/)

---

### 🙌 Happy Automating!

![rocket](https://img.shields.io/badge/Keep%20Building-%F0%9F%9A%80-blue)  
![code](https://img.shields.io/badge/Code%20Your%20Future-%F0%9F%92%BB-green)  
![automation](https://img.shields.io/badge/Automate-All%20the%20Things-%23ff69b4)
