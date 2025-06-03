# Affinity

- A way to control how Pods are scheduled.
- Allows to express preferences or requirements about where a Pod should (or should not) run, based on labels assigned to nodes or other pods.

## Basic Idea:

**Say you have three nodes:**
- node-A
- node-B
- node-C

And you already have a pod ``frontend`` running on node-A.


Now you create a new pod with Pod Anti-Affinity like:

```yaml
affinity:
  podAntiAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchLabels:
            app: frontend
        topologyKey: "kubernetes.io/hostname"
```

### 🔄 What happens during scheduling?

Kubernetes looks at all nodes and asks:

- Does any node already have a pod with app=frontend?

If yes:

- That node (e.g., node-A) is excluded due to anti-affinity.

If no:

- The pod can be scheduled there.

So here, node-A already has a frontend pod → skip.

Kubernetes considers other nodes (node-B, node-C) for scheduling.



## Two Types

### 🔷 1. Node Affinity (for Nodes)

-  Controls which nodes a pod can be scheduled on, based on node labels.

**Types of Node Affinity:**

- **requiredDuringSchedulingIgnoredDuringExecution (hard requirement)**
    - Pod will only be scheduled on matching nodes.

- **preferredDuringSchedulingIgnoredDuringExecution (soft preference)**
    - Kubernetes will try to schedule on preferred nodes but fall back if not possible.

```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
        - matchExpressions:
            - key: disktype
              operator: In
              values:
                - ssd
```

**This pod will only run on nodes with the label disktype=ssd.**

---


### 🔷 2. Pod Affinity & Anti-Affinity (for other Pods)

> "Other node" simply means a different node than the one currently being evaluated.


- Controls **how pods are scheduled relative to other pods**
- Can make **pods stick together (affinity)** or **keep apart (anti-affinity)**.

**🧲 Pod Affinity:**

You can schedule a Pod on the same node or zone as other specific Pods.

```yaml
affinity:
  podAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchLabels:
            app: frontend
        topologyKey: "kubernetes.io/hostname"
```

This will schedule the pod on the same node as another pod with app=frontend.


**🚫 Pod Anti-Affinity:**

You can prevent Pods from being scheduled with certain other Pods.

```yaml
affinity:
  podAntiAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchLabels:
            app: frontend
        topologyKey: "kubernetes.io/hostname"
```

This will not schedule this pod on a node that already has a pod with app=frontend.


## 🔧 Topology Key

- ``topologyKey`` defines the domain across which the rule applies.
    - kubernetes.io/hostname: **per node**
    - topology.kubernetes.io/zone: **per availability zone** (for multi-zone clusters)
