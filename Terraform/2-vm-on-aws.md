### Steps
main.tf, init, plan, apply<br>
Create user in IAM to get access_key and secret_key (Attach policy PowerUserAccess or AdministratorAccess)
### Don't hard code keys
```bash
# Install AWS CLI, > aws --version
aws configure list-profiles
aws configure
# Link this creds using profile
```
### main.tf
```bash
# AWS Docs: https://registry.terraform.io/providers/hashicorp/aws/latest/docs

# https://registry.terraform.io/providers/hashicorp/aws/latest/docs#authentication-and-configuration
provider "aws" {
  region     = "ap-south-1"
  # access_key = "AKIA2QJXHO7"
  # secret_key = "lzh59V0Vh2ikgwmKzLgIQKx"
    profile = "default"
}

# https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance
resource "aws_instance" "os1" {
  ami           = "ami-0da59f1af71ea4ad2"
  instance_type = "t2.micro"

  tags = {
    Name = "VmName"
  }
}
```
```bash
terraform init
terraform plan
terraform apply

# Change tag from VmName to Changed
terraform plan
terraform apply
```
