### Replication Controller

Used for monitoring, Load Balancing and Scaling

```bash
apiVersion: v1
kind: ReplicationController
metadata:
  name: rc1
spec:
  template:
    metadata:
      name: pod1
      labels:
        app: myapp
    spec:
      containers:
        - name: myc1
          image: vimal13/apache-webserver-php
  selector:
    app: myapp
  replicas: 4
```

#### Commands

```bash
kunectl create -f rc.yaml
kubectl get pods -L app

kubectl expose rc rc1 --port=80 --type=NodePort
minikube service rc1 --url

kunectl delete pod rc1-xxxx

# After changing replicas in yaml
kunectl apply -f rc.yaml
(or)
kubectl scale rc rc1 --replicas=4
```

#### Observations

Monitoring - a new pod is created when we delete a new pod

Load Balancing - IP of pod is changed for every refresh

Scaling - use kubectl apply -f rc.yaml after changing replicas
