### Project Diagram
![projectpng](https://github.com/digitalakhil7/devops-repo/assets/63640168/00bd6466-71c0-4d21-96ba-36416bca2bf1)

### Manual Steps
```bash
# ami-06f621d90fa29f6d0
# t2.micro
# launch-wizard-1
# terraform.pem

ssh -i terraform.pem ec2-user@13.232.153.206
sudo -i
yum install httpd -y
yum install php -y
systemctl start httpd
systemctl enable httpd
```
### Other points
```bash
# create SG/pem using Terraform
# rpm -q php - to check php is installed
# rpm -q httpd - to check httpd is installed
# w - to check logged in users
```
## Main Code
```bash
provider "aws" {
  region = "ap-south-1"
  profile = "default"
}

resource "aws_instance" "os1" {
  ami = "ami-06f621d90fa29f6d0"
  instance_type = "t2.micro"
  security_groups = ["launch-wizard-1"]
  key_name = "terraform"
  tags = {
    Name = "WebServer2"
  }
  # https://developer.hashicorp.com/terraform/language/resources/provisioners/connection
  # to connect to vm via ssh
  connection {
    type     = "ssh"
    user     = "ec2-user"
    private_key = file("C:/Users/akhil/Downloads/terraform.pem")
    host     = aws_instance.os1.public_ip
  }
# https://developer.hashicorp.com/terraform/language/resources/provisioners/remote-exec
# to run commands on remote vm
# use Ansible in this place (recommended)
    provisioner "remote-exec" {
    inline = [
      "sudo yum install httpd -y",
      "sudo yum install php -y",
      "sudo systemctl start httpd",
      "sudo systemctl enable httpd"
    ]
  }
}
# https://registry.terraform.io/providers/hashicorp/null/latest/docs/resources/resource
# null_resource - if resource already exists (note: need to do terraform init when using null_resource)
resource "null_resource" "nullvmresource1" {
  connection {
    type     = "ssh"
    user     = "ec2-user"
    private_key = file("C:/Users/akhil/Downloads/terraform.pem")
    host     = aws_instance.os1.public_ip
  }
  provisioner "remote-exec" {
    inline = [
      "sudo yum install httpd -y",
      "sudo yum install php -y",
      "sudo systemctl start httpd",
      "sudo systemctl enable httpd"
    ]
  }
}

resource "aws_ebs_volume" "vol1" {
  availability_zone = aws_instance.os1.availability_zone
  size              = 1

  tags = {
    Name = "vol1"
  }
}

resource "aws_volume_attachment" "ebs_att" {
  device_name = "/dev/sdh"
  volume_id   = aws_ebs_volume.vol1.id
  instance_id = aws_instance.os1.id
}

output "hdname" {
  value = aws_volume_attachment.ebs_att
}

resource "null_resource" "nullvmresource2" {
  connection {
    type     = "ssh"
    user     = "ec2-user"
    private_key = file("C:/Users/akhil/Downloads/terraform.pem")
    host     = aws_instance.os1.public_ip
  }
  provisioner "remote-exec" {
    inline = [
      "sudo mkfs.ext4 /dev/xvdh",
      "sudo mount /dev/xvdh /var/www/html",
      "sudo yum install git -y",
      "sudo git clone https://github.com/vimallinuxworld13/gitphptest.git /var/www/html/"
    ]
  }
}

# Access website using ip/web if we use /var/www/html/web when cloning
# open url after downloading app from git
resource "null_resource" "nullvmresource3" {

  provisioner "local-exec" {
    command = "start firefox http://${aws_instance.os1.public_ip}/index.php"
  }
}
```
