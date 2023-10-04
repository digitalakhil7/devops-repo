### vpc.tf
```bash
provider "aws" {
  region     = var.aws_region
  profile = "default"
}

resource "aws_vpc" "avVPC" {
  cidr_block       = var.vpc_cidr
  instance_tenancy = "default"

  tags = {
    Name = "avVPC"
  }
}

resource "aws_subnet" "avSN" {
  count = length(var.subnets_cidr)
  vpc_id     = aws_vpc.avVPC.id
  cidr_block = element(var.subnets_cidr,count.index)
  availability_zone = element(var.azs,count.index)
  map_public_ip_on_launch = true
  tags = {
    Name = "avSN-${count.index + 1}"
  }
}

resource "aws_internet_gateway" "avIGW" {
  vpc_id = aws_vpc.avVPC.id
  
  tags = {
    Name = "avIGW"
  }
}

resource "aws_route_table" "avRT" {
  vpc_id = aws_vpc.avVPC.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.avIGW.id
  }

  tags = {
    Name = "avRT"
  }
}

resource "aws_route_table_association" "a" {
  count = length(var.subnets_cidr)
  subnet_id      = element(aws_subnet.avSN.*.id, count.index)
  route_table_id = aws_route_table.avRT.id
}
```

### variables.tf
```bash
variable "aws_region" {
  default = "ap-south-1"
}

variable "vpc_cidr" {
  default = "10.0.0.0/16"
}

variable "subnets_cidr" {
  type = list
  default = ["10.0.1.0/24","10.0.2.0/24"]
}

variable "azs" {
  default = ["ap-south-1a","ap-south-1b"]
}
```
