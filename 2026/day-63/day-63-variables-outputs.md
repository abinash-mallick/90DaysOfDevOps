### Task 1: Extract Variables
Take your Day 62 infrastructure config and refactor it:

1. Create a `variables.tf` file with input variables for:
   - `region` (string, default: your preferred region)
   - `vpc_cidr` (string, default: `"10.0.0.0/16"`)
   - `subnet_cidr` (string, default: `"10.0.1.0/24"`)
   - `instance_type` (string, default: `"t2.micro"`)
   - `project_name` (string, no default -- force the user to provide it)
   - `environment` (string, default: `"dev"`)
   - `allowed_ports` (list of numbers, default: `[22, 80, 443]`)
   - `extra_tags` (map of strings, default: `{}`)
  
  ```
variable "region" {
    default = "us-west-2"
    type = string
}

variable "ami_value" {
    default = "ami-043ab4148b7bb33e9"
    type = string
}

variable "vpc_cidr" {
    default = "10.0.0.0/16"
    type = string
}

variable "subnet_cidr" {
    default = "10.0.1.0/24"
    type = string
}

variable "instance_type" {
    default = "t3.micro"
    type = string
}

variable "project_name" {
    type = string
}

variable "environment" {
    default = "dev"
    type = string
}

variable "allowed_ports" {
    default = [22, 80, 443]
    type = list(number)
}

variable "extra_tags" {
  description = "Additional tags"
  type        = map(string)
  default     = {}
}
```

main.tf file
```
resource "aws_vpc" "my_vpc" {

    cidr_block = var.vpc_cidr
    tags = {
      Name = "${var.project_name}-vpc"
    }
  
}

resource "aws_subnet" "my_subnet" {

    vpc_id = aws_vpc.my_vpc.id
    cidr_block = var.subnet_cidr
    map_public_ip_on_launch = true
    tags = {
      Name = "${var.project_name}-Subnet"
    }
}

resource "aws_internet_gateway" "my_gateway" {

    vpc_id = aws_vpc.my_vpc.id
    tags = {
      Name = "${var.project_name}-gateway"
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

resource "aws_security_group" "my_sg" {

    vpc_id = aws_vpc.my_vpc.id
    tags = {
      Name = "${var.project_name}-SG"
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
  for_each = { for port in var.allowed_ports : port => port }
  security_group_id = aws_security_group.my_sg.id
  cidr_ipv4         = aws_vpc.my_vpc.cidr_block
  from_port         = each.value
  ip_protocol       = "tcp"
  to_port           = each.value
}

resource "aws_vpc_security_group_egress_rule" "allow_all_traffic_ipv4" {
  security_group_id = aws_security_group.my_sg.id
  cidr_ipv4         = "0.0.0.0/0"
  ip_protocol       = "-1"
}


resource "aws_instance" "my_ec2" {

    ami = var.ami_value
    instance_type = var.instance_type
    subnet_id     = aws_subnet.my_subnet.id
    vpc_security_group_ids = [aws_security_group.my_sg.id]
    associate_public_ip_address = true
    tags = {
        Name = "${var.project_name}-Server"
    }
}

resource "aws_ec2_instance_state" "my_ec2" {
  instance_id = aws_instance.my_ec2.id
  state = "stopped"
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "${var.project_name}-bkt-unq-007"
  depends_on = [ aws_instance.my_ec2 ]
}
```

---

### Task 2: Variable Files and Precedence
Create `terraform.tfvars`
```
# Dynamically passing the values to variables

project_name = "TerraAtlas"
environment = "test"
instance_type = "t2.micro"
```

Create `prod.tfvars`:
```
project_name = "terraweek"
environment  = "prod"
instance_type = "t3.small"
vpc_cidr     = "10.1.0.0/16"
subnet_cidr  = "10.1.1.0/24"
```

To apply this we need to pass this tfvars as argument durring terraform plan and apply

` terraform plan -var-file = "prod.tfvars" `

To Override with CLI:
```
terraform plan -var="instance_type=t2.nano"  # CLI overrides everything
```

Terraform has a rule:
Any environment variable starting with TF_VAR_ is treated as an input variable

` export TF_VAR_environment="stg" `

prority of different variables:

Priority 1 : variable passed as an argument in CLI
Priority 2 : variable passed through tfvars file
Priority 3 : variable passed through environment variables
Priority 4 : variable passed through variable.tf file
Priority 5 : default variable in variable.tf file





























