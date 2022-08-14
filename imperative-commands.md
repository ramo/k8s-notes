### Create a Pod

```
kubectl run nginx --image=nginx
```

### Create a Pod with label

```
kubectl run nginx --image=nginx --labels="app=nginx,tier=front-end"
```

### Create a Pod with environment variables

```
kubectl run nginx --image=nginx --env="APP_NAME=nginx" --env="ABC=xyz"
```

### Create a Deployment

```
kubectl create deployment nginx-deployment --image=nginx --replicas=1
```

### Create a Service

```
kubectl expose deployment nginx-deployment --port=80 --target-port=80 --name=nginx-service
```

### View KubeConfig

```
kubectl config view
```

### Change context

```
kubectl config use-context kubernetes-admin@kubernetes
```

### Change namespace of current context

```
kubectl config set-context --current --namespace=test
```

### Show the current context

```
kubectl config current-context
```

### List k8s api resources

```
kubectl api-resources
```

### Run a test curl Pod

kubectl run curl --image=alpine/curl --rm -it --command -- /bin/sh
