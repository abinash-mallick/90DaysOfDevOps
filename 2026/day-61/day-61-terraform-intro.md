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

---

### Task 3: Your First Terraform Config -- Create an S3 Bucket

```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
  }
}
provider "aws" {
    region = "us-west-2"
  
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "terra-bucket"

  tags = {
    Name = "My bucket"
  }
}
```

**Document:** What did `terraform init` download? What does the `.terraform/` directory contain?

Terraform init prepares the working directory so that terraform can run in it.
It initializes the setup where the state file(terraform.tfstate) will be stored. It will be in the current working dir if we have not configured remote backend.
Necessary provider plugins will be installed.
It also creates the .terraform.lock.hcl file. It locks the provider version and ensures that the same version is used through out.
It also created a .terraform file that stores the metadata of the providers.

---

### Task 4: Add an EC2 Instance
In the same `main.tf`, add:
1. A `resource "aws_instance"` using AMI `ami-0f5ee92e2d63afc18` (Amazon Linux 2 in ap-south-1 -- use the correct AMI for your region)
2. Set instance type to `t2.micro`
3. Add a tag: `Name = "TerraWeek-Day1"`

```
resource "aws_instance" "my_instance" {
    ami = "ami-043ab4148b7bb33e9"
    instance_type = "t3.micro"

    tags = {
      Name = "TerraWeek-Day1"
    }
}

resource "aws_ec2_instance_state" "instance_state" {
instance_id = aws_instance.my_instance.id
state = "stopped"

}
```



How does Terraform know the S3 bucket already exists and only the EC2 instance needs to be created?

This is where the .tfstate file comes into picture. The tfstate file keeps the records of the resources provisioned by terraform. Terraform validates the actual state versus the desired state using the tfstate file.
The resources that are already provisioned are skipped by terraform using the tfstate file.

---


### Task 5: Understand the State File
Terraform tracks everything it creates in a state file. Time to inspect it.

1. Open `terraform.tfstate` in your editor -- read the JSON structure


2. Run these commands and document what each returns:
```
ubuntu@ip-172-31-39-129:~/terraform$ terraform state list
aws_ec2_instance_state.instance_state
aws_instance.my_instance
aws_s3_bucket.my_bucket
ubuntu@ip-172-31-39-129:~/terraform$ terraform state list aws_s3_bucket.my_bucket
aws_s3_bucket.my_bucket
ubuntu@ip-172-31-39-129:~/terraform$ terraform state show aws_s3_bucket.my_bucket
# aws_s3_bucket.my_bucket:
resource "aws_s3_bucket" "my_bucket" {
    acceleration_status         = null
    arn                         = "arn:aws:s3:::erra-bucket-007"
    bucket                      = "erra-bucket-007"
    bucket_domain_name          = "erra-bucket-007.s3.amazonaws.com"
    bucket_namespace            = "global"
    bucket_prefix               = null
    bucket_region               = "us-west-2"
    bucket_regional_domain_name = "erra-bucket-007.s3.us-west-2.amazonaws.com"
    force_destroy               = false
    hosted_zone_id              = "Z3BJ6K6RIION7M"
    id                          = "erra-bucket-007"
    object_lock_enabled         = false
    policy                      = null
    region                      = "us-west-2"
    request_payer               = "BucketOwner"
    tags                        = {
        "Name" = "My bucket"
    }
    tags_all                    = {
        "Name" = "My bucket"
    }

    grant {
        id          = "8e5d1356408dc92d2592651c989b07d3f6a6ff0ee4b56aff68cbc975882cb5f0"
        permissions = [
            "FULL_CONTROL",
        ]
        type        = "CanonicalUser"
        uri         = null
    }

    server_side_encryption_configuration {
        rule {
            bucket_key_enabled = false

            apply_server_side_encryption_by_default {
                kms_master_key_id = null
                sse_algorithm     = "AES256"
            }
        }
    }

    versioning {
        enabled    = false
        mfa_delete = false
    }
}

```

3. Answer these questions in your notes:
   - What information does the state file store about each resource?
   - Why should you never manually edit the state file?
   - Why should the state file not be committed to Git?

The state file stores the metadata of the resources created. 
It also stores the resource ID, current configuration and the attributes of the resource.

manually editing the state file will lead to conflict between the actual state and desired state of resources.
It breaks the terraform's source of truth and can lead to a corrupt state file 

state file should not be committed to Git as it contains sensitive data (credentials, ARNs, endpoints). This will cause security leaks.
It should be stored in remote backends such as S3 bucket.

---

### Task 6: Modify, Plan, and Destroy
1. Change the EC2 instance tag from `"TerraWeek-Day1"` to `"TerraWeek-Modified"` in your `main.tf`
```
resource "aws_instance" "my_instance" {
  ami           = "ami-043ab4148b7bb33e9"
  instance_type = "t3.micro"

  tags = {
    Name = "TerraWeek-Modified"
  }
}

resource "aws_ec2_instance_state" "instance_state" {
  instance_id = aws_instance.my_instance.id
  state       = "stopped"

}
```
   
2. Run `terraform plan` and read the output carefully:
   - What do the `~`, `+`, and `-` symbols mean?
   - Is this an in-place update or a destroy-and-recreate?

Terraform updates in-place whereever there is a change.
It will not destroy and recreate the entire resource

```
Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  ~ update in-place

Terraform will perform the following actions:

  # aws_instance.my_instance will be updated in-place
  ~ resource "aws_instance" "my_instance" {
        id                                   = "i-0c14d27a2a253e6ad"
      ~ tags                                 = {
          ~ "Name" = "TerraWeek-Day1" -> "TerraWeek-Modified"
        }
      ~ tags_all                             = {
          ~ "Name" = "TerraWeek-Day1" -> "TerraWeek-Modified"
        }
        # (39 unchanged attributes hidden)

        # (9 unchanged blocks hidden)
    }

Plan: 0 to add, 1 to change, 0 to destroy.
```
Applied the change `terraform apply`

3. Finally, destroyed everything:
```
terraform destroy
```





























   

   
