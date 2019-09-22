## kubectl

### getting information

```bash
# get detailed information about pods
kubectl get pods -o wide
# list all pods, services and deployments
kubectl get all --all-namespaces
# list all serivce accounts
kubectl get sa --all-namespaces
# list all secrets
kubectl get secret --all-namespaces
# list pvc's
kubectl get pvc --all-namespaces
# get clusterrolebinding's
kubectl get clusterrolebinding
# list clusterissuer's
kubectl get clusterissuer
# list certificates
kubectl get certificate
# list ingress'es
kubectl get ingress
# list replication controllers
kubectl get rc
```

### port forwarding

```bash
# forward local port 3000 to port 80 of the service
kubectl port-forward svc/hello-kubernetes-my 3000:80
```

