## Deployment
Used for RollingUpdates + Monitoring, Load Balancing and Scaling.
### Creating Deployment
```bash
kubectl create deployment --image=httdp httpdep
kubectl scale deployment --replicas=3
kubectl expose deployment httpdep --port=80 --type=NodePort
```
### Perform RollingUpdate and Checking
```bash
# Syntax: kubectl set image deployment deployment-name <container-name=image-name>
kubectl set image deployment httpdep httpd=httpd:2.4.57

#Checking
kubectl get pods
curl ip
```
### rollout commands
```bash
kubectl rollout status deployment/httpdep
kubectl rollout history deployment/httpdep

# Rollback
kubectl rollout undo deployment/httpdep
```

