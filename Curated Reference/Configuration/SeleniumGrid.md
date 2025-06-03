# Configuration For Selenium Grid

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: selenium-node-chrome
  namespace: selenium-grid
  labels:
    app: selenium-node-chrome
  annotations:
    prometheus.io/scrape: "true"
    prometheus.io/port: "5555"
spec:
  replicas: 2
  selector:
    matchLabels:
      app: selenium-node-chrome
  template:
    metadata:
      labels:
        app: selenium-node-chrome
    spec:
      terminationGracePeriodSeconds: 30
      securityContext:
        runAsNonRoot: true
        runAsUser: 1000
        fsGroup: 1000
      affinity:
        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
            - weight: 100
              podAffinityTerm:
                labelSelector:
                  matchLabels:
                    app: selenium-node-chrome
                topologyKey: "kubernetes.io/hostname"
      containers:
      - name: selenium-node-chrome
        image: selenium/node-chrome:4.21.0-20240528
        imagePullPolicy: Always
        env:
        - name: SE_EVENT_BUS_HOST
          value: selenium-hub.selenium-grid.svc.cluster.local
        - name: SE_EVENT_BUS_PUBLISH_PORT
          value: "4442"
        - name: SE_EVENT_BUS_SUBSCRIBE_PORT
          value: "4443"
        - name: SE_NODE_MAX_SESSIONS
          value: "3"
        ports:
        - containerPort: 5555
        livenessProbe:
          httpGet:
            path: /status
            port: 5555
          initialDelaySeconds: 30
          periodSeconds: 10
          failureThreshold: 3
        readinessProbe:
          httpGet:
            path: /status
            port: 5555
          initialDelaySeconds: 10
          periodSeconds: 5
          failureThreshold: 3
        resources:
          requests:
            memory: "400Mi"
            cpu: "750m"
          limits:
            memory: "600Mi"
            cpu: "1000m"
```

## 1. Resources (requests and limits)


```yaml
resources:
  requests:
    memory: "400Mi"
    cpu: "750m"
  limits:
    memory: "600Mi"
    cpu: "1000m"
```

### Explanation:

- **Requests** = Minimum amount of CPU/memory guaranteed.

- **Limits** = Maximum amount allowed.

- Ensures **predictable scheduling** and avoids noisy neighbors.

- Chrome browsers are CPU/memory intensive, so tune accordingly to avoid OOM errors or throttling.

## 2. Liveness & Readiness Probes

```yaml
  livenessProbe:
    httpGet:
      path: /status
      port: 5555
    initialDelaySeconds: 30
    periodSeconds: 10
    failureThreshold: 3
  readinessProbe:
    httpGet:
      path: /status
      port: 5555
    initialDelaySeconds: 10
    periodSeconds: 5
    failureThreshold: 3
```

### 💡 Explanation:

- **Liveness Probe:** Checks if the container is healthy. If it fails, Kubernetes restarts the container.



- **Readiness Probe:** Checks if the container is ready to serve traffic. If it fails, it's temporarily removed from service.


- Important in Selenium nodes, since they may fail or become non-responsive during tests.

**Terminologie:**

- **Starts after:** ``initialDelaySeconds: 10`` → starts 10 seconds after the container begins running.

- **Runs every:** ``periodSeconds: 5`` → runs every 5 seconds after the initial delay.

- **Fails if:** failureThreshold: 3 → if 3 consecutive probes fail

> **Readiness Probe:** Kubernetes waits 10 seconds, then probes every 5 seconds. If /status fails 3 times in a row, the pod is marked as Not Ready.

>  **Liveness Probe:** Kubernetes waits 30 seconds, then probes every 10 seconds. If /status fails 3 times in a row, the container is restarted.

## 3. Affinity & Anti-Affinity

```yaml
affinity:
  podAntiAffinity:
    preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 100
        podAffinityTerm:
          labelSelector:
            matchLabels:
              app: selenium-node-chrome
          topologyKey: "kubernetes.io/hostname"
```

**preferredDuringSchedulingIgnoredDuringExecution:**
- The scheduler will try to follow it but won’t block scheduling if it can’t.
- If it can, it will try to spread out the pods.

**weight=100**
- The importance of this preference (from 1 to 100).
- The scheduler uses the sum of weights to choose the best node when placing a new pod.
- 100 means this preference is very strong, but still not mandatory.

**topologyKey: "kubernetes.io/hostname"**
- Defines the grouping of nodes for affinity/anti-affinity.
- Here, it's "kubernetes.io/hostname" which means the rule is applied per node.
- So, avoid putting two selenium-node-chrome pods on the same node.



### Explanation:

- Prevents scheduling multiple Chrome nodes on the **same physical worker node.**

- Helps with **load spreading** and avoids resource contention.

- topologyKey: "kubernetes.io/hostname" means “spread across different nodes”.

## 4. Security Context

```yaml
securityContext:
  runAsNonRoot: true
  runAsUser: 1000
  fsGroup: 1000
```

### Explanation:
- Runs containers as a non-root user (UID 1000) and file system user group 1000, a **Kubernetes security best practice.** 

- Prevents privilege escalation if the container is compromised.

## 5. Termination Grace Period

```yaml
terminationGracePeriodSeconds: 30
```

### Explanation:

- When the pod is being terminated (e.g., during upgrade or scale-down), it has 30 seconds to finish current tasks (like test execution).

- Prevents test failures due to abrupt kill.

##  6. Annotations for Monitoring:


```yaml
annotations:
  prometheus.io/scrape: "true"
  prometheus.io/port: "5555"
```

### Explanation:

- Used by **Prometheus** to auto-discover and scrape metrics from this pod.

- You can visualize metrics in **Grafana** (e.g., session count, response times).

## 7. Image Pull Policy

```yaml
imagePullPolicy: Always
```

### Explanation:

- Forces Kubernetes to pull the image every time the pod is created.

- Useful in CI/CD or staging where images may change frequently under the same tag.

