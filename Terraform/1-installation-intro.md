## Installation
### Windows
Download terrfaorm and copy path to environment varilables
```bash
https://developer.hashicorp.com/terraform/downloads
```
### Linux
```bash
sudo -i
cd /opt
wget https://releases.hashicorp.com/terraform/1.5.5/terraform_1.5.5_linux_386.zip
unzip terraform_1.5.5_linux_386.zip
echo $PATH
mv terraform /bin

#Testing
terraform
terraform version
```

## Introduction
Infra can be created by using
* WebUI
* CLI
* API

Terraform is used for
* Creating Infra
* Switching Clouds

### Terraform Commands
ivpad - init validate plan apply destroy
```bash
terraform init -upgrade
terraform apply -auto-approve
terraform destroy --auto-approve
terraform apply -h
```
### Important Terms
```bash
Provisioning / Resources - RAM/CPU/HD + OS
Provider - AWS / Azure / GCP
WAS Server - Configuration Management

Desired State, Current State
Imperative Language - Python, Scripting
Declaration Language - Terraform - if reource is not present then only apply
```


