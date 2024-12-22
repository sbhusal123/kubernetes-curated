# Example Syntax For Deployment

A Deployment ensures that the specified number of pods are running and is ideal for managing stateless applications.


```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```


**Explanation**

- **apiVersion:** apps/v1 is the API version for resources like Deployments.

- **kind:** The type of resource is Deployment.

- **metadata:**
    - **name:** The name of the deployment (nginx-deployment).

- **spec:**
    - **replicas:** Specifies the number of pods to run (3 replicas).

    - **selector:** Defines how the Deployment finds which pods to manage. In this case, pods with the label app: nginx will be managed.
 
- **template:** The pod template used to create the pods for the deployment.

    - **metadata:** Specifies labels for the pods.
    - **spec:** Contains the pod specifications, including the container to run (nginx in this case).
