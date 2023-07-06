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
