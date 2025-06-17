# EC2 Instance Deployment Using Terraform

## Objective
Automate the provisioning of an EC2 instance on AWS using Terraform, from installation to destruction, all done from an Ubuntu-based EC2 instance.

## Prerequisites
- AWS account with EC2 and IAM access.
- An existing Ubuntu EC2 instance with internet access.
- IAM user with permission to create and destroy EC2 instances.

## Step-by-Step Process

--------------------------------------------------
Step 1: Install Terraform CLI on Ubuntu EC2
--------------------------------------------------

1. Update packages and install dependencies:
   sudo apt update
   sudo apt-get install -y gnupg software-properties-common curl

2. Add HashiCorp GPG key and repository:
   wget -O- https://apt.releases.hashicorp.com/gpg | \
   gpg --dearmor | \
   sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null

3. Add the official HashiCorp repository:
   echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
   https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
   sudo tee /etc/apt/sources.list.d/hashicorp.list

4. Update and install Terraform:
   sudo apt update
   sudo apt-get install terraform

5. Verify installation:
   terraform -version

--------------------------------------------------
Step 2: Configure AWS Credentials
--------------------------------------------------

1. Create Access Keys via AWS Console:
   - Go to IAM > Users > [YourUser] > Security credentials.
   - Click "Create access key".
   - Choose "Application running outside AWS".
   - Copy the access key and secret key.

2. Export the credentials in your EC2 terminal:
   export AWS_ACCESS_KEY_ID=your_access_key_id
   export AWS_SECRET_ACCESS_KEY=your_secret_access_key

--------------------------------------------------
Step 3: Create Terraform Project
--------------------------------------------------

1. Create and move into your project folder:
   mkdir ec2-terraform
   cd ec2-terraform

2. Get the AMI ID of your current EC2 instance:
   curl http://169.254.169.254/latest/meta-data/ami-id

3. Create a file named `main.tf` and paste the following:

   provider "aws" {
     region = "us-east-1"
   }

   resource "aws_instance" "example" {
     ami           = "ami-xxxxxxxxxxxxxxxxx"
     instance_type = "t2.micro"

     tags = {
       Name = "TerraformEC2"
     }
   }

4. Save and exit the file (Ctrl + O, Enter, Ctrl + X if using nano).

--------------------------------------------------
Step 4: Initialize, Validate & Apply Terraform
--------------------------------------------------

1. Initialize the Terraform working directory:
   terraform init

2. (Optional) Format the config:
   terraform fmt

3. Validate the configuration:
   terraform validate

4. Plan the deployment:
   terraform plan

5. Apply to launch the instance:
   terraform apply

   - Type "yes" when prompted.

6. Check the EC2 Dashboard in AWS Console to verify the instance is running.

--------------------------------------------------
Step 5: Destroy Resources (Cleanup)
--------------------------------------------------

1. To destroy the EC2 instance:
   terraform destroy

   - Type "yes" when prompted.

--------------------------------------------------
Files Used
--------------------------------------------------

main.tf

   provider "aws" {
     region = "us-east-1"
   }

   resource "aws_instance" "example" {
     ami           = "ami-xxxxxxxxxxxxxxxxx"
     instance_type = "t2.micro"

     tags = {
       Name = "TerraformEC2"
     }
   }

--------------------------------------------------
Summary
--------------------------------------------------

This guide covered:

- Installing Terraform CLI with GPG key verification.
- Setting up AWS credentials using environment variables.
- Creating a basic Terraform project to launch an EC2 instance.
- Verifying in the AWS Console.
- Cleaning up by destroying the Terraform-managed instance.

This is a foundational walkthrough for automating AWS infrastructure using Terraform.
