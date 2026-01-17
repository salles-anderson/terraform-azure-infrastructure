# Azure Terraform Infrastructure

Production-ready Azure infrastructure provisioned with Terraform, featuring App Service, Container Registry, CosmosDB, and observability.

![Azure](https://img.shields.io/badge/Azure-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        Azure Cloud                               │
│                                                                  │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │                    Resource Group                          │ │
│  │                                                            │ │
│  │  ┌──────────────┐    ┌──────────────┐    ┌─────────────┐  │ │
│  │  │  App Service │───▶│   Container  │    │   Azure     │  │ │
│  │  │   (Web App)  │    │   Registry   │    │    DNS      │  │ │
│  │  └──────┬───────┘    └──────────────┘    └─────────────┘  │ │
│  │         │                                                  │ │
│  │         ▼                                                  │ │
│  │  ┌──────────────┐    ┌──────────────┐                     │ │
│  │  │   CosmosDB   │    │   Storage    │                     │ │
│  │  │  (Database)  │    │   Account    │                     │ │
│  │  └──────────────┘    └──────────────┘                     │ │
│  │                                                            │ │
│  │  ┌──────────────────────────────────────────────────────┐ │ │
│  │  │              Observability (O11y)                     │ │ │
│  │  │  Application Insights + Log Analytics Workspace       │ │ │
│  │  └──────────────────────────────────────────────────────┘ │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

## Resources Provisioned

| Resource | Description |
|----------|-------------|
| Resource Group | Container for all resources |
| App Service Plan | Hosting plan for web applications |
| App Service | Web application hosting |
| Container Registry | Docker image storage |
| CosmosDB | NoSQL database |
| Storage Account | Blob storage for static files |
| Azure DNS | Custom domain management |
| Application Insights | Application monitoring |
| Log Analytics | Centralized logging |

## Prerequisites

- Azure subscription
- Terraform >= 1.0
- Azure CLI
- GitHub account (for CI/CD)

## Project Structure

```
.
├── terraform/
│   ├── main.tf              # Main configuration (if exists)
│   ├── providers.tf         # Azure provider configuration
│   ├── backend.tf           # Remote state configuration
│   ├── variables.tf         # Input variables
│   ├── outputs.tf           # Output values
│   ├── terraform.tf         # Terraform settings
│   ├── data.tf              # Data sources
│   ├── resource-group.tf    # Resource group
│   ├── app-service.tf       # App Service resources
│   ├── container-registry.tf # ACR configuration
│   ├── cosmosdb.tf          # CosmosDB database
│   ├── storage-account.tf   # Storage account
│   ├── dns.tf               # DNS zone and records
│   ├── api.tf               # API Management (if used)
│   └── o11y.tf              # Observability resources
└── .github/
    └── workflows/
        └── pipeline.yml     # CI/CD pipeline
```

## Quick Start

### 1. Clone the repository

```bash
git clone https://github.com/salles-anderson/azure-terraform-infrastructure.git
cd azure-terraform-infrastructure
```

### 2. Configure Azure credentials

```bash
az login
az account set --subscription "<subscription-id>"
```

### 3. Initialize Terraform

```bash
cd terraform
terraform init
```

### 4. Plan and Apply

```bash
terraform plan -out=tfplan
terraform apply tfplan
```

## CI/CD Pipeline

The project includes a GitHub Actions pipeline for automated deployments.

### Required Secrets

Configure these secrets in your GitHub repository:

| Secret | Description |
|--------|-------------|
| `AZURE_CLIENT_ID` | Service Principal App ID |
| `AZURE_CLIENT_SECRET` | Service Principal Secret |
| `AZURE_SUBSCRIPTION_ID` | Azure Subscription ID |
| `AZURE_TENANT_ID` | Azure AD Tenant ID |

### Pipeline Stages

1. **Checkout** - Clone repository
2. **Azure Login** - Authenticate with Azure
3. **Terraform Init** - Initialize providers
4. **Terraform Validate** - Validate configuration
5. **Terraform Plan** - Preview changes
6. **Terraform Apply** - Apply changes (main branch only)

## Configuration

### Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `environment` | Environment name (dev/staging/prod) | Yes |
| `location` | Azure region | Yes |
| `app_name` | Application name | Yes |

### Backend Configuration

Configure remote state in `backend.tf`:

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "tfstate-rg"
    storage_account_name = "tfstate<unique>"
    container_name       = "tfstate"
    key                  = "terraform.tfstate"
  }
}
```

## Outputs

After applying, Terraform will output:

- App Service URL
- Container Registry login server
- CosmosDB connection string
- Storage Account connection string

## Security Best Practices

- Use Azure Key Vault for secrets
- Enable managed identities
- Configure network restrictions
- Enable diagnostic logging
- Use private endpoints for PaaS services

## Cost Optimization

- Use appropriate SKUs for each environment
- Enable auto-scaling for App Service
- Configure CosmosDB throughput based on usage
- Use reserved capacity for production

## Author

**Anderson Sales** - DevOps Cloud Engineer

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/salesanderson)
