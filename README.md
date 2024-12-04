# Helm Example App

This repository demonstrates how to set up and manage a JupyterHub deployment using Helm charts.

## Prerequisites

- Kubernetes cluster
- Helm 3.x installed
- kubectl configured to connect to your cluster

## Repository Structure

```
~/w/g/s/g/s/helm-example-app *main> tree -A -L3
.
├── README.md
├── manifests
│   └── 3.3.7 # Kyverno chart version, the directory name can be changed to the chart version
│       ├── Chart.lock
│       ├── Chart.yaml
│       ├── README.md
│       ├── charts
│       ├── templates
│       └── values.yaml
└── overlays # Environment specific values files
    ├── prod # Production environment
    │   └── values.yaml
    ├── sit # System Integration Testing environment
    │   └── values.yaml
    └── uat # User Acceptance Testing environment
        └── values.yaml

9 directories, 8 files
```

## Setup Instructions

1. Copy the default values file to each environment:
```shell
cp manifests/3.3.7/values.yaml overlays/prod/values.yaml
cp manifests/3.3.7/values.yaml overlays/sit/values.yaml
cp manifests/3.3.7/values.yaml overlays/uat/values.yaml
```

## Deployment or Dryrun

### Using helm template
To generate and view the Kubernetes manifests without connecting to a cluster:

```shell
# For development environment
~/w/g/s/g/s/helm-example-app *main> helm template kyverno manifests/3.3.7 \
                                            --values overlays/prod/values.yaml \
                                            --output-dir ./manifests/prod
wrote ./manifests/prod/kyverno/templates/admission-controller/serviceaccount.yaml
wrote ./manifests/prod/kyverno/templates/background-controller/serviceaccount.yaml
wrote ./manifests/prod/kyverno/templates/cleanup-controller/serviceaccount.yaml
wrote ./manifests/prod/kyverno/templates/reports-controller/serviceaccount.yaml
...
```

The generated manifests will be saved in the specified output directory for review.

### Using helm upgrade --dry-run

To simulate the deployment with cluster connection:

```shell
# For development environment
helm upgrade --install kyverno manifests/3.3.7 \
    --namespace kyverno \
    --create-namespace \
    --values overlays/prod/values.yaml \
    --dry-run --debug
```

The `--dry-run` flag will simulate the installation without actually installing anything.
The `--debug` flag will output the generated manifests for inspection.

## Configuration

Each environment can be configured by modifying the corresponding `values.yaml` file in the `environments` directory. Refer to the [JupyterHub Helm Chart documentation](https://zero-to-jupyterhub.readthedocs.io/) for detailed configuration options.

## Maintenance

To update the chart version:

1. Pull the new version
2. Extract it to the manifest directory
3. Update the environment-specific values files as needed

## Contributing

1. Fork the repository
2. Create a feature branch
3. Submit a pull request

## License

This project is licensed under the MIT License - see the LICENSE file for details.