# ✅ Step-by-Step Fresh Installation

## 🧰 Prerequisites
Make sure you have:

- `kubectl` configured to point to your cluster  
- `helm` installed (`helm version` to check)

## 📦 1. Add the Prometheus Helm repo
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

## 📁 2. Create a monitoring namespace
```bash
kubectl create namespace monitoring
```

## 🚀 3. Install kube-prometheus-stack
```bash
helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring
```

This installs:

- Prometheus Operator  
- Prometheus  
- Grafana  
- Alertmanager  
- Node Exporter  
- Kube State Metrics

## 🔍 4. Check installed pods
```bash
kubectl get pods -n monitoring
```

Wait until all pods show `Running` or `Completed`.

## 🌐 5. Access Grafana
Grafana is installed as a service. To access it:

### Port forward
```bash
kubectl port-forward svc/prometheus-grafana 3000:80 -n monitoring
```

Then open: http://localhost:3000

**Default credentials:**

- **Username:** `admin`
- **Password:** Run this to get it:
```bash
kubectl get secret prometheus-grafana -n monitoring -o jsonpath="{.data.admin-password}" | base64 --decode
```


## Using On Deployment:

- For monitoring pods from a deployment.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: <deployment_name>
  namespace: <namespace>
  labels:
    app: <selenium-node-chrome>
  annotations:
    prometheus.io/scrape: "true"
    prometheus.io/port: "5555"
```
