# Liveliness And Readiness Probe:

- Mechanisms to monitor and manage the health of containers within pods.

- Helps Kubernetes determine whether a **container is running correctly** and whether it is **ready to accept traffic.**

## 1. Liveness Probe

**Purpose:** Detects if a container is still alive (running properly).

- If a **liveness probe fails**, Kubernetes **kills the container**, and the pod is subject to restart policy.

- Useful for detecting stuck containers (e.g., deadlocks, infinite loops).

- **Example Use Case:** Your app runs but is stuck and cannot recover itself—Kubernetes will restart it if the liveness probe fails.

## 2. Readiness Probe

- If a **readiness probe fails**, Kubernetes **removes the pod from service** (i.e., from endpoints of a Service) but does **not restart the container.**

- Useful when your app needs time for initialization, warming up caches, etc.

- **Example use case:** Your app starts but needs to load models/configurations before it can handle requests. It becomes “ready” after this setup.

## Example Yaml:

```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 10

readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 5
```

### Parameters Description:

| Parameter             | Description                                                                 |
|-----------------------|-----------------------------------------------------------------------------|
| `initialDelaySeconds` | Time (in seconds) to wait after container start before starting the probe. |
| `periodSeconds`       | How often (in seconds) to perform the probe. Default is `10`.               |
| `timeoutSeconds`      | Number of seconds to wait for a probe response before marking as failed.    |
| `successThreshold`    | Minimum consecutive successes required to consider the probe successful.    |
| `failureThreshold`    | Minimum consecutive failures to consider the probe as failed.               |

### Probe Type:

**1. ``httpGet`` Probe Example:**

```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 10
  timeoutSeconds: 2
  failureThreshold: 3
```

**2. ``tcpSocket`` Probe Example:**

```yaml
readinessProbe:
  tcpSocket:
    port: 3306
  initialDelaySeconds: 10
  periodSeconds: 5
  timeoutSeconds: 1
  failureThreshold: 3
```

**3. ``exec`` Probe Example:**

```yaml
livenessProbe:
  exec:
    command:
      - cat # <executable>
      - /tmp/healthy # <agr1> can have multiple arg
  initialDelaySeconds: 3
  periodSeconds: 5
  timeoutSeconds: 1
  failureThreshold: 3
```
