## MYSQL + WordPress

### MySQL
```bash
kubectl run --image=mysql:5.7 --env=MYSQL_ROOT_PASSWORD=akhil --env=MYSQL_DATABASE=mydb --env=MYSQL_USER=akhil --env=MYSQL_PASSWORD=akhil mydb
```
```bash
kubectl exec -it mydb -- bash
mysql -u akhil -pakhil
# mysql> will come
```

### WordPress
```bash
kubectl run --image=wordpress:5.1.1-php7.3-apache mywp
kubectl expose pod mywp --port=80 --type=NodePort
```
