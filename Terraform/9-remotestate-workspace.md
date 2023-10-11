### Steps
* Create S3 bucket with versioning enabled (manually or using tf)
* Create S3 backend using terraform to maintain remote state (.tfstate file)
* Lock state using DynamoDB (only one user can use state file at once)

Remote State - S3, Lock - DynamoDB

```bash
provider "aws" {
  region = "ap-south-1"
  profile = "defaut"
}

resource "aws_instance" "os1" {
  ami           = "ami-0da59f1af71ea4ad2"
  instance_type = "t2.micro"
  tags = {
    Name = "akhilvm"
  }
}

terraform {
  backend "s3" {
    bucket = "myavbuc"
    key    = "my.tfstate"
    region = "ap-south-1"
    access_key = "AKIA2QJXHO75F"
    secret_key = "DfIRZzIKkB6/vuj5bnkEx7my"
    dynamodb_table = "TfStateLock"
  }
}

resource "aws_dynamodb_table" "basic-dynamodb-table" {
  name           = "TfStateLock"
  billing_mode   = "PROVISIONED"
  read_capacity  = 5
  write_capacity = 5
  hash_key       = "LockID"

  attribute {
    name = "LockID"
    type = "S"
  }
}
```
### Testing
```bash
# Use 2 terminals and do
terraform apply
```

## Workspace
```bash
variable "type" {
  type = map
  default = {
    dev = "aws"
    test = "azure"
    prod = "gcp"
  }
}

output "o2" {
  value = lookup(var.type,terraform.workspace)
}
```
