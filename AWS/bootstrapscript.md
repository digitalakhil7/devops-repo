```sh
#!/bin/bash
sudo -i
yum update -y
yum install httpd -y
cd /var/www/html/
echo "God is Amazing" > index.html
service httpd start
chkconfig httpd on
```
