# terraformskill

# EC2 Nginx Deployment with Terraform

## 1. Project Overview

This project demonstrates Infrastructure as Code (IaC) using Terraform to deploy an Ubuntu EC2 instance on AWS and configure an Nginx web server automatically.

The deployment uses an existing default VPC and installs Nginx using an EC2 `user_data` script. The default Nginx welcome page is replaced with a custom HTML page.

**Custom webpage content:**

> Welcome to the Terraform-managed Nginx Server on Ubuntu

## 2. Objectives

- Configure the AWS provider and region.
- Launch a `t2.micro` EC2 instance using Ubuntu 20.04 LTS.
- Use the existing default VPC.
- Configure a security group to allow HTTP and SSH traffic.
- Install and configure Nginx automatically using `user_data`.
- Display the EC2 public IP address using Terraform outputs.
- Destroy the resources created by Terraform when the deployment is no longer required.

## 3. Technologies Used

- AWS EC2
- AWS VPC and Security Groups
- Ubuntu 20.04 LTS
- Nginx
- Terraform
- AWS CLI
- Git and GitHub

## 4. Project Structure

```text
terraform-nginx-ubuntu/
├── main.tf
├── variables.tf
├── outputs.tf
├── README.md
└── .gitignore
```

### File Description

- **main.tf:** Configures the AWS provider, retrieves the Ubuntu AMI and default VPC, creates the security group, and launches the EC2 instance.
- **variables.tf:** Defines configurable variables such as AWS region, instance type, SSH CIDR range, and optional EC2 key pair name.
- **outputs.tf:** Displays the EC2 instance ID, public IP address, and website URL.
- **README.md:** Documents the project and deployment instructions.
- **.gitignore:** Excludes Terraform state files and other local files from Git.

## 5. Prerequisites

Before starting the deployment, ensure that the following are available:

1. An AWS account with the necessary EC2 permissions.
2. Terraform CLI installed on the local machine.
3. AWS CLI installed and configured.
4. Access to an existing default VPC in the selected AWS region.
5. An approved Ubuntu 20.04 LTS AMI available in that region.

Check the Terraform installation:

```powershell
terraform -version
```

Check the AWS CLI installation:

```powershell
aws --version
```

Configure AWS credentials if required:

```powershell
aws configure
```

Verify the AWS identity:

```powershell
aws sts get-caller-identity
```

**Note:** In AWS accounts governed by an Organizations Service Control Policy (SCP), the administrator must permit the required actions. An explicit SCP deny cannot be overridden by attaching an IAM allow policy.

## 6. AWS Resources

### EC2 Instance

- Instance type: `t2.micro`
- Operating system: Ubuntu 20.04 LTS
- Public IPv4 address: Assigned during deployment
- Web server: Nginx

### Security Group

| Protocol | Port | Purpose |
|---|---:|---|
| HTTP | 80 | Access the Nginx website |
| SSH | 22 | Remote server administration |

SSH access should be restricted to the user's public IP address wherever possible.

### Networking

The deployment reuses the existing default VPC and its subnet. It does not create a separate VPC, subnet, or internet gateway.

## 7. Deployment Instructions

Open PowerShell and navigate to the project directory:

```powershell
cd terraform-nginx-ubuntu
```

### Step 1: Initialize Terraform

```powershell
terraform init
```

This downloads the required provider and initializes the Terraform working directory.

### Step 2: Review the Execution Plan

```powershell
terraform plan
```

This previews the resources Terraform intends to create.

Resolve any configuration or permission errors before proceeding.

### Step 3: Deploy the Infrastructure

```powershell
terraform apply
```

Review the proposed changes and enter `yes` when prompted.

Terraform creates the security group and EC2 instance. The EC2 startup script installs Nginx and configures the custom HTML page.

### Step 4: Retrieve the Public IP

```powershell
terraform output public_ip
```

To retrieve the website URL:

```powershell
terraform output website_url
```

Open the displayed URL in a web browser using HTTP.

Example:

```text
http://YOUR_PUBLIC_IP
```

Replace `YOUR_PUBLIC_IP` with the actual address returned by Terraform.

## 8. User Data Configuration

The EC2 `user_data` script performs the following operations during initial startup:

1. Updates the Ubuntu package index.
2. Installs Nginx.
3. Replaces `/var/www/html/index.html` with a custom HTML page.
4. Enables Nginx to start automatically.
5. Restarts the Nginx service.

The resulting webpage displays:

**Welcome to the Terraform-managed Nginx Server on Ubuntu**

## 9. Verification

After deployment, verify the following:

1. The EC2 instance is running in the AWS Console.
2. The instance uses the expected Ubuntu 20.04 AMI and `t2.micro` instance type.
3. The security group contains the required inbound rules.
4. Terraform outputs the public IP address.
5. The custom webpage loads successfully in a browser.

If the webpage does not load, check the instance's public IP, subnet routing, security group rules, and Nginx startup logs.

The cloud-init startup log can be inspected on the instance using:

```bash
sudo tail -100 /var/log/cloud-init-output.log
```

Check the Nginx service status using:

```bash
sudo systemctl status nginx
```

## 10. Destroy the Infrastructure

After verification and screenshot collection, run:

```powershell
terraform destroy
```

Review the proposed deletions and enter `yes` when prompted.

Terraform removes the resources managed by this configuration, including the EC2 instance and security group. The existing default VPC and subnet are not deleted.

Verify that Terraform reports successful destruction.

## 11. Screenshots for Submission

Include screenshots showing the actual results of your deployment:

1. **Terraform Apply:** Successful resource creation and public IP output.
2. **AWS EC2 Console:** The deployed EC2 instance and its instance type.
3. **Working Nginx Website:** The custom welcome page displayed in the browser.
4. **Security Group:** The HTTP and SSH inbound rules.
5. **Terraform Destroy:** Successful resource cleanup.

Place the screenshots in a `screenshots/` directory if desired.

## 12. Troubleshooting

### AWS Authorization Error

An error such as `UnauthorizedOperation` indicates that the current IAM identity or an AWS Organizations policy prevents the requested action.

For example, an explicit deny for `ec2:DescribeImages` must be reviewed by the AWS Organizations administrator. Changing Terraform syntax or attaching an IAM allow policy alone will not override an explicit SCP deny.

### Ubuntu AMI Lookup Error

Confirm that the Ubuntu 20.04 AMI lookup uses the correct region and that an approved AMI is available. If AMI discovery is blocked, obtain an approved AMI ID from the administrator.

### Website Not Accessible

Check that the instance has a public IPv4 address, that its subnet has appropriate internet routing, that inbound TCP port 80 is allowed, and that Nginx has started successfully.

## 13. Conclusion

This project demonstrates how Terraform can provision AWS infrastructure, configure a Linux web server automatically, expose the application through a public IP address, and manage the infrastructure lifecycle through code.

Using Infrastructure as Code makes the deployment repeatable, easier to maintain, and simpler to clean up when the resources are no longer required.

---

**Assignment:** EC2 Nginx Deployment with Terraform  
**Repository:** Add your GitHub repository URL here  
**Status:** Update this section after successfully deploying and testing the application.
