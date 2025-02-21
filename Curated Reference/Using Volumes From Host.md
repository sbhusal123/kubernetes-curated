# Using Persistent Volume From Host Machine In Kubernetes:

## 1. Enable Minikube Mount

To share a directory between your host machine and Minikube, you must mount the directory using the minikube mount command.

```sh
minikube mount /home/surya/shared-data:/mnt/shared-data
```

**Explanation:**
- **/home/surya/shared-data** → The directory on your host machine.
- **/mnt/shared-data** → The directory inside Minikube.

🔹 Keep this terminal open while your Minikube cluster is running, or the mount will be lost.

## 2. Use hostPath in Your Deployment

Now, define a Persistent Volume (PV) and Persistent Volume Claim (PVC) in Kubernetes.


```yml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: shared-pv
spec:
  capacity:
    storage: 2Gi
  accessModes:
    - ReadWriteMany  # Allows multiple pods to read/write
  persistentVolumeReclaimPolicy: Retain
  storageClassName: manual
  hostPath:
    path: /mnt/shared-data  # ✅ This should match the Minikube mount path
```

```yml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: shared-pvc
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 2Gi
  storageClassName: manual
```

## 3. Use PVC In Your Pod

```yml
apiVersion: v1
kind: Pod
metadata:
  name: shared-storage-pod
spec:
  containers:
    - name: busybox
      image: busybox
      command: [ "sleep", "3600" ]  # Keep the container running
      volumeMounts:
        - mountPath: /data
          name: shared-storage
  volumes:
    - name: shared-storage
      persistentVolumeClaim:
        claimName: shared-pvc
```

To View the storage inside the minikube:

- SSH into VM: ``minikube ssh``

- List directory: ``ls``

- To fix permission issue while cding into directory: ``sudo chmod 777 <directory>``