## Jenkins
```bash
gcloud compute instances create jenkinsmaster --zone=us-west4-b --machine-type=e2-medium --create-disk=auto-delete=yes,boot=yes,device-name=jenkins,image=projects/centos-cloud/global/images/centos-7-v20221206,mode=rw,size=20
```
```bash
sudo -i
yum install wget -y

# official link
https://pkg.jenkins.io/redhat-stable/

sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
yum install fontconfig java-11-openjdk
yum install jenkins

systemctl status jenkins
systemctl start jenkins
```

## Important Links
```bash
https://www.jenkins.io/doc/book/pipeline/syntax/
https://www.jenkins.io/doc/pipeline/steps/workflow-basic-steps/
```

## Important Points
```bash
Linux Commands - sh
echo - inbuilt
script - Groovy code
```
## Jenkins Slave
root user, PA yes, restart ssh, mkdir jenkins
```bash
sudo -i
useradd akhil
passwd akhil
usermod -aG wheel akhil (sudo persmissions)
vim /etc/ssh/sshd_config (PasswordAuthentication yes)
service sshd restart

sudo - akhil
cd
mkdir jenkins (/home/akhil/jenkins)

Install Java 11, 17, Maven, Git and required software
```
