# Helm Chart Development & Chart Museum

Production Helm chart for a multi-tier application covering values schema
validation, lifecycle hooks, subchart dependencies, multi-environment
management and OCI registry publishing.

## Project Structure
helm-chart-development/
├── myapp/
│   ├── Chart.yaml                    # Chart metadata and dependencies
│   ├── values.yaml                   # Default values
│   ├── values.schema.json            # JSON schema validation
│   └── templates/
│       ├── deployment.yaml           # App deployment
│       ├── service.yaml              # Kubernetes service
│       ├── hpa.yaml                  # Horizontal pod autoscaler
│       ├── pdb.yaml                  # Pod disruption budget
│       ├── networkpolicy.yaml        # Network policy
│       ├── migration-job.yaml        # Pre-install DB migration hook
│       ├── _helpers.tpl              # Reusable named templates
│       └── tests/
│           └── test-connection.yaml  # Helm test pod
├── helmfile.yaml                     # Multi-environment management
├── values-dev.yaml                   # Dev environment values
├── values-staging.yaml               # Staging environment values
├── values-prod.yaml                  # Production environment values
└── .github/workflows/
└── helm-ci.yml                   # Lint, template, package CI

## How to Use

### Lint the chart
```bash
helm lint myapp/
```

### Render templates
```bash
helm template myapp-dev myapp/ --values values-dev.yaml
helm template myapp-staging myapp/ --values values-staging.yaml
helm template myapp-prod myapp/ --values values-prod.yaml
```

### Package the chart
```bash
helm package myapp/
```

## Tech Stack
Helm · Kubernetes · Docker · GitHub Actions · PostgreSQL · Redis · Nginx