# Azure YAML Practical Guide

This document answers key questions related to Azure App Service, API hosting, DevOps practices, Bitbucket vs GitHub, deployment methods, and more. It also includes two common methods for deploying a project using Azure Pipelines and YAML.

---

## 1) What is the use of Azure App Service?

Azure App Service is a fully managed platform for building, deploying, and scaling web apps. It supports multiple programming languages such as .NET, Java, Node.js, PHP, and Python. Developers can host web applications, RESTful APIs, and mobile backends with minimal configuration.

### Key Features:
- Built-in auto-scaling and high availability
- Custom domain and SSL support
- Staging environments for testing
- Integration with Visual Studio, GitHub, Azure DevOps, and Bitbucket
- Supports Windows and Linux environments

---

## 2) Describe different types of APIs.

### Types of APIs:
- **REST API**: Uses HTTP methods (GET, POST, PUT, DELETE). It is stateless and uses URIs to access resources.
- **SOAP API**: A protocol that uses XML messaging and operates over HTTP, SMTP, etc.
- **GraphQL API**: A query language for APIs that allows clients to request exactly the data they need.
- **gRPC**: A high-performance RPC framework developed by Google, often used with protocol buffers.

---

## 3) How can we host API on Azure App Service?

To host an API on Azure App Service:
1. Develop your API (e.g., using Node.js, ASP.NET Core).
2. Push the code to a Git repository (GitHub, Azure Repos, etc.).
3. Create an Azure App Service from the Azure Portal.
4. Set up deployment via:
   - **GitHub Actions**
   - **Azure Pipelines (YAML)**
   - **ZIP deploy**
   - **FTP**
5. App Service will pull and build the code, then expose it at a public URL.

---

## 4) Explain Automatic Scaling & Load Balancing.

- **Automatic Scaling**:
  - Azure App Service can automatically scale up (increase resources) or out (increase instances) based on rules (CPU %, request count, etc.).
  - Manual or schedule-based scaling is also available.

- **Load Balancing**:
  - Azure distributes traffic across multiple instances of the app to ensure no single instance is overwhelmed.
  - Helps improve reliability and performance.

---

## 5) What is Bitbucket and how is it different from GitHub?

- **Bitbucket**:
  - A Git repository management tool developed by Atlassian.
  - Tight integration with Jira and Trello.
  - Supports both Git and Mercurial (GitHub supports only Git).

- **GitHub**:
  - Largest platform for open-source Git repositories.
  - Owned by Microsoft.
  - Strong integration with GitHub Actions and Azure services.

**Difference Summary**:
| Feature        | Bitbucket               | GitHub              |
|----------------|--------------------------|----------------------|
| VCS Support    | Git, Mercurial (earlier) | Git only             |
| CI/CD          | Bitbucket Pipelines      | GitHub Actions       |
| Integration    | Atlassian tools          | Microsoft/Azure      |

---

## 6) What is role-based access control (RBAC) and VNet integration?

- **RBAC**:
  - Role-Based Access Control allows administrators to assign specific permissions to users, groups, and apps.
  - Roles include: Owner, Contributor, Reader, etc.

- **VNet Integration**:
  - Allows Azure App Services to securely communicate with resources in an Azure Virtual Network (VNet).
  - Useful for accessing databases, backend APIs, or services behind firewalls.

---

## 7) What are different ways to deploy app to Azure App Service?

- Azure DevOps (Pipelines with YAML)
- GitHub Actions
- Visual Studio publish
- ZIP deploy
- FTP/SFTP
- Azure CLI (`az webapp deploy`)
- Deployment Center in Azure Portal

---

## 8) What are agents when creating VMs?

- **Agents** (in Azure DevOps):
  - Software used to run build and deployment tasks in a pipeline.
  - Types:
    - **Microsoft-hosted agents**: Pre-configured with tools and OS.
    - **Self-hosted agents**: You manage and configure your own VM or machine.

---

## 9) Do You Need a Security Group for Azure App Service?

- Generally, **no**. Azure App Services are PaaS and managed by Azure, so NSGs (Network Security Groups) are not typically needed.
- If VNet integration is used, and the app accesses private resources, then NSGs might be applied to control traffic inside the VNet.

---

## 10) What is publish profile authentication?

- A **publish profile** is an XML file that contains credentials and settings to deploy apps directly to Azure App Service.
- Used by Visual Studio or CI/CD tools for automated deployments.
- Contains deployment endpoint, userName, and userPWD.

---

## 11) Remember:
Static HTML app at root → '$(Build.SourcesDirectory)/'
Static app inside folder → '$(Build.SourcesDirectory)/app'
Built app output folder → '$(Build.SourcesDirectory)/build' or '$(Build.SourcesDirectory)/dist'

---

## METHOD-1 (Reference)

-- project folder on local machine  
-- open in vscode, initialise git to connect to azure repos  
-- commands:
    git init
    git remote add origin <link from repos>
    git add .
    git commit -m "msg"
    git push -u origin --all
-- already create app service resource on portal  
-- modify (change app name to "index.html" in repos & add correct deployment token) and run pre-generated .yml pipeline  
-- deployed application  

---

## METHOD-2 (Reference)

-- project folder on local machine  
-- open in vscode, initialise git to connect to azure repos  
-- commands:
    git init
    git remote add origin <link from repos>
    git add .
    git commit -m "msg"
    git push -u origin --all
-- already create app service resource on portal  
-- establish service connection to azure devops (manually)  
-- modify or create yaml file in pipeline  
-- only valid for app services not static web apps  

---
