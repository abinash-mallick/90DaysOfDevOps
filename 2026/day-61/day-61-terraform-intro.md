### Task 1: Understand Infrastructure as Code
Before touching the terminal, research and write short notes on:

1. What is Infrastructure as Code (IaC)? Why does it matter in DevOps?
2. What problems does IaC solve compared to manually creating resources in the AWS console?
3. How is Terraform different from AWS CloudFormation, Ansible, and Pulumi?
4. What does it mean that Terraform is "declarative" and "cloud-agnostic"?

---

1. Infrastructure as a code means to provision the resources in an automated way thus increasing the speed and reducing the time to market.
   DevOps is all about the automation mindset, hence Terraform plays a key role in fitting into the DevOps world.

2. Problems that we face while manually creating resources: Chances of human error, Spped of provisoning the resource, Inconsistent deployment of resource, no automation, reusability and version control.
   All the above problems can be solved by using IaC.

3. AWS cloudFormation is native to AWS only. We cannot use AWS CFT to provision resources in Azure. Whereas Terraform Cross cloud Platform. We can define the provider for which we need to provision our resources.
   Ansible is a tool for configuration management. Ansible comes into picture after Infrastructure is created.
   Pulumi is similar to Terraform. It is also an IaC tool.

4. Terraform is "declarative" and "cloud-agnostic". This means Terraform follows a declarative approach. We just need to tell Terraform what we want. We do not need to mention the steps on how to provision the resource.
   Terraform will do it itself. Terraform Cross cloud Platform. We can define the provider for which we need to provision our resources.

---

### Task 2: Install Terraform and Configure AWS

1. Install and Verify Terraform

   Installed and verified terraform
   ```

   sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
   wget -O- https://apt.releases.hashicorp.com/gpg | \
   gpg --dearmor | \
   sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null

   gpg --no-default-keyring \
   --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
   --fingerprint

   echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?         <=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

   sudo apt update

   sudo apt-get install terraform

   ```

   Verify Terraform:

   ```
   terraform --version
   ```

   Install aws CLI

   ```
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   unzip awscliv2.zip
   sudo ./aws/install
   ```

   Configure Azure CLI:

   We have to create an IAM user and grant necessary permissions to provision resources.
   Generate the secret token for the IAM user

   Configure AWS CLI using commnad :  ` aws configure `
































   

   
