# Helm

- [Helm Docs](https://helm.sh/docs/)
- [Helm Releases](https://github.com/helm/helm/releases)

## Install Helm in Ubuntu 18.04

```bash
wget https://get.helm.sh/helm-v3.4.2-linux-amd64.tar.gz
tar -zxvf helm-v3.4.2-linux-amd64.tar.gz
sudo mv linux-amd64/helm /usr/local/bin/helm
rm -rf linux-amd64/ helm-v3.4.2-linux-amd64.tar.gz # Optional
```

## Find the Version

```bash
helm version
```

## Initialize a Helm Chart Repository

```bash
helm repo add stable https://charts.helm.sh/stable
```

## Search the available charts

```bash
helm search repo stable
```

## Update the list of charts

```bash
helm repo update
```

## Install the helm chart

```bash
helm install <repo-name>/<chart-name>
# As per the above instructions `repo-name` would be `stable`, `chart-name` could be anything like `mysql`
```

## Get the information about helm chart

```bash
helm show all <repo-name>/<chart-name>
```

## Learn about installed helm chart

```bash
helm ls
helm list
```

## Uninstall helm chart

```bash
helm uninstall <chart-instance-name>
# `chart-instance-name` would be the name given to chart when chart had been installed
```
