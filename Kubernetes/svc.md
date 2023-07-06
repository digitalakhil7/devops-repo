### Service

to implement Load Balancing and reverse proxy.

```bash
apiVersion: v1
kind: Service
metadata:
  name: svc1
spec:
  type: NodePort
  ports:
    - port: 3000
      targetPort: 80
  selector:
    app: svc1-app
```

### Pod1

```bash
apiVersion: v1
kind: Pod
metadata:
  name: pod1
  labels:
    app: svc1-app
spec:
  containers:
    - name: myc2
      image: vimal13/apache-webserver-php
```


### Pod2

```bash
apiVersion: v1
kind: Pod
metadata:
  name: pod2
  labels:
    app: svc1-app
spec:
  containers:
    - name: myc3
      image: vimal13/apache-webserver-php
```

#### Commands

```bash
kubectl create -f svc.yaml/pod1.yaml/pod2.yaml

# Check Endpoints, there should be 2 end points.
kubectl describe svc svc1

minikube service svc1 --url
```
