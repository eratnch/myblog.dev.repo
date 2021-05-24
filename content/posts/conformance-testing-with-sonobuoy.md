---
title: "Conformance Testing With Sonobuoy"
date: 2021-05-23T23:49:19-04:00
draft: true
---

# Conformance Testing Kubernetes Clusters

## Validate k8s cluster with sonobuoy

### Create a Kind cluster with multiple nodes

```
~/conformance_test$ cat kind_config.yaml

kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
# One control plane node and three "workers".
#
# While these will not add more real compute capacity and
# have limited isolation, this can be useful for testing
# rolling updates etc.
#
# The API-server and other control plane components will be
# on the control-plane node.
#
# You probably don't need this unless you are testing Kubernetes itself.
nodes:
- role: control-plane
- role: worker
- role: worker
- role: worker
```