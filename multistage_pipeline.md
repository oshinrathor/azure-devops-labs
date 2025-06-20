# 🚀 Azure DevOps CI/CD Pipeline: Deploy HTML App to App Service & AKS

This project documents the full journey of deploying a static HTML website to two environments using Azure DevOps:

1. Azure App Service (for testing)
2. Azure Kubernetes Service (AKS, for production)

The entire deployment is automated using a multi-stage YAML pipeline.

---

## 📌 Step-by-Step Process (Full Journey Summary)

1. Created an HTML app and committed it to Azure Repos (`site/` folder)

2. Created an **App Service** in Azure named `oshin`

3. Created a new **AKS (Azure Kubernetes Service)** cluster from Azure Portal
   - The cluster was empty at the start

4. Created a **DockerHub account**
   - Set up a public repo `oshinrathor/html-app` to push images

5. Created the following supporting files and committed to Azure Repos:
   - `Dockerfile` to containerize the HTML app
   - `manifests/deployment.yaml` for AKS deployment
   - `manifests/service.yaml` to expose app via LoadBalancer
   - `azure-pipelines.yml` for CI/CD

6. Created 3 **Service Connections** in Azure DevOps:
   - Azure Resource Manager (ARM) for App Service
   - DockerHub connection
   - Kubernetes Service Connection for AKS

7. Built the **first stage** of the YAML pipeline to deploy code directly to Azure App Service
   - Used `AzureWebApp@1` task with the ARM connection

8. Built the **second stage** of the pipeline to:
   - Login to DockerHub
   - Build the Docker image using the Dockerfile
   - Push the image with the tag `latest`
   - Deploy to AKS using `Kubernetes@1` task
   - Expose the AKS app using a LoadBalancer service

9. Committed and ran the multi-stage pipeline from Azure DevOps
   - Verified App Service deployment first
   - Docker image pushed successfully to DockerHub
   - Kubernetes deployment and service applied to AKS

10. Accessed the deployed app on AKS using the external IP from the LoadBalancer

---

## ⚔️ Challenges Faced & Solutions

**Challenge 1:** Deployment to AKS failed with `invalid image reference format`  
**Reason:** Kubernetes manifest contained a placeholder `$(Build.BuildId)` that wasn’t replaced  
**Solution:** Switched to using a static `latest` image tag in both the YAML and Docker tasks for simplicity

---

**Challenge 2:** Pod was stuck in `0/2 Running`  
**Reason:** Kubernetes couldn’t pull the image because the tag was invalid  
**Solution:** After fixing the image tag and pushing the correct image to DockerHub, the pod became `1/1 Running`

---

**Challenge 3:** External IP returned a blank page  
**Reason:** The container hadn’t started because image failed  
**Solution:** Waited for the image to be rebuilt, pushed correctly, and verified that NGINX could serve static content

---

## ✅ Final Outcome

- Full multi-stage CI/CD pipeline successfully built and deployed
- App runs on both Azure App Service and AKS
- Live URL from AKS LoadBalancer works as expected

This documentation is a summary of the entire DevOps flow: from nothing to a fully automated deployment pipeline running in the cloud.
