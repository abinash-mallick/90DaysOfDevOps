### Task 1: Explore the AWS Provider
1. Create a new project directory: `terraform-aws-infra`
2. Write a `providers.tf` file:
   - Define the `terraform` block with `required_providers` pinning the AWS provider to version `~> 5.0`
   - Define the `provider "aws"` block with your region
3. Run `terraform init` and check the output -- what version was installed?
4. Read the provider lock file `.terraform.lock.hcl` -- what does it do?

**Document:** What does `~> 5.0` mean? How is it different from `>= 5.0` and `= 5.0.0`?

```
terraform {

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-west-2"
}
```

As we had mentioned the version to be ~> 5, the version installed will be any version in 5.x, but not 6.x automatically

`.terraform.lock.hcl` file locks the exact provider version that terraform will use. It prevents unexpected upgrades. Even if a new version is released, terraform wont use that.

`~> 5.0` means terraform will istall the version in 5.x but not 6.x automatically. >= 5.0 means terraform init will install any version above 5.0. it can be any version like 6.4 or 7.1 etc.
`= 5.0.0` means terraform will install the exact version of 5.0.0. No other version.

---

### Task 2: Build a VPC from Scratch
Create a `main.tf` and define these resources one by one:

1. `aws_vpc` -- CIDR block `10.0.0.0/16`, tag it `"TerraWeek-VPC"`
2. `aws_subnet` -- CIDR block `10.0.1.0/24`, reference the VPC ID from step 1, enable public IP on launch, tag it `"TerraWeek-Public-Subnet"`
3. `aws_internet_gateway` -- attach it to the VPC
4. `aws_route_table` -- create it in the VPC, add a route for `0.0.0.0/0` pointing to the internet gateway
5. `aws_route_table_association` -- associate the route table with the subnet

```
resource "aws_vpc" "my_vpc" {

    cidr_block = "10.0.0.0/16"
    tags = {
      Name = "TerraWeek-VPC"
    }
  
}

resource "aws_subnet" "my_subnet" {

    vpc_id = aws_vpc.my_vpc.id
    cidr_block = "10.0.1.0/24"
    tags = {
      Name = "TerraWeek-Public-Subnet"
    }
}

resource "aws_internet_gateway" "my_gateway" {

    vpc_id = aws_vpc.my_vpc.id
    tags = {
      Name = "I-gate"
    }
  
}

resource "aws_route_table" "my_route" {
  vpc_id = aws_vpc.my_vpc.id
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.my_gateway.id
  }
}

resource "aws_route_table_association" "my_rta" {
  subnet_id      = aws_subnet.my_subnet.id
  route_table_id = aws_route_table.my_route.id
}

```

**Verify:** Apply and check the AWS VPC console. Can you see all five resources connected?

Yes

---

- How does Terraform know to create the VPC before the subnet?
  Terraform determines resource creation order using implicit dependencies by analyzing references between resources and building a dependency graph.
  
- What would happen if you tried to create the subnet before the VPC existed?
  The resource creation will fail since the dependency graph will not be satisfied.
  
- Find all implicit dependencies in your config and list them

  ```
  1. aws_subnet.my_subnet → aws_vpc.my_vpc
     (via vpc_id)

  2. aws_internet_gateway.my_gateway → aws_vpc.my_vpc
     (via vpc_id)
  
  3. aws_route_table.my_route → aws_vpc.my_vpc
     (via vpc_id)
  
  4. aws_route_table.my_route → aws_internet_gateway.my_gateway
     (via gateway_id inside route block)
  
  5. aws_route_table_association.my_rta → aws_subnet.my_subnet
     (via subnet_id)
  
  6. aws_route_table_association.my_rta → aws_route_table.my_route
     (via route_table_id)
  ```

### Task 4: Add a Security Group and EC2 Instance
Add to your config:

1. `aws_security_group` in the VPC:
   - Ingress rule: allow SSH (port 22) from `0.0.0.0/0`
   - Ingress rule: allow HTTP (port 80) from `0.0.0.0/0`
   - Egress rule: allow all outbound traffic
   - Tag: `"TerraWeek-SG"`

2. `aws_instance` in the subnet:
   - Use Amazon Linux 2 AMI for your region
   - Instance type: `t2.micro`
   - Associate the security group
   - Set `associate_public_ip_address = true`
   - Tag: `"TerraWeek-Server"`

  ```
resource "aws_security_group" "my_sg" {

    vpc_id = aws_vpc.my_vpc.id
    tags = {
      Name = "TerraWeek-SG"
    }  
}

resource "aws_vpc_security_group_ingress_rule" "allow_ssh" {
  security_group_id = aws_security_group.my_sg.id
  cidr_ipv4         = aws_vpc.my_vpc.cidr_block
  from_port         = 22
  ip_protocol       = "tcp"
  to_port           = 22
}

resource "aws_vpc_security_group_ingress_rule" "allow_http" {
  security_group_id = aws_security_group.my_sg.id
  cidr_ipv4         = aws_vpc.my_vpc.cidr_block
  from_port         = 80
  ip_protocol       = "tcp"
  to_port           = 80
}

resource "aws_vpc_security_group_egress_rule" "allow_all_traffic_ipv4" {
  security_group_id = aws_security_group.my_sg.id
  cidr_ipv4         = "0.0.0.0/0"
  ip_protocol       = "-1"
}


resource "aws_instance" "my_ec2" {

    ami = "ami-043ab4148b7bb33e9"
    instance_type = "t3.micro"
    subnet_id     = aws_subnet.my_subnet.id
    vpc_security_group_ids = [aws_security_group.my_sg.id]
    associate_public_ip_address = true
    tags = {
        Name = "TerraWeek-Server"
    }
}

resource "aws_ec2_instance_state" "my_ec2" {
  instance_id = aws_instance.my_ec2.id
  state = "stopped"
}
```
---

### Task 5: Explicit Dependencies with depends_on

1. Add a second `aws_s3_bucket` resource for application logs
2. Add `depends_on = [aws_instance.main]` to the S3 bucket -- even though there is no direct reference, you want the bucket created only after the instance
3. Run `terraform plan` and observe the order

```
resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-bkt-unq-007"
  depends_on = [ aws_instance.my_ec2 ]
}
```

```
terraform graph

digraph G {
  rankdir = "RL";
  node [shape = rect, fontname = "sans-serif"];
  "aws_ec2_instance_state.my_ec2" [label="aws_ec2_instance_state.my_ec2"];
  "aws_instance.my_ec2" [label="aws_instance.my_ec2"];
  "aws_internet_gateway.my_gateway" [label="aws_internet_gateway.my_gateway"];
  "aws_route_table.my_route" [label="aws_route_table.my_route"];
  "aws_route_table_association.my_rta" [label="aws_route_table_association.my_rta"];
  "aws_s3_bucket.my_bucket" [label="aws_s3_bucket.my_bucket"];
  "aws_security_group.my_sg" [label="aws_security_group.my_sg"];
  "aws_subnet.my_subnet" [label="aws_subnet.my_subnet"];
  "aws_vpc.my_vpc" [label="aws_vpc.my_vpc"];
  "aws_vpc_security_group_egress_rule.allow_all_traffic_ipv4" [label="aws_vpc_security_group_egress_rule.allow_all_traffic_ipv4"];
  "aws_vpc_security_group_ingress_rule.allow_http" [label="aws_vpc_security_group_ingress_rule.allow_http"];
  "aws_vpc_security_group_ingress_rule.allow_ssh" [label="aws_vpc_security_group_ingress_rule.allow_ssh"];
  "aws_ec2_instance_state.my_ec2" -> "aws_instance.my_ec2";
  "aws_instance.my_ec2" -> "aws_security_group.my_sg";
  "aws_instance.my_ec2" -> "aws_subnet.my_subnet";
  "aws_internet_gateway.my_gateway" -> "aws_vpc.my_vpc";
  "aws_route_table.my_route" -> "aws_internet_gateway.my_gateway";
  "aws_route_table_association.my_rta" -> "aws_route_table.my_route";
  "aws_route_table_association.my_rta" -> "aws_subnet.my_subnet";
  "aws_s3_bucket.my_bucket" -> "aws_instance.my_ec2";
  "aws_security_group.my_sg" -> "aws_vpc.my_vpc";
  "aws_subnet.my_subnet" -> "aws_vpc.my_vpc";
  "aws_vpc_security_group_egress_rule.allow_all_traffic_ipv4" -> "aws_security_group.my_sg";
  "aws_vpc_security_group_ingress_rule.allow_http" -> "aws_security_group.my_sg";
  "aws_vpc_security_group_ingress_rule.allow_ssh" -> "aws_security_group.my_sg";
}
```

### Task 6: Lifecycle Rules and Destroy

Normally, when an EC2 instance is replaced, the old instance gets deleted first before the new instance with new config is created.
This may cause some downtime.
With life cycle rules, the instance gets created first before the old one is destroyed.
This results in zero downtime and having a backup incause of any failure in creation of new instance

```
lifecycle {
  create_before_destroy = true
}
```




























