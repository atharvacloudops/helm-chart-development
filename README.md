# Helm Chart Development & Chart Museum

A production-grade Helm chart project covering values schema validation,
lifecycle hooks, subchart dependencies, multi-environment management
and CI pipeline with lint, template validation and chart packaging.

---

helm-chart-development/
├── myapp/
│   ├── Chart.yaml                    # Chart metadata and dependencies
│   ├── values.yaml                   # Default values
│   ├── values.schema.json            # JSON schema validation
│   └── templates/
│       ├── deployment.yaml           # App deployment with rolling update
│       ├── service.yaml              # Kubernetes service
│       ├── hpa.yaml                  # Horizontal pod autoscaler
│       ├── pdb.yaml                  # Pod disruption budget
│       ├── networkpolicy.yaml        # Network policy
│       ├── migration-job.yaml        # Pre-install DB migration hook
│       ├── _helpers.tpl              # Reusable named templates
│       └── tests/
│           └── test-connection.yaml  # Helm connection test pod
├── helmfile.yaml                     # Multi-environment declarative management
├── values-dev.yaml                   # Development environment values
├── values-staging.yaml               # Staging environment values
├── values-prod.yaml                  # Production environment values
├── dev-rendered.yaml                 # Rendered dev manifests
├── staging-rendered.yaml             # Rendered staging manifests
├── prod-rendered.yaml                # Rendered production manifests
├── myapp-1.2.3.tgz                   # Packaged Helm chart
└── .github/workflows/
└── helm-ci.yml                   # Lint, template validation, package CI

---

## Environment Comparison

| Setting | Dev | Staging | Production |
|---|---|---|---|
| Replicas | 1 | 2 | 3 |
| Autoscaling | Off | Off | On (3-10) |
| Ingress | Off | On | On |
| Monitoring | Off | Off | On |
| DB Memory | 128Mi | 256Mi | 512Mi |

---

## Key Features

### Values Schema Validation
JSON Schema defined in values.schema.json validates all values on install.
Invalid values fail immediately with a clear error before anything deploys.

### Lifecycle Hooks
Pre-install migration job runs database migrations before the app starts.
Hook deletion policy set to hook-succeeded to keep the cluster clean.

### Subchart Dependencies
PostgreSQL and Redis pulled from Bitnami as subchart dependencies.
Conditional deployment — postgresql only installs when postgresql.enabled is true.
Per-environment resource sizing configured via parent chart values overrides.

### Multi-Environment Management
Three separate values files for dev, staging and production.
Helmfile manages all three environments declaratively in one file.
Single command deploys the right config to the right namespace.

### HPA + PDB + NetworkPolicy
Horizontal Pod Autoscaler scales between 3 and 10 replicas in production.
Pod Disruption Budget ensures minimum 1 pod always available during updates.
Network Policy restricts ingress and egress to only allowed pods and DNS.

### CI Pipeline
GitHub Actions runs on every push with three stages in sequence.
Helm Lint validates chart syntax and values across all three environments.
Template validation renders manifests for dev, staging and production.
Package stage produces myapp-1.2.3.tgz and uploads it as a build artifact.

---

## How to Use

### Lint the chart
```bash
helm lint myapp/
helm lint myapp/ --values values-dev.yaml
helm lint myapp/ --values values-staging.yaml
helm lint myapp/ --values values-prod.yaml
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

### Update subchart dependencies
```bash
helm dependency update myapp/
```

---

## Tech Stack

![Helm](https://img.shields.io/badge/Helm-0F1689?style=flat&logo=helm&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat&logo=github-actions&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat&logo=redis&logoColor=white)
