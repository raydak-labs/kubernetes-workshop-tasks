# Code together - Kubernetes

```bash
# After all docker stuff the kubectl commands will be kinda the same as in docker but with more power
# First make sure to have a running cluster or start one in docker-desktop
# After installing the cluster everything should be up and running and hopefully the command works:

kubectl get pods
# No pods in default namespace? This is what we want to see.
# lets check some cluster information like version, nodes and so

kubectl version --output=yaml

kubectl get nodes
# lets add a flag
kubectl get nodes -owide

kubectl cluster-info

# wow so much information but nice to see things going
# many things in kubernetes can be done from the CLI directly without needed yaml files for testing.
# But YAML files can be versioned and easily shared
# urls and local files can be used alike
kubectl apply -f https://k8s.io/examples/controllers/nginx-deployment.yaml
kubectl get deployment
# not ready? lets watch for changes
kubectl get deployment -w
# check the pods
kubectl get pods
# nice. lets kill them
kubectl delete deployment nginx-deployment

kubectl get pods
# Gone. Nice. The equivalent would be something like this from CLI only
kubectl create deployment nginx-deployment --image=nginx:alpine --replicas 3
kubectl get pods

# lets update the deployment. We just need one pod. Scaling has its own command luckily
kubectl scale deployment --replicas 1 nginx-deployment
kubectl get pods

# lets change the image of the pods with updating the deployment
kubectl set image deployment/nginx-deployment nginx=nginx:latest

kubectl get pods
# Should be new. Lets check the logs
kubectl logs ...

# Lets kill the pod
kubectl delete pod ...
kubectl get pods
# Wait its back? No its a new own. Kubernetes ensures to keep the definition of the deployment up and running.

# How can we access the nginx deployment? With a service! With a NodePort service we can directly map the host port to the container port
kubectl create service nodeport nginx-deployment --tcp=80:80 --node-port 30180
# lets check http://localhost:30180 ... nice like docker!

# cleanup
kubectl delete svc nginx-deployment
kubectl delete deployment nginx-deployment
```
