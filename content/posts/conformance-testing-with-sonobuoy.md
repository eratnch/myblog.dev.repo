---
title: "Conformance Testing With Sonobuoy"
date: 2021-05-23T23:49:19-04:00
draft: true
---

# Conformance Testing Kubernetes Clusters

Kubernetes has seen exponential growth and wide-scale adoption in the cloud computing and container orchestration land since last several years. It has become the defacto tool for deploying and maintaining large scale workloads in production environments across platforms. There are currently over 90 certified kubernetes platforms and distributions available in the market.

In order to standardize the certification process of kubernetes distributions, the Kubernetes Community and Cloud Native Computing Foundation(CNCF) run a Kubernetes Conformance Certification program. It requires each kubernetes distribution to successfully run targeted set of conformance test cases in order to achieve CNCF certification. 

## Sonobuoy

The standard tool for running these tests is [Sonobuoy](!https://github.com/vmware-tanzu/sonobuoy). Sonobuoy is a diagnostic tool that can validate the state of a Kubernetes cluster by running set of plugins. 

### E2E Plugin 

Kubernetes end-to-end testing plugin (the E2E plugin) is used to run tests maintained by upstream Kubernetes community.

Following `modes` or run configurations are used commonly when running conformance tests

- quick
  This runs a single test from `e2e` test suite to validate cluster and sonobuoy configuration.
- non-disruptive-conformance
  This is the default mode. In this mode, it runs all `e2e` tests that are non-disruptive to the workloads in the cluster.
- certified-conformance
  This mode runs all `Conformance` tests as required for the Certified Kubernetes COnformance Program. 

Allright! 
Let's get hands on and see `Sonobuoy` in action.
All the code is available on my [Github](https://github.com/eratnch/hello-sonobuoy)

## Validate k8s cluster with sonobuoy

We are going to run the tests on a kubernetes cluster created by using [Kind](https://kind.sigs.k8s.io/).

If you're new to `Kind`, it's a tool for running kubernetes clusters using Docker containers. It's one of the easiest ways to spin up a multinode kubernetes cluster in no time!

### Create a Kind cluster with multiple nodes

Create the `kind` configuration file to set up 1 control-plane and 3 worker nodes in the cluster.

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

Check that the kubernetes cluster is up and pods are in running state.

```bash
$ kubectl get po -A
NAMESPACE            NAME                                                READY   STATUS    RESTARTS   AGE
kube-system          coredns-f9fd979d6-dp444                             1/1     Running   0          119s
kube-system          coredns-f9fd979d6-pq9rr                             1/1     Running   0          119s
kube-system          etcd-conformance-control-plane                      1/1     Running   0          2m9s
kube-system          kindnet-b6mrv                                       1/1     Running   0          109s
kube-system          kindnet-sg4rk                                       1/1     Running   0          119s
kube-system          kindnet-zktrp                                       1/1     Running   0          110s
kube-system          kindnet-zxg2r                                       1/1     Running   0          110s
kube-system          kube-apiserver-conformance-control-plane            1/1     Running   0          2m9s
kube-system          kube-controller-manager-conformance-control-plane   1/1     Running   0          2m9s
kube-system          kube-proxy-4bghm                                    1/1     Running   0          110s
kube-system          kube-proxy-76mpw                                    1/1     Running   0          109s
kube-system          kube-proxy-lwgmg                                    1/1     Running   0          110s
kube-system          kube-proxy-ndwks                                    1/1     Running   0          119s
kube-system          kube-scheduler-conformance-control-plane            1/1     Running   0          2m9s
local-path-storage   local-path-provisioner-78776bfc44-jzhws             1/1     Running   0          119s

```

## Versions

Kind
```bash
~$ kind version
kind v0.9.0 go1.15.2 linux/amd64
```
