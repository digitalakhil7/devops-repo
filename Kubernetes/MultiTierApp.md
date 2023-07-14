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

## Using YAML
mysql.yaml
```bash
apiVersion: v1
kind: Pod
metadata:
  name: mydb
  labels:
    type: db
spec:
  containers:
    - name: mysqlcon
      image: mysql:5.7.42
      env:
        - name: MYSQL_ROOT_PASSWORD
          value: akhil
        - name: MYSQL_DATABASE
          value: mydb
        - name: MYSQL_USER
          value: akhil
        - name: MYSQL_PASSWORD
          value: akhil
```

wordpress.yaml
```bash
apiVersion: v1
kind: Pod
metadata:
  name: mywp
  labels:
    type: php
spec:
  containers:
    - name: wpcon
      image: wordpress:5.1.1-php7.3-apache
```

wpservice.yaml
```bash
apiVersion: v1
kind: Service
metadata:
  name: wpsvc
spec:
  type: NodePort
  ports:
    - port: 80
  selector:
    type: php
```
