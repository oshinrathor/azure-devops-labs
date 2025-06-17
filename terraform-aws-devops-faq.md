# 📘 Terraform, AWS, and DevOps Concepts

Welcome to this concise question-and-answer style knowledge base designed to help beginners and practitioners understand key concepts related to Terraform, AWS, and cloud infrastructure automation.

This guide covers foundational topics such as common Linux commands, AWS credentials, Terraform configuration files, security keys, and essential Terraform commands.

Whether you’re just starting with infrastructure as code or looking to brush up on core concepts, this FAQ-style repository serves as a handy reference to accelerate your learning journey.

---

## ❓ 1) What is httpd?

httpd is the Apache HTTP Server daemon. It is a web server used to serve web content over HTTP. On Linux systems, installing and starting the httpd service allows a server to respond to web requests on port 80 (or HTTPS on port 443 if configured).

Command to install on Ubuntu:  
`sudo apt install apache2`

Command to start it:  
`sudo systemctl start apache2`

On Amazon Linux or RedHat:  
`sudo yum install httpd`

---

## ❓ 2) What does curl command do?

curl is a command-line tool used to send requests and receive responses over various internet protocols, such as HTTP, HTTPS, FTP, etc.

Example:  
`curl http://example.com`

This retrieves the raw HTML or data from the given URL.

It is often used in scripts to fetch API data, download files, or check web server status.

---

## ❓ 3) What does `sudo chmod 777 prometheus` do?

This command changes the file or directory permissions of `prometheus`:

- `chmod` changes file modes (permissions).  
- `777` means:  
  - Read (4), write (2), and execute (1) permissions are given to user, group, and others.  
- `sudo` is used to run the command as a superuser.

In short: this command allows **anyone** to read, write, and execute the `prometheus` file or directory, which may not be secure in production.

---

## ❓ 4) What is the difference in normal deployments and deployment through Kubernetes?

**Normal deployments:**

- Manual or scripted processes  
- Use traditional tools like shell scripts, Docker Compose, or cloud-specific deployment  

**Deployment via Kubernetes:**

- Declarative (YAML files define desired state)  
- Managed by Kubernetes itself (auto-scaling, healing, rolling updates)  
- Works at container level, with orchestration, load balancing, service discovery, etc.

Kubernetes abstracts the infrastructure and automates the lifecycle of container-based applications.

---

## ❓ 5) Why do we need gnupg, software-properties-common, and curl packages installed?

- `gnupg`: Enables key-based authentication and verification (used for verifying GPG keys).  
- `software-properties-common`: Provides scripts for managing APT repositories.  
- `curl`: Used to fetch files from the internet over HTTP.

These are needed to:

- Securely import and verify the HashiCorp GPG key  
- Add the HashiCorp APT repository  
- Install trusted versions of Terraform

---

## ❓ 6) What content exists in terraform .tf file, variables.tf, outputs.tf, terraform.tfvars?

- `main.tf`: Contains the actual infrastructure configuration (e.g., AWS provider, resources).  
- `variables.tf`: Defines input variables with type, description, and optional default values.  
- `terraform.tfvars`: Provides the values for variables defined in `variables.tf`.  
- `outputs.tf`: Specifies the output values (like instance IPs) after `terraform apply`.

This separation helps organize reusable, clean, and readable Terraform code.

---

## ❓ 7) How is AWS and HashiCorp different with respect to providers in Terraform?

- **AWS Provider**: Official plugin maintained by HashiCorp that lets you manage AWS infrastructure like EC2, S3, RDS, IAM, etc.

- **HashiCorp Provider**: Generic term. HashiCorp maintains many providers (AWS, Azure, Google, etc.).

In Terraform:  
- AWS is **a provider**  
- HashiCorp is the **maintainer** of the AWS provider

Providers act as API bridges between Terraform and the cloud platform.

---

## ❓ 8) Explain all Terraform commands.

- `terraform init`: Initializes the directory and downloads provider plugins.  
- `terraform plan`: Shows what Terraform will do (create, destroy, update).  
- `terraform apply`: Applies the configuration to build infrastructure.  
- `terraform destroy`: Removes all infrastructure managed by Terraform.  
- `terraform validate`: Checks for syntax errors in configuration.  
- `terraform fmt`: Formats the `.tf` files for readability.  
- `terraform show`: Shows the current state or a saved plan file.  
- `terraform state`: Advanced management of the state file.  
- `terraform taint`: Marks a resource for recreation during the next apply.  
- `terraform untaint`: Reverses a taint.  
- `terraform refresh`: Syncs Terraform state with real-world infrastructure.

---

## ❓ 9) What is RSA, SSH, PUB, AMI, IAM?

- `RSA`: A cryptographic algorithm used to generate public/private key pairs for secure communication.  
- `SSH`: Secure Shell, a protocol for securely accessing remote machines.  
- `PUB`: A `.pub` file is a public key file used in SSH key pairs.  
- `AMI`: Amazon Machine Image — a snapshot/template to launch EC2 instances.  
- `IAM`: Identity and Access Management — AWS service for managing users, permissions, and roles.

---

## ❓ 10) Why do we sometimes switch to administrator for running commands in EC2?

Some commands require elevated permissions to:

- Install software  
- Modify system files  
- Change configurations  
- Start/stop services

Using `sudo` (superuser do) gives temporary administrator rights for those operations.

---

## ❓ 11) What is HashiCorp’s GPG key used for?

HashiCorp’s GPG key is used to:

- Sign their software packages and repositories  
- Verify package integrity and authenticity during installation  
- Prevent tampering or downloading malicious versions

The key is imported before installing Terraform to ensure it comes from a trusted source.

---

## ❓ 12) What each one of them is used for and how are they different: AWS Access Key ID and Secret Access Key

- **Access Key ID**: Like a username or public identifier. Used to identify the AWS user.  
- **Secret Access Key**: Like a password. Used to authenticate requests.

Together, they are your credentials for programmatic access to AWS services. You must keep the Secret Access Key private.

---

## ❓ 13) What is S3 and DynamoDB?

- **S3 (Simple Storage Service)**:  
    - Object storage service in AWS.  
    - Used to store files, backups, logs, static websites.  
    - Can also store Terraform state files (remote backend).

- **DynamoDB**:  
    - Fully-managed NoSQL key-value and document database.  
    - Commonly used with S3 for Terraform state locking and consistency.  
    - Prevents multiple users from editing the same Terraform state at once.

Together, S3 + DynamoDB enable collaborative, remote-safe Terraform workflows.
