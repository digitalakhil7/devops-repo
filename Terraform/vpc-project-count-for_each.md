## VPC + SG + EC2 + EIP
VPC - VPC, SN (public), IGW, RT, RT Association
```bash
provider "aws" {
  region = "ap-south-1"
  profile = "default"
}

resource "aws_vpc" "avVPC" {
  cidr_block       = "10.0.0.0/16"
  instance_tenancy = "default"

  tags = {
    Name = "avVPC"
  }
}

resource "aws_subnet" "avSN1" {
  availability_zone = "ap-south-1a"
  map_public_ip_on_launch = true
  vpc_id     = aws_vpc.avVPC.id
  cidr_block = "10.0.1.0/24"

  tags = {
    Name = "avSN1"
  }
}

resource "aws_internet_gateway" "avIGW" {
  vpc_id = aws_vpc.avVPC.id

  tags = {
    Name = "avIGW"
  }
}

resource "aws_route_table" "avRT1" {
  vpc_id = aws_vpc.avVPC.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.avIGW.id
  }

  tags = {
    Name = "avRT1"
  }
}

resource "aws_route_table_association" "avRTA1" {
  subnet_id      = aws_subnet.avSN1.id
  route_table_id = aws_route_table.avRT1.id
}

resource "aws_security_group" "avSG" {
  name        = "avSGTF"
  description = "av security group from terraform"
  vpc_id      = aws_vpc.avVPC.id

  ingress {
    description      = "SSH All"
    from_port        = 22
    to_port          = 22
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
    ipv6_cidr_blocks = ["::/0"]
  }
  ingress {
    description      = "HTTP All"
    from_port        = 80
    to_port          = 80
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
    ipv6_cidr_blocks = ["::/0"]
  }

  egress {
    from_port        = 0
    to_port          = 0
    protocol         = "-1"
    cidr_blocks      = ["0.0.0.0/0"]
    ipv6_cidr_blocks = ["::/0"]
  }

  tags = {
    Name = "avSGTF"
  }
}

resource "aws_instance" "avOS" {
  ami           = "ami-0da59f1af71ea4ad2"
  instance_type = "t2.micro"

  key_name = "terraform"
  subnet_id = aws_subnet.avSN1.id
  vpc_security_group_ids = [aws_security_group.avSG.id]
  user_data = <<-EOF
    #!/bin/bash
    sudo -i
     yum update -y
     yum install httpd -y
     cd /var/www/html/
     echo "god will make a way" > index.html
     service httpd start
     chkconfig httpd on
    EOF
#   user_data = file("abc.sh") - chmod +x abc.sh
  tags = {
    Name = "avOS"
  }
}

resource "aws_eip" "avEIP" {
  instance = aws_instance.avOS.id
  domain   = "vpc"
# EIP should be created after IGW is created
  depends_on = [ aws_internet_gateway.avIGW ]
}
```

### count, for_each - Meta Arguments
```bash
provider "aws" {
  region = "ap-south-1"
  profile = "default"
}

variable "myOS" {
  type = map
  default = {
    slow = "windows"
    fast = "linux"
    medium = "mac"
  }
}

resource "aws_instance" "avOS" {
  for_each = var.myOS
  ami           = "ami-0da59f1af71ea4ad2"
  instance_type = "t2.micro"
  tags = {
    Name = "avOS-${each.value}"
  }
}

resource "aws_s3_bucket" "mybuc" {
  for_each = toset(["dev","test","prod"])
  bucket = "akhilvee-${each.key}"

  tags = {
    Name        = "My bucket in ${each.key}"
    Environment = each.key
  }
}

resource "aws_iam_user" "myIAM" {
  for_each = toset(["mom","dad"])
  name = each.key
}
```
