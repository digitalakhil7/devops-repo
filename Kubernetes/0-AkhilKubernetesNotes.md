## Pod
Smallest object that we can create in K8S.
```
apiVersion: v1
kind: Pod
metadata:
  name: podname
  labels:
    type: front-end
spec:
  containers:
    - name: containername
      image: nginx
```
#### Commands
```
k create -f pod.yaml
k expose pod podname –port=80 –type=NodePort
k get svc
Browser: ip:port(from svc)
```
## ReplicationController(Old) - TR (Template, Replicas)
High Availability / Monitoring - a new pod is created automatically if a pod is deleted <br>
Load Balancing & Scaling
```
apiVersion: v1
kind: ReplicationController
metadata:
  name: rc-name
  labels:
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: podname
      labels:
        type: front-end
        app: myapp
    spec:
      containers:
        - name: containername
          image: nginx
  replicas: 4
```
#### Commands
```
k create -f rc.yaml
k expose rc rc-name –port=80 –type=NodePort
k get svc
Browser: ip:port(from svc)
```
## ReplicaSet(New) - TRS ( Template, Replicas, Selector(matchLabels) )
**Note:** labels in matchLabels and template should match <br>
High Availability / Monitoring - a new pod is created automatically if a pod is deleted <br>
Load Balancing & Scaling
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: rs-name
  labels:
    name: myapp
    type: front-end
spec:
  template:
    metadata:
      name: podname
      labels:
        type: front-end
        app: myapp
    spec:
      containers:
        - name: containername
          image: nginx
  replicas: 4
  selector:
    matchLabels:
      app: myapp
```
#### commands
```
k create -f rs.yaml
k expose rs rs-name --port=80 --type=NodePort
k get svc
Browser: ip:port(from svc)
```
#### Scaling
```
k scale -f rs.yaml --replicas=4
(or)
change replicas in yaml file and
k apply -f rs.yaml
(or)
k scale rs rs-name --replicas=4
```
## Deployment - TRS ( Template, Replicas, Selector(matchLabels) )
Pod + ReplicaSet (Monitoring, LB, Scaling) + Rolling Update
```
yaml file same as ReplicaSet, replace kind with Deployment
```
## Service
NodePort <br>
ClusterIP (default) <br>
LoadBalancer
### NodePort
![1-Service-Ports](https://github.com/user-attachments/assets/e0a51e18-ca69-422a-98c9-d42c5e8eed6c)
#### Pre-requisite (POD)
**Note:** labels in **POD** and labels in **selector** of service should match
```
apiVersion: v1
kind: Pod
metadata:
  name: podname
  labels:
    type: front-end
    app: myapp
spec:
  containers:
    - name: containername
      image: nginx
```
#### NodePort service
```
apiVersion: v1
kind: Service
metadata:
  name: svc-name
spec:
  type: NodePort
  ports:
    - targetPort: 80 # (optional) targetPort = port
      port: 80 # (mandatory)
      nodePort: 30002 #(optional) 30000 to 32767
  selector:
    type: front-end
    app: myapp
```
#### ClusterIP service
doesn't have nodePort
```
apiVersion: v1
kind: Service
metadata:
  name: svc-name
spec:
  type: ClusterIP
  ports:
    - targetPort: 80 # (optional) targetPort = port
      port: 80 # (mandatory)
  selector:
    type: front-end
    app: myapp
```
#### LoadBalancer service
```
apiVersion: v1
kind: Service
metadata:
  name: svc-name
spec:
  type: LoadBalancer
  ports:
    - targetPort: 80 # (optional) targetPort = port
      port: 80 # (mandatory)
      nodePort: 30003 #(optional) 30000 to 32767
  selector:
    type: front-end
    app: myapp
```
