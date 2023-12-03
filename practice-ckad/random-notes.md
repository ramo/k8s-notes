```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ckad-sidecar-pod
  name: ckad-sidecar-pod
  namespace: ckad-multi-containers
spec:
  volumes:
    - name: my-vol
      emptyDir: {}
  containers:
    - image: nginx:1.16
      name: main-container
      volumeMounts:
        - name: my-vol
          mountPath: /usr/share/nginx/html
    - image: busybox:1.28
      name: sidecar-container
      command:
        - /bin/sh
        - -c
        - while true; do echo "Hi I am from Sidecar container $(date)" >> /var/log/index.html; sleep 5; done
      volumeMounts:
        - name: my-vol
          mountPath: /var/log
```

```yaml
# k create cronjob learning-every-minute --schedule="*/1 * * * *" --image=busybox:1.28 $do -- echo "I am practicing for CKAD certification"
apiVersion: batch/v1
kind: CronJob
metadata:
  creationTimestamp: null
  name: learning-every-minute
spec:
  jobTemplate:
    metadata:
      creationTimestamp: null
      name: learning-every-minute
    spec:
      template:
        metadata:
          creationTimestamp: null
        spec:
          containers:
            - command:
                - echo
                - I am practicing for CKAD certification
              image: busybox:1.28
              name: learning-every-minute
              resources: {}
          restartPolicy: OnFailure
  schedule: '*/1 * * * *'
status: {}
```

```yaml
# k create cronjob simple-python-job --image=python --schedule="*/30 * * * *" -- /bin/sh -c "ps -eaf"

apiVersion: batch/v1
kind: CronJob
metadata:
  name: simple-python-job
  namespace: ckad-job
spec:
  schedule: '*/30 * * * *'
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: simple-python-job
              image: python
              imagePullPolicy: IfNotPresent
              command:
                - /bin/sh
                - -c
                - ps –eaf
          restartPolicy: OnFailure
```

```yaml
# k run tres-containers-pod --image=busybox:1.28 --env ORDER=FIRST --port=8080 $do
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: tres-containers-pod
  name: tres-containers-pod
  namespace: ckad-multi-containers
spec:
  containers:
    - env:
        - name: ORDER
          value: FIRST
      image: busybox:1.28
      name: primero
    - image: nginx:1.17
      name: segundo
      ports:
        - containerPort: 8080
    - env:
        - name: ORDER
          value: THIRD
      image: busybox:1.31.1
      name: tercero
```
