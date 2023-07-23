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
