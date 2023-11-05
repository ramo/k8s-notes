### Namespace

```
apiVersion: v1
kind: Namespace
metadata:
  name: dvl1987

```

### Pod

```
# Pod example to use pvc volumes
apiVersion: v1
kind: Pod
metadata:
  name: logger
spec:
  volumes:
    - name: log
      persistentVolumeClaim:
        claimName: log-claim
  containers:
    - name: nginx
      image: nginx:alpine
      ports:
        - containerPort: 80
          name: "http-server"
      volumeMounts:
        - mountPath: "/var/www/nginx"
          name: log
```

```
# Pod example to use emptyDir volume
# and use configmap value
# execute command
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: time-check
  name: time-check
  namespace: dvl1987
spec:
  volumes:
  - name: local-vol
    emptyDir: {}
  containers:
  - image: busybox
    name: time-check
    command:
    - "/bin/sh"
    - "-c"
    - "while true; do date; sleep $TIME_FREQ; done > /opt/time/time-check.log"
    volumeMounts:
    - name: local-vol
      mountPath: /opt/time
    env:
    - name: TIME_FREQ
      valueFrom:
        configMapKeyRef:
          name: time-config
          key: TIME_FREQ
```

### Service

### Deployment

```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: redis
  name: redis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: redis
    spec:
      volumes:
      - name: data
        emptyDir: {}
      - name: redis-config
        configMap:
          name: redis-config
      containers:
      - image: redis:alpine
        name: redis
        ports:
        - containerPort: 6379
        volumeMounts:
        - name: data
          mountPath: /redis-master-data
        - name: redis-config
          mountPath: /redis-master
        resources:
          requests:
            cpu: "0.2"
```

### ReplicaSet

### ReplicationController

### StatefulSet

### DaemonSet

### Secret

### ConfigMap

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: time-config
  namespace: dvl1987
data:
  TIME_FREQ: "10"

```

### ServiceAccount

### Role

### RoleBinding

### ClusterRole

### ClusterRoleBinding

### PersistentVolume

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: log-volume
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  storageClassName: manual
  hostPath:
    path: /opt/volume/nginx

```

### PersistentVolumeClaim

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: log-claim
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 200Mi

```

### NetworkPolicy

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: access-secure-service
spec:
  podSelector:
    matchLabels:
      run: secure-pod
  ingress:
  - from:
    - podSelector:
        matchLabels:
          name: "webapp-color"
```
