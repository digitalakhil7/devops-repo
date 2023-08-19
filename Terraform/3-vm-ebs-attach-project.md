### Variable declaration and printing
#### Data Manipulation / Data Reference
```bash
# declaration
variable "x" {
  type = string
  default = "Linux World"
}

# printing
output "name" {
  value = "${var.x}"
}
```
### Project Steps
* Create VM
* Create EBS Volume
* Attach Both

```bash
# AWS Docs: https://registry.terraform.io/providers/hashicorp/aws/latest/docs

# https://registry.terraform.io/providers/hashicorp/aws/latest/docs#authentication-and-configuration
provider "aws" {
  region     = "ap-south-1"
  # access_key = "AKIA2QJXJ"
  # secret_key = "OZDQdIAxs1uM2G80oC"
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

output "az" {
  # value = aws_instance.os1 - to get all key-value pairs
  value = aws_instance.os1.availability_zone
}

# https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ebs_volume
resource "aws_ebs_volume" "ebs" {
  availability_zone = aws_instance.os1.availability_zone
  size              = 10

  tags = {
    Name = "AkhilEBS"
  }
}

output "id" {
  # value = aws_ebs_volume.ebs.id - to get all key-value pairs
  value = aws_ebs_volume.ebs.id
}

# https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/volume_attachment
resource "aws_volume_attachment" "ebs_att" {
  device_name = "/dev/sdh"
  volume_id   = aws_ebs_volume.ebs.id
  instance_id = aws_instance.os1.id
}

output "ebs_att_vals" {
  value = aws_volume_attachment.ebs_att
}
```
