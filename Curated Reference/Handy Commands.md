# Kubernetes (kubectl) Commands:

Note: By Default if ``-n`` flag is not used, it shows a resource from default namespace. 

## Basic Commands
```sh
kubectl version                      # Check the kubectl and API server versions
kubectl cluster-info                 # Display cluster information
kubectl get nodes                    # List all nodes in the cluster
kubectl get nodes -o wide            # Get detailed node info
kubectl get pods                      # List all pods in the current namespace
kubectl get all                       # Get al resources in the current namespace
kubectl get services                  # List all services in the current namespace
kubectl get deployments               # List all deployments
kubectl get replicasets               # List all ReplicaSets
```

## Managing Namespaces:

```sh
kubectl get namespaces               # List all namespaces
kubectl create namespace my-namespace # Create a new namespace
kubectl delete namespace my-namespace # Delete a namespace
kubectl config set-context --current --namespace=my-namespace  # Set default namespace
```

## Managing Pods

```sh
kubectl run my-pod --image=nginx         # Create a pod using an image
kubectl get pods                          # List all pods in the current namespace
kubectl get pods -o wide                  # Get detailed pod information
kubectl describe pod my-pod               # Get detailed information about a pod
kubectl delete pod my-pod                  # Delete a pod
kubectl logs my-pod                        # View logs of a pod
kubectl logs -f my-pod                     # Stream pod logs (follow mode)
kubectl exec -it my-pod -- /bin/sh         # Access a running pod's shell
```

## Managing Deployments

```sh
kubectl create deployment my-deployment --image=nginx  # Create a deployment
kubectl get deployments                               # List all deployments
kubectl describe deployment my-deployment            # Get detailed deployment info
kubectl scale deployment my-deployment --replicas=3  # Scale deployment to 3 replicas
kubectl delete deployment my-deployment              # Delete a deployment
```

## Managing Services

```sh
kubectl expose deployment my-deployment --type=NodePort --port=80  # Expose a deployment as a service
kubectl get services                                             # List all services
kubectl describe service my-service                             # Get service details
kubectl delete service my-service                               # Delete a service
```

##  ConfigMaps & Secrets

```sh
kubectl create configmap my-config --from-literal=key=value    # Create a ConfigMap
kubectl get configmaps                                         # List all ConfigMaps
kubectl describe configmap my-config                          # Get ConfigMap details
kubectl delete configmap my-config                            # Delete a ConfigMap

kubectl create secret generic my-secret --from-literal=password=12345  # Create a Secret
kubectl get secrets                                                    # List secrets
kubectl describe secret my-secret                                      # Get secret details
kubectl delete secret my-secret                                       # Delete a Secret
```

## Managing StatefulSets & DaemonSets

```sh
kubectl get statefulsets                   # List all StatefulSets
kubectl delete statefulset my-statefulset  # Delete a StatefulSet

kubectl get daemonsets                      # List all DaemonSets
kubectl delete daemonset my-daemonset      # Delete a DaemonSet
```

## Managing Jobs & CronJobs

```sh
kubectl get jobs                           # List all jobs
kubectl delete job my-job                   # Delete a job

kubectl get cronjobs                        # List all cronjobs
kubectl delete cronjob my-cronjob           # Delete a cronjob
```

## Managing Persistent Volumes (PVs) & Persistent Volume Claims (PVCs)

```sh
kubectl get pv                            # List Persistent Volumes
kubectl get pvc                           # List Persistent Volume Claims
kubectl describe pvc my-pvc               # Describe a Persistent Volume Claim
kubectl delete pvc my-pvc                 # Delete a PVC
```

## Scaling & Rolling Updates

```sh
kubectl scale deployment my-deployment --replicas=5  # Scale deployment to 5 replicas
kubectl rollout status deployment my-deployment      # Check rollout status
kubectl rollout history deployment my-deployment     # View rollout history
kubectl rollout undo deployment my-deployment       # Roll back to the previous version
```

## Debugging & Troubleshooting

```sh
kubectl describe pod my-pod                        # Get detailed pod information
kubectl logs my-pod                                # View pod logs
kubectl logs -f my-pod                             # Stream logs in real-time
kubectl exec -it my-pod -- /bin/sh                 # Access a running pod's shell
```