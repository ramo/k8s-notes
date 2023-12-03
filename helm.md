```
# list all releases in all namespaces
helm ls -A

# search repo for a helm chart and all available versions
helm search repo lvm-crystal-apd/nginx -n crystal-apd-ns --versions

# Upgrade a helm package and update its value, here replicaCount
helm upgrade lvm-crystal-apd lvm-crystal-apd/nginx -n crystal-apd-ns --version=13.2.12 --set replicaCount=2

# List all releases and see the deployment details
helm ls -A --no-headers | grep -v kube-system | awk '{print $1 " -n " $2}' | xargs -l kubectl get deploy

```
