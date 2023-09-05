## Passing values to variables
* default
* Command line: -var="key=value"
* terraform.tfvars: -var-file="fileName.tfvars"

### variable.tf
```bash
# variable "x" {
#   type = string
#   default = "t2.micro"
# }

variable "x" {
  type = string
}

variable "y" {
  type = bool
}

output "o1" {
  value = var.y
  # acts as command line argument
}

output "os2" {
  value = var.x
  # t2.micro
}
```

### terraform.tfvars
```bash
x="t2.micro"
```

### testing
```bash
terraform init
terraform plan
```
