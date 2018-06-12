## Cheat Sheet

### Kubernetes Basics

```
kubectl run nginx-demo --image=nginx --replicas=1 
```

```
kubectl get deployments,rs,pods
```

```
kubectl expose deploy nginx-demo --type=LoadBalancer --port=80
```
```
kubectl get pods -o wide
kubectl get services,endpoints
```

```
kubectl run -i --tty busybox --image=busybox --restart=Never -- sh

# nslookup $svc
```



