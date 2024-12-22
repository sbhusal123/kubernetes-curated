# Django Deployment With:

**Note: this configuration is untested.**


**1. Namespace:**

- [All resources created in ``my-djang-app`` namespace](./namespace.yaml)

```sh
kubectl apply -f namespace.yaml
```

**ConfigMap and Secrets**

- [ConfigMap to store the configurations for Database and Redis](./ConfigMap.yaml)

- [Secrets to Store DB password and django secrets](./secrets.yaml)

```sh
kubectl apply -f ConfigMap.yaml

kubectl apply -f secrets.yaml
```

**Volumes for Media And StaticFiles**

- [Persistent Volume for Static And Media](./pv-static-media.yaml)

- [Persistent Volumand And Persistent Volume Claim for Redis](./pvc-static-media.yaml)

```sh
kubectl apply -f pv-static-media.yaml

kubectl apply -f pvc-static-media.yaml
```


**Volumes for Databases:**

- [Persistent Volumand And Persistent Volume Claim for Postgres](./pvc-postgres.yaml)

- [Persistent Volumand And Persistent Volume Claim for Redis](./pvc-redis.yaml)

```sh
kubectl apply -f pvc-postgres.yaml

kubectl apply -f pvc-redis.yaml

```

**Stateful Set For Redis And Postgres:**

- [Postgres Stateful Set](./postgres-statefulset.yaml)

- [Redis Stateful Set](./postgres-statefulset.yaml)

```sh
kubectl apply -f postgres-statefulset.yaml

kubectl apply -f redis-statefulset.yaml

```

**Deployment:**

- [3 replicas of Backend Pods](./django-deployment.yaml)

- [3 replicas of celery pods](./celery-deployment.yaml)

```sh
kubectl apply -f django-deployment.yaml
kubectl apply -f celery-cipservice.yaml
```


**Cluster IP service for Backend And Databases:**


- [Cluster IP servce for django application](./django-cipservice.yaml)

- [Cluster IP service for databases](./redis-postres-cip-service.yaml)

```sh
kubectl apply -f django-cipservice.yaml
kubectl apply -f redis-postres-cip-service.yaml
```

**Horizontal Pod AutoScaller:**

- [HPA for Celery And Django Pods](./hpa.yaml)

Adds up more pods when the CPU utilization of Celery or Django application reaches more than 70%.

Min Replicas: 3
Max Replicas: 10
