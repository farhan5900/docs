# Kind

- [Official Kind Repo](https://github.com/kubernetes-sigs/kind/)
- [Location for Hosted Kind Images](https://hub.docker.com/r/kindest/node/)
- [Kind Releases](https://github.com/kubernetes-sigs/kind/releases)
- [Pre-built Kind Image](https://kind.sigs.k8s.io/docs/design/node-image)
- [Kind Example Config](https://raw.githubusercontent.com/kubernetes-sigs/kind/master/site/content/docs/user/kind-example-config.yaml)

## Install Kind on Ubuntu 18.04

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.9.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

## Find version

```bash
kind version
```

## Creating a Cluster

```bash
kind create cluster
```

## Getting List of Clusters

```bash
kind get clusters
```

## To interact with a specific cluster

```bash
kubectl cluster-info --context <full-cluster-name>
# By default `full-cluster-name` would be `kind-kind`
```

## Loading an Image into Cluster

```bash
kind load docker-image <image-name:tag> --name <cluster-name>
# By default `cluster-name` would be `kind`
```

## List the loaded image on a node of the Cluster

```bash
docker exec -ti <node-name> crictl images
# By default `node-name` would be `kind-control-plane`
# To get the node names
kubectl get nodes [--context <full-cluster-name>]
```

## Exporting Cluster Logs

```bash
kind export logs [/path/to/store/logs]
```

## Deleting a cluster

```bash
kind delete cluster
```
