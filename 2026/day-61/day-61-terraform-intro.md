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

   
