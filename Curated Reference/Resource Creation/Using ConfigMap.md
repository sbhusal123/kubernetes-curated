# Using COnfig Map In Kubernetes:

Suppose you have following config value for setting env variables in the application.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  APP_ENV: "production"
  APP_DEBUG: "false"
```

Now we can pass each of those values into the env variables in the Deployment or ReplicaSet.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: my-app-image
          env:
            - name: APP_ENV
              valueFrom:
                configMapKeyRef:
                  name: my-config
                  key: APP_ENV
            - name: APP_DEBUG
              valueFrom:
                configMapKeyRef:
                  name: my-config
                  key: APP_DEBUG

```

Notice the section:

```yaml
          env:
            - name: APP_ENV
              valueFrom:
                configMapKeyRef:
                  name: my-config
                  key: APP_ENV
            - name: APP_DEBUG
              valueFrom:
                configMapKeyRef:
                  name: my-config
                  key: APP_DEBUG
```

This way we can pass the env variables values into the Deployment with ConfigMap.

But wait, what if we need to pass all the values as it it (same key and value), value into the env from ConfigMap.

**Note:Here, we are passing env values into the env list as per yaml syntax.**


```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: my-app-image
          envFrom:
            - configMapRef:
                name: my-config
```

Notice the section:

```yaml
          envFrom:
            - configMapRef:
                name: my-config
```

**Note:** Here we are passing whole ConfigMap object data into ``envFrom``
