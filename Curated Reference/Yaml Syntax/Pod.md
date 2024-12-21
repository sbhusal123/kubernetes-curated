# Example of Yaml Syntax For Pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

**Explanation:**
- **apiVersion:** v1 is the API version for core resources like Pods.
- **kind:** The type of resource is Pod.
- **metadata:**
    - **name:** The name of the pod (nginx-pod).
    - **labels:** A label is added to the pod for identification (app: nginx).

- **spec:**
    - **containers:** Specifies the container running inside the pod.
        - **name:** Name of the container (nginx).
        - **image:** Docker image for the container (nginx:latest).
        - **ports:** The port exposed by the container (80).
