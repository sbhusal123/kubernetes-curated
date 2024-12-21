# Example 1: Deploying MongoDB Express with Kubernetes

```
2 Deployment / Pod
2 Services
1 ConfigMap
1 Secrets
```



- `MongoDB Pod` => To Run a MongoDB database.

- `Internal Service` to allow only services in internal cluster to talk to each other.

- `MongoExpress` deployment, 

- Credentials to be stored inside the Secrets.

- Configurations to be stored inside the ConfigMap.


**Note:**
- Deployment Creates ReplicaSets.
- ReplicaSets Creates Pods.
- Inside pods runs a container.
