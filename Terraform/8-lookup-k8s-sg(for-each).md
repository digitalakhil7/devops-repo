### lookup function
```bash
# Getting Ami based on region using lookup function
# Syntax: lookup(map,key,default)
variable "region" {
  default = "ap-south-1"
}

variable "ami" {
  type = map
  default = {
    "ap-south-1" = "ami-1111"
    "eu-east-1" = "ami-2222"
    "eu-west-1" = "ami-3333"
  }
}

output "o1" {
  value = lookup(var.ami,var.region,"ami-4444")
}
```
### K8S + Terraform
```bash
# https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs

provider "kubernetes" {
  config_path    = "~/.kube/config"
  config_context = "minikube"
}

# Creating Pod
resource "kubernetes_pod" "mypod" {
  metadata {
    name = "vimpod"
  }

  spec {
    container {
      image = "vimal13/apache-webserver-php"
      name  = "vimcontainer"

      env {
        name  = "environment"
        value = "test"
      }
    }
  }
}

# Creating Deployment
resource "kubernetes_deployment" "mydep" {
  metadata {
    name = "vimdep"
    labels = {
      test = "MyExampleApp"
    }
  }

  spec {
    replicas = 3

    selector {
      match_labels = {
        test = "MyExampleApp"
      }
    }

    template {
      metadata {
        labels = {
          test = "MyExampleApp"
        }
      }

      spec {
        container {
          image = "vimal13/apache-webserver-php"
          name  = "vimcon"
          }
        }
      }
    }
  }
```

### Sg on AWS - Dynamic block concept using for_each
```bash
provider "aws" {
  region     = "ap-south-1"
    profile = "default"
}

resource "aws_security_group" "sg1" {
  name        = "mysgtf"

  ingress {
    from_port        = 80
    to_port          = 80
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
  }
}

# Making Dynamic
variable "sgports" {
  type = list
  default = [81,82,8080,8081]
}
resource "aws_security_group" "sg2" {
  name        = "mydynsgtf"

  dynamic "ingress" {
    for_each = var.sgports
    content {
    from_port        = ingress.value
    to_port          = ingress.value
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
    }
  }
}
```
