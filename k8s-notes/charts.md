# Charts

- [Mesosphere Helm Chart](https://github.com/mesosphere/charts/)
- [Helm Docs](https://helm.sh/docs/)

Helm is the package manager for Kubernetes. Three big concepts of helm chart as follows:

- A `Chart` is a Helm package.
- A `Repository` is the place where charts can be collected and shared.
- A `Release` is an instance of a chart running in a Kubernetes cluster.

Helm installs `charts` into Kubernetes, creating a new `release` for each installation. And to find new charts, you can search Helm chart `repositorie
s`.

## 'helm search': Finding Charts

```bash
# Searches the Artifact Hub
helm search hub

# Searches the repositories
helm search repo
```

## 'helm install': Installing a Package

```bash
# Takes two mandatory arguments: release name and name of the chart
helm install happy-panda bitnami/wordpress

# To check the status of the installation
helm status happy-panda

# To check the values option available for the chart
helm show values bitnami/wordpress

# Install using custom values and auto generated release name
helm install -f values.yaml bitnami/wordpress --generate-name

# To check the values applied on the release
helm get values happy-panda
```

## 'helm upgrade' and 'helm rollback': Upgrading a Release, and Recovering on Failure

```bash
# Will only update things that have changed since the last release
helm upgrade -f panda.yaml happy-panda bitnami/wordpress

# If something does not go as planned, it is easy to roll back
helm rollback happy-panda 1

# First revision number is always 1, we can find out the revision history
helm history happy-panda
```

## 'helm uninstall': Uninstalling a Release

```bash
# Uninstall a release
helm uninstall happy-panda

# Check the available releases
helm list

# To check all the releases
helm list --all
```

In Helm 3, deletion removes the release record as well. If you with to keep a deletion release record, use `helm uninstall --keep-history`. Using `helm list --uninstalled` will only show releases that were uninstalled with the `--keep-history` flag. It is not possible to rollback an uninstalled resource.

## 'helm repo': Working with Repositories

```bash
# List the added repos
helm repo list

# Add new repo
helm repo add dev https://example.com/dev-charts

# Update or Sync the repo with upstream
helm repo update

# Remove the repo
helm repo remove dev
```

## 'helm create': Create your own Chart

```bash
# Generate basic chart structure
helm create deis-workflow

# To validate the chart
helm lint .

# Package chart for distribution
helm package deis-workflow
```

