## PVC, PV, SC
SAS - storage, accessModes, storageClassName
## PVC - PersistentVolumeClaims

```bash
https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims
```
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
## Persistent Volumes
```bash
minikube ssh
sudo mkdir /mydata
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
  hostPath:
    path: /mydata
```
## Attaching PVC to Deployment
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
## Checking Persitent Storage in host path and container path
```bash
kubectl exec -it pod -- bash
cd /var/www/html/
echo "hello akhil" > index.php

minikube ssh
cd /mydata
ls
#index.php
cat index.php
```
## Deleting PVC
To delete PVC we need to delete POD first because PVC is attached to POD
```bash
kubectl delete pvc mypvc
kubectl delete pods --all
```
## Reclaim Policy in PV
**Retain(default)** - **pv** cannot be reused, but **data** is retained<br>
**Recycle** - **pv** can be reused but **data** is lost<br>
**Delete** - delete the storage in (EBS)

### Retain
```bash
kubectl delete pvc mypvc
kubectl delete pods --all
# check Reclaim Policy
kubectl get pv
# pv won't get attached to new pvc because of Retain - Reclaim Policy
kubectl create -f pvc.yaml
# status shows as released only
kubectl get pvc
``` 
### Recycle
```bash
# change Reclaim Policy to Recycle
kubectl edit pv mypv
kubectl delete pvc mypvc
kubectl delete pods --all
# check Reclaim Policy
kubectl get pv
# pv gets attached to pvc because of Recycle - Reclaim Policy
kubectl create -f pvc.yaml
# status shows as released -> available -> bound
kubectl get pvc
```
