# Lab 11 - Ceph Cluster and Toolbox

## Goals

* Start up a Ceph cluster using Rook
* Start up the Ceph Toolbox container
* Access and play with the Ceph Toolbox

## Deploying a Ceph Cluster

Now that Rook is installed we can go ahead and create a Ceph cluster.

```
kubectl create -f https://raw.githubusercontent.com/packet-labs/Rook-on-Bare-Metal-Workshop/master/files/cluster.yml
```

Watch Rook creating pods and initializing your cluster.
```
watch -n 1 -d kubectl get CephClusters,deployments,pods --all-namespaces
```

Press ctrl-c to exit once you see that your cluster is `Created` state and in `HEALTH_OK`.

## Deploy Ceph Toolbox

Rook deploys containers with minimal images, we will need a *toolbox* container to have familiars CLI tools like `ceph`, `rbd` and `rados` commonly used for testing and debugging:

```
#  Create toolbox
kubectl apply -f https://raw.githubusercontent.com/rook/rook/release-1.0/cluster/examples/kubernetes/ceph/toolbox.yaml

# Wait for the toolbox to be Running
kubectl --namespace rook-ceph get pod -l app=rook-ceph-tools

# Open a terminal on the toolbox
kubectl --namespace rook-ceph exec -it $(kubectl --namespace rook-ceph get pod -l "app=rook-ceph-tools" -o jsonpath='{.items[0].metadata.name}') bash
ceph status

# You can run your usual ceph commands from this container
```


## Next Steps

Once you're done, proceed to [Lab12](Lab12.md)