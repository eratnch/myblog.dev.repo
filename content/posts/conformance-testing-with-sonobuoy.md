---
title: "Conformance Testing With Sonobuoy"
date: 2021-05-23T23:49:19-04:00
draft: true
---

# Conformance Testing Kubernetes Clusters

## Validate k8s cluster with sonobuoy

### Create a Kind cluster with multiple nodes

```bash
~/conformance_test$ cat kind_config.yaml

kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
- role: worker
```


```bash
kind create cluster --name conformance --config conformance_test/kind_config.yaml
Creating cluster "conformance" ...
✓ Ensuring node image (kindest/node:v1.19.1) 🖼
✓ Preparing nodes 📦 📦 📦 📦
✓ Writing configuration 📜
✓ Starting control-plane 🕹️
✓ Installing CNI 🔌
✓ Installing StorageClass 💾
✓ Joining worker nodes 🚜
Set kubectl context to "kind-conformance"
You can now use your cluster with:

kubectl cluster-info --context kind-conformance

Not sure what to do next? 😅  Check out https://kind.sigs.k8s.io/docs/user/quick-start/
```

