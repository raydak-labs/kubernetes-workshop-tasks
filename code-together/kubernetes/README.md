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
# check the owner reference of the pod in the metadata
kubectl get pods -o yaml
# check the owner reference of the replicaset in the metadata
kubectl get replicasets -o yaml
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

# Lets change the deployment to use a persistent volume for the website
# Copy the deployment yaml from below to a file and apply it over the existing one
kubectl apply -f deployment.yml

# The deployment creates a pod which is stuck in pending, inspect why
kubectl describe pod ...

# Create the PVC we have forgotten
# Copy the PVC yaml to a file and apply it
kubectl apply -f pvc.yml
# Check the PVC status, it should show Status: Bound
kubectl get pvc

# Now check the pod again, it should start as the PVC does now exist
kubectl get pod

# Now check the exposed website again, it should show an error: http://localhost:30180
# Any idea why?

# Now create a new index.html we want expose with any content you like
vim index.html

# Now copy your new index.html into the container to the path of the persistent volume
kubectl cp index.html nginx-deployment-*-*:/usr/share/nginx/html

# Now check the exposed website again, it should show your website: http://localhost:30180

# Let check if the files are really persistent, delete the deployment and create it again
kubectl delete deployment nginx-deployment
# Check the deployments and pods to see, if they are gone
kubectl get deploy
kubectl get po

# Create the deployment again by applying the yaml from before
kubectl apply -f deployment.yml

# Now check the exposed website again, it should again show your website: http://localhost:30180


# cleanup

kubectl delete svc nginx-deployment
kubectl delete deployment nginx-deployment
kubectl delete pvc website-claim

```

## Deployment with PVC

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx-deployment
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx-deployment
  template:
    metadata:
      labels:
        app: nginx-deployment
    spec:
      containers:
        - image: nginx
          name: nginx
          volumeMounts:
            - mountPath: "/usr/share/nginx/html"
              name: mypd
      volumes:
        - name: mypd
          persistentVolumeClaim:
            claimName: website-claim
```

## PVC

```yml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: website-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 50Mi
```
