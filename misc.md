### Taints and Tolerations

```yaml
# If taint only has key and no value.
tolerations:
  - key: node-role.kubernetes.io/master
    effect: NoSchedule
```

### Scheduling

> **spec.nodeName** is used in manual scheduling. This will ignore taints, nodeAffinity, etc.

### Scaling

> deploy, sts, rc, rs

### Labels

> --show-labels # to list labels of k8s object.

### Pods

> Any command use **"sh -c"** before that.

```yaml
#spec.containers[*]
resources:
  limits:
    cpu: 1m
    memory: 10Mi
  requests:
    cpu: 1m
    memory: 10Mi
```

```bash
# sort by creation time
kubectl get pod -A --sort-by=.metadata.creationTimestamp
```

```yaml
# Mount a volume to container
containers:
  - name: container
    volumeMounts:
      - name: data
        mountPath: /tmp/data
```

```yaml
# Expose pod information as environment variable
containers:
  - env:
      - name: MY_NODE_NAME
        valueFrom:
          fieldRef:
            fieldPath: spec.nodeName
```

### PersistentVolume & PersistentVolumeClaim

> pv - cluster level
> pvc - namespace level

```yaml
# Define POD volume from pvc
spec:
  volumes:
    - name: data
      persistentVolumeClaim:
        claimName: my-pvc
```

### Cluster metrics

> kubectl top --help

### Check user/serviceaccount permissions

> k auth can-i --list --as user1
> k auth can-i --list --as system:serviceaccount:ns1:sa1

### DaemonSet

> k create deploy can be used

### topologyKey

> uniquely identify a k8s resource. Eg: node, zone, etc.

### static pods

> The spec of a static Pod cannot refer to a ConfigMap or any other API objects.

### ConfigMaps

```yaml
# mount as volumes in pod
volumes:
  - name: config
    configMap:
      name: game-demo
      items:
        - key: 'game.properties'
          path: 'game.properties'
        - key: 'user-interface.properties'
          path: 'user-interface.properties'
```

```yaml
# configure as environment variable in container
env:
  - name: SPECIAL_LEVEL_KEY
    valueFrom:
      configMapKeyRef:
        name: special-config
        key: special.how
```

```yaml
# Define all of the ConfigMap's data as container environment variables
envFrom:
  - configMapRef:
      name: special-config
```

```yaml
# You can use ConfigMap-defined environment variables in the command and args of a container using the $(VAR_NAME) Kubernetes substitution syntax.
command: ['/bin/echo', '$(SPECIAL_LEVEL_KEY)']
env:
  - name: SPECIAL_LEVEL_KEY
    valueFrom:
      configMapKeyRef:
        name: special-config
        key: SPECIAL_LEVEL
```

### Secrets

```yaml
# volume in pod
volumes:
  - name: foo
    secret:
      secretName: mysecret
```

```yaml
# volume in pod with projections
volumes:
  - name: foo
    secret:
      secretName: mysecret
      items:
        - key: username
          path: my-group/my-username
```

> Default file permission is 0644

```yaml
# override file permission
volumes:
  - name: foo
    secret:
      secretName: mysecret
      defaultMode: 0400
```

```yaml
# container environment variable
env:
  - name: SECRET_USERNAME
    valueFrom:
      secretKeyRef:
        name: backend-user
        key: backend-username
```

```yaml
# container environment variable all
envFrom:
  - secretRef:
      name: test-secret
```
