## 1. Deployment Instructions

### Prerequisites
Ensure the `mateapp` namespace exists before applying the manifests:
```bash
kubectl create namespace mateapp --dry-run=client -o yaml | kubectl apply -f -
```

Ensure the `todoapp-service` ClusterIP service is running in the `todoapp` namespace:
```bash
kubectl apply -f .infrastructure/clusterIp.yml
```

### Apply Manifests
Deploy the DaemonSet and CronJob resources:
```bash
# Deploy DaemonSet
kubectl apply -f .infrastructure/daemonset.yml

# Deploy CronJob
kubectl apply -f .infrastructure/cronjob.yml
```

---

## 2. Validation Instructions

### Verify Resource Creation
Check that the namespace, DaemonSet, and CronJob are created:
```bash
# List DaemonSets in mateapp namespace
kubectl get daemonsets -n mateapp

# List CronJobs in mateapp namespace
kubectl get cronjobs -n mateapp

# List Pods running in mateapp namespace
kubectl get pods -n mateapp -o wide
```

### Check Logs

#### DaemonSet Logs
To check logs for the DaemonSet container executing a `curl` request every 5 seconds to `todoapp-service.todoapp`:
```bash
kubectl logs -n mateapp -l app=daemonset --tail=20
```

#### CronJob Logs
To check logs for completed CronJob pod executions hitting `/api/health` every 4 minutes:
```bash
# Get jobs created by the cronjob
kubectl get jobs -n mateapp

# View logs of the latest job pod created by CronJob
kubectl logs -n mateapp -l app=cronjob --tail=20
```