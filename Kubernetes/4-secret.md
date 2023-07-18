### secret.yaml
```bash
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
data:
  pwd: base64 encoded password
```

### mysql.yaml
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
          valueFrom:
            secretKeyRef:
              name: mysecret
              key: pwd
```
```bash
kubectl create -f secret.yaml
kubectl create -f mysql.yaml

kubectl get pod mydb -o yaml
kubectl get secret -o yaml
```
