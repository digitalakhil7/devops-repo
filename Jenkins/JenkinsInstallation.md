## Jenkins
```bash
gcloud compute instances create jenkinsmaster --zone=us-west4-b --machine-type=e2-medium --create-disk=auto-delete=yes,boot=yes,device-name=jenkins,image=projects/centos-cloud/global/images/centos-stream-8-v20240110,mode=rw,size=20
```
```bash
sudo -i
yum install wget -y

# official link - ***Install Jenkins and Java from official link only***
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
```bash
gcloud compute instances create javaslave --zone=us-west4-b --machine-type=e2-small --create-disk=auto-delete=yes,boot=yes,device-name=javaslave,image=projects/centos-cloud/global/images/centos-7-v20221206,mode=rw,size=20
```
```bash
gcloud compute instances create nodeslave --zone=us-west4-b --machine-type=e2-small --create-disk=auto-delete=yes,boot=yes,device-name=nodeslave,image=projects/centos-cloud/global/images/centos-7-v20221206,mode=rw,size=20
```
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
## Steps to install node on ubuntu
### Update System Packages
sudo yum update -y

###  Install Development Tools
sudo yum groupinstall "Development Tools" -y

###  Install Node.js using NodeSource repository:
```bash
curl -sL https://rpm.nodesource.com/setup_14.x | sudo bash -
sudo yum install -y nodejs
```

### Verify the Node.js installation:
node -v
npm -v

### Install Java 11
```bash
yum search jdk
yum install java-11-openjdk-devel.i686 -y
```

### Sonar for node
```bash
npm run sonar
# If issues do following
npm cache clean --force
```
