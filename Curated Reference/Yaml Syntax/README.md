# General Yaml Syntax:

A Kubernetes YAML file typically follows a key-value structure. Each YAML file starts with metadata and a specification (spec) for the resource.

```yaml
apiVersion: <version>
kind: <resource-kind>
metadata:
  name: <name-of-resource>
  labels:
    <label-key>: <label-value>
spec:
    <specification-of-resource>
```

- **apiVersion:** Defines the version of the Kubernetes API used by the resource.

- **kind:** Defines the type of Kubernetes resource (e.g., Pod, Deployment, Service).

- **metadata:** Contains information like the name, labels, and annotations of the resource.

- **spec:** Contains the desired state of the resource, like the container specs, replica count, etc.
