## Kind

- [Official Kind Repo](https://github.com/kubernetes-sigs/kind/)
- [Location for Hosted Kind Images](https://hub.docker.com/r/kindest/node/)
- [Kind Releases](https://github.com/kubernetes-sigs/kind/releases)
- [Pre-built Kind Image](https://kind.sigs.k8s.io/docs/design/node-image)

# Install Kind on Ubuntu 18.04

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.9.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

# Find version

```bash
kind version
```

# Creating a Cluster

```bash
kind create cluster
```

# Getting List of Clusters

```bash
kind get clusters
```

# To interact with a specific cluster

```bash
kubectl cluster-info --context <cluster-name>
# By default `cluster-name` would be `kind-kind`
```

# Deleting a cluster

```bash
kind delete cluster
```
