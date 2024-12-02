# Helm Example App

This repository demonstrates how to set up and manage a JupyterHub deployment using Helm charts.

## Prerequisites

- Kubernetes cluster
- Helm 3.x installed
- kubectl configured to connect to your cluster

## Repository Structure

```
.
├── environments/
│   ├── dev/
│   │   └── values.yaml    # Development environment values
│   ├── sit/
│   │   └── values.yaml    # System Integration Testing environment values
│   └── uat/
│       └── values.yaml    # User Acceptance Testing environment values
└── manifest/
    └── 4.0.0/            # JupyterHub chart template version 4.0.0
```

## Setup Instructions

1. Add the JupyterHub Helm repository:
```shell
helm repo add jupyterhub https://hub.jupyter.org/helm-chart/
```

2. Update Helm repositories:
```shell
helm repo update
```

3. Download and extract the JupyterHub chart:
```shell
helm pull jupyterhub/jupyterhub
tar zxvf jupyterhub-4.0.0.tgz -C .
mv jupyterhub 4.0.0
```

4. Copy the default values file to each environment:
```shell
cp manifest/4.0.0/values.yaml environments/dev/values.yaml
cp manifest/4.0.0/values.yaml environments/sit/values.yaml
cp manifest/4.0.0/values.yaml environments/uat/values.yaml
```

## Deployment or Dryrun

### Using helm template
To generate and view the Kubernetes manifests without connecting to a cluster:

```shell
# For development environment
helm template jupyterhub jupyterhub/jupyterhub \
    --values environments/dev/values.yaml \
    --output-dir ./manifests/dev

# For SIT environment
helm template jupyterhub jupyterhub/jupyterhub \
    --values environments/sit/values.yaml \
    --output-dir ./manifests/sit

# For UAT environment
helm template jupyterhub jupyterhub/jupyterhub \
    --values environments/uat/values.yaml \
    --output-dir ./manifests/uat
```

The generated manifests will be saved in the specified output directory for review.

### Using helm upgrade --dry-run

To simulate the deployment with cluster connection:

```shell
# For development environment
helm upgrade --install jupyterhub jupyterhub/jupyterhub \
    --namespace <namespace> \
    --create-namespace \
    --values environments/dev/values.yaml \
    --dry-run --debug

# For SIT environment
helm upgrade --install jupyterhub jupyterhub/jupyterhub \
    --namespace <namespace> \
    --create-namespace \
    --values environments/sit/values.yaml \
    --dry-run --debug

# For UAT environment
helm upgrade --install jupyterhub jupyterhub/jupyterhub \
    --namespace <namespace> \
    --create-namespace \
    --values environments/uat/values.yaml \
    --dry-run --debug
```

The `--dry-run` flag will simulate the installation without actually installing anything.
The `--debug` flag will output the generated manifests for inspection.

To deploy to a specific environment, use:

```shell
# For development
helm upgrade --install jupyterhub jupyterhub/jupyterhub \
    --namespace <namespace> \
    --create-namespace \
    --values environments/dev/values.yaml

# For SIT
helm upgrade --install jupyterhub jupyterhub/jupyterhub \
    --namespace <namespace> \
    --create-namespace \
    --values environments/sit/values.yaml

# For UAT
helm upgrade --install jupyterhub jupyterhub/jupyterhub \
    --namespace <namespace> \
    --create-namespace \
    --values environments/uat/values.yaml
```

## Configuration

Each environment can be configured by modifying the corresponding `values.yaml` file in the `environments` directory. Refer to the [JupyterHub Helm Chart documentation](https://zero-to-jupyterhub.readthedocs.io/) for detailed configuration options.

## Maintenance

To update the chart version:

1. Pull the new version
2. Extract it to the templates directory
3. Update the environment-specific values files as needed

## Contributing

1. Fork the repository
2. Create a feature branch
3. Submit a pull request

## License

This project is licensed under the MIT License - see the LICENSE file for details.