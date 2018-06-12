## Kubernetes and GKE Meetup, June 2018

This is a repo containing the demos displayed at the June 2018 Silicon Valley Cloud-Native and Kubernetes Meetup in Sunnyvale, CA.

Please keep in mind billing applies to resources created in GCP. This is NOT an official Google product or tutorial.

### Kubernetes Basics

This section assumes that you have created a GKE cluster. If you have not, see the documentation [here](https://cloud.google.com/kubernetes-engine/docs/how-to/creating-a-cluster).

Create a Kubernetes Pod with an nginx container. This will be controlled by the Deployment controller, guaranteeing 1 replica of the Pod and a defined upgrade strategy.

```
kubectl run nginx-demo --image=nginx --replicas=1 
```

View how the Deployment controller, the ReplicaSet controller, and the Scheduler for Pods work together.

```
kubectl get deployments,rs,pods
```

Expose that nginx container to the public on port 80 with a Kubernetes Service, type=LoadBalancer. On Google Cloud Platform, this will front your nginx Pod with a Layer 3/4 Network Load Balancer.

```
kubectl expose deploy nginx-demo --type=LoadBalancer --port=80
```

View how the Endpoint controller maps the pod IP addresses to the Service. This Service will be the stable method for accessing the Pods, which are theoretically ephemeral and replacable. 

```
kubectl get pods -o wide
kubectl get services,endpoints
```

We also get a FQDN from our services so that we can resolve DNS local to our Kubernetes cluster.
```
kubectl run -i --tty busybox --image=busybox --restart=Never -- sh

# nslookup nginx-demo
```

### Global Presence with Multi-Cluster Ingress, aka Kubemci

[This](https://cloud.google.com/kubernetes-engine/docs/how-to/multi-cluster-ingress) tutorial demonstrates how to get up and running with Kubemci.

### GPUs "as a service" with GPU Node Pools

[This](https://cloud.google.com/kubernetes-engine/docs/concepts/gpus#gpu_pool) tutorial shows you how to add a GPU Node Pool to your existing GKE cluster.

Create a workload that requires GPU resources. Observe this requirement in the existing manifest for a Pod titled gpu-pod.

```
cat kubernetes/pods/gpu-pod.yaml
```

```
kubectl apply -f kubernetes/pods/gpu-pod.yaml
kubectl get pods -w
```
Open a new terminal and introspect the workload you just created.
```
kubectl describe pod gpu-pod
```
You should see a TriggeredScaleUp event. Once you delete the pod, you will see the node pool scale back down to zero. 

### Using Kubernetes Engine to Deploy Apps with Regional Persistent Disks

[This](https://cloud.google.com/solutions/using-kubernetes-engine-to-deploy-apps-with-regional-persistent-disks) solution demonstrates how to deploy an application with Regional Persistent Disks on Kubernetes Engine. It will also enable you to simulate a zonal failure to demonstrate the HA properties Regional PDs gives you. 
