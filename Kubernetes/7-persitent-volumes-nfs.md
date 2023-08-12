## Creating and testing NFS Server
### In Local RedHat VM

```bash
sudo -i
cd /
mkdir mydata1
vim /etc/exports
# add below line to exports
/mydata1                *(rw,no_root_squash,insecure)
# (or) /mydata1                192.168.37.129(rw,no_root_squash)

yum install nfs-utils -y

systemctl start nfs-server
# see which folder is shared
exportfs -v
# to reload exports file
exportfs -r

ifconfig
192.168.37.128
```
### Minikube
```bash
minikube ssh
sudo -i
mkdir /opt/mydata2
# mount ip:nfs host
mount 192.168.37.128:/mydata1 /opt/mydata2
unmount /opt/mydata2
```

### RedHat VM
```bash
# to check logs
cat /var/log/messages
```
## Creating Storage
### pvc.yaml
```bash
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mypvc
spec:
  resources:
    requests:
      storage: 10Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: ""
```
### pv.yaml
```bash
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mypv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: ""
  nfs:
    path: /mydata1
    server: 192.168.37.128
  persistentVolumeReclaimPolicy: Retain
```
## vimdep.yaml
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: vimdep
spec:
  replicas: 2
  selector:
    matchLabels:
      type: php
  template:
    metadata:
     name: vimpod
     labels:
      type: php
    spec:
      volumes:
        - name: anyname
          persistentVolumeClaim:
            claimName: mypvc
      containers:
        - name: vimcon
          image: vimal13/apache-webserver-php
          volumeMounts:
            - mountPath: "/var/www/html"
              name: anyname
```
### checking
```bash
kubectl exec -it pod -- bash
df -h
# ip:/mydata1
```
