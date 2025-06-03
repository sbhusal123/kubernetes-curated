# 🐳 Minikube Logging Stack: Install Promtail, Loki, and Grafana with Helm

This guide walks you through setting up **Promtail**, **Loki**, and **Grafana** on a **Minikube** cluster using **Helm**.

---

## ✅ Prerequisites

Make sure the following are set up:

- **Minikube** running:
  ```bash
  minikube start
  ```

- **Helm** installed:
  ```bash
  helm version
  ```

---

## 🔧 Step 1: Add Helm Repositories

Add the Grafana Helm chart repository and update:

```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

---

## 📦 Step 2: Install Loki (Log Aggregator)

Install Loki (without Promtail for now):

```bash
helm upgrade --install loki grafana/loki-stack \
  --namespace monitoring --create-namespace \
  --set promtail.enabled=false
```

---

## 📦 Step 3: Install Promtail (Log Collector)

Install Promtail to collect logs from the cluster and send them to Loki:

```bash
helm upgrade --install promtail grafana/promtail \
  --namespace monitoring \
  --set "loki.serviceName=loki"
```

---

## 📊 Step 4: Install Grafana (Visualization UI)

Install Grafana to view logs stored in Loki:

```bash
helm upgrade --install grafana grafana/grafana \
  --namespace monitoring \
  --set adminPassword='admin' \
  --set service.type=NodePort
```

Expose Grafana on a local port:

```bash
minikube service grafana -n monitoring
```

This will open Grafana in your browser.

---

## 🔧 Step 5: Access Grafana

Default login:
- **Username:** `admin`
- **Password:** `admin` (or whatever you set with `--set adminPassword`)

---

## ➕ Step 6: Add Loki as a Data Source in Grafana

1. In Grafana UI:
   - Go to **Settings → Data Sources → Add data source**
   - Choose **Loki**
   - URL: `http://loki.monitoring.svc.cluster.local:3100`
   - Save & Test

Now, you can use **Explore** in Grafana to view logs.

---

## ✅ Verify

Check that:
- Promtail is running and sending logs.
- Loki is storing them.
- Grafana can display them.

---

## 📂 Optional: Helm Values Customization

You can customize Promtail's config:

```bash
helm show values grafana/promtail > promtail-values.yaml
```

Edit `promtail-values.yaml` to set `scrape_configs`, then install with:

```bash
helm upgrade --install promtail grafana/promtail \
  --namespace monitoring -f promtail-values.yaml
```

---
