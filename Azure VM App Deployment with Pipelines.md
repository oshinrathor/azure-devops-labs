# Deploying an Application on a Virtual Machine using YAML Pipeline (Azure DevOps)

## 📌 Questions & Answers

---

### 1) How can I ready agent status for VM?

In Azure DevOps, the term "agent" usually refers to a machine that runs your pipeline jobs. However, when you're deploying to a VM (like one on Azure), you typically don't install an agent directly on that VM.

Instead, you:
- Create an SSH service connection in Azure DevOps that allows it to connect to your VM.
- Ensure your VM is running, SSH-enabled, and accessible (i.e., has a public IP and port 22 open).
- Once you've connected via SSH successfully or verified connectivity via Azure DevOps (via the service connection), you can consider the "agent status" ready. This just means your DevOps pipeline can communicate with the VM over SSH.

---

### 2) What is the difference in copying app files to VM and actually deploying application on VM?

**Copying** app files means:
- Moving your built or prepared application files from the Azure DevOps pipeline (typically from the build agent) to a specific folder on the VM (e.g., `/home/yourusername/deploy`).
- This only transfers the files — your app is not yet running or active.

**Deploying** the application means:
- Taking the copied files and doing everything necessary to make the app run correctly on the server.
- This might include restarting services, installing dependencies, setting environment variables, running database migrations, or starting web servers.

In short: **copying = uploading**, **deploying = running & configuring**.

---

### 3) What does SSH really do?

**SSH (Secure Shell)** is a secure network protocol that allows you to:
- Remotely access and execute commands on another machine (your VM in this case).
- Securely transfer files and perform administrative tasks.

In the context of Azure DevOps:
- SSH allows your pipeline to log into the VM.
- It enables you to copy files (`CopyFilesOverSSH`) and execute scripts (`SSH@0`) directly on the VM.

---

### 4) Who sets up the server and what do we mean by serving an application?

**Who sets up the server?**
- Usually, the **DevOps engineer**, **system administrator**, or **cloud engineer** sets up the server (the VM in this case).
- They are responsible for installing software (like NGINX, Node.js, Python, etc.), configuring ports, firewalls, and ensuring that the machine is ready for hosting applications.

**What does serving an application mean?**
- It means making the application available to users, typically over a network (like the internet).
- The application is usually connected to a server (e.g., a web server like Apache or NGINX) that handles incoming requests and sends back responses.

Example: If you're deploying a Node.js app, "serving" means starting the Node.js server (`node server.js`) so users can access it in their browser.

---

### 5) What is the entire workflow behind application deployment on VM, including setting up servers, sending requests in order?

Here’s a simplified version of the **end-to-end workflow**:

1. **Create a Virtual Machine** (e.g., on Azure).
2. **Set up the server**:
   - Install runtime environment (Node.js, Python, Java, etc.)
   - Open necessary ports in the firewall (e.g., port 80 or 443)
   - Set up a web server (optional but recommended)
3. **Enable SSH Access**:
   - Set username/password or SSH key
   - Open port 22 for SSH in Azure NSG
4. **Create Azure DevOps Pipeline**:
   - Add SSH service connection to the VM
   - Add YAML pipeline with tasks:
     - Copy files to VM
     - Run deployment script (e.g., to start the app)
5. **Run the Pipeline**:
   - Pipeline builds and copies the app files to the VM
   - SSH task runs `deploy_script.sh` to start or update the app
6. **Application is live**:
   - VM listens on a specific port
   - Users can access the application by sending requests to the VM’s public IP or domain

---

## 🔧 Deployment Steps

---

**1. Create a VM on Azure portal**

**2. SSH into your VM using command:**
"ssh username@public_ip_address" from cmd

**3. Agent status is ready. Create a target folder where deployment happens:**
((mkdir -p /home/yourusername/deploy))

**4. Set up an SSH service connection in Azure DevOps**

This allows the pipeline to securely connect to your VM via SSH.

- Go to your Azure DevOps project.
- Navigate to Project settings (bottom-left corner).
- Click Service connections under Pipelines.
- Click New service connection and choose SSH.
- Fill in the details:
        Host: your VM’s public IP address  
        Port: 22 (default)  
        Username: your VM username  
        Password: your VM password (since you are using password authentication)
- Give the connection a name (e.g., MyVM-SSH) and save it.

**5. There are 2 scripts to write in DevOps pipelines:**

a) Copy script (`CopyFilesOverSSH@0` task)

This copies your app code/files from the pipeline agent (where your build runs) to your VM.  
The destination folder on the VM is specified by `targetFolder` in the YAML.

Example:  
targetFolder: '/home/yourusername/deploy'  
Your app files will be copied there on the VM.

b) Deploy script (`deploy_script.sh`)

This is a script already present on your VM or copied along with your files.  
The `SSH@0` task runs this script on the VM.  
The script contains commands like:
- Stop/start your app or service
- Move files if needed
- Install dependencies
- Configure environment variables
- Run database migrations, etc.

So:  
The first task uploads your app code to a folder on the VM.  
The second task connects to the VM and runs commands to finish the deployment.

**6. Write both `deploy_script.sh` and `copyfiles.yml` in Azure Repos, save and commit.**

**7. Create a pipeline in Azure DevOps that uses your YAML**

- Go to your Azure DevOps project in the browser.
- Navigate to Pipelines from the left menu.
- Click New Pipeline.
- Select Azure Repos Git as your code source.
- Pick the repo where you committed your YAML.
- When asked, select Existing Azure Pipelines YAML file.
- Browse and select your YAML file (e.g., deploy-to-azure-vm.yml).
- Click Run or Save.

**8. The application will be deployed.**

---
