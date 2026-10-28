# NextAmp Dev: Infrastructure & Basic Provisioning

Creates:
- **Networking**: VNet + 4 subnets (`aca`, `psql`, `pep`, `mgmt`) + NSGs
- **Private DNS**: 6 private DNS zones linked to VNet (`blob`, `servicebus`, `keyvault`, `acr`, `openai`, `postgres`)
- **Identity**: 6 user-assigned managed identities (`submission-api`, `core`, `identity`, `intel-api`, `intel-worker`, `audit`)
- **Monitoring**: Log Analytics Workspace + Application Insights
- **Key Vault**: Azure Key Vault with Private Endpoint (`pep`) & RBAC authorization
- **Storage**: Azure Storage Account (Blob) with Private Endpoint (`pep`)
- **ACR**: Azure Container Registry
- **Service Bus**: Azure Service Bus Namespace
- **PostgreSQL**: Azure Database for PostgreSQL Flexible Server in delegated subnet `snet-psql` (credentials stored in Key Vault)
- **Container Apps Environment**: Internal VNet Container Apps Environment (`cae`) in delegated subnet `snet-aca`

## Prerequisites
Ensure required Azure resource providers are registered:
```bash
az provider register --namespace Microsoft.Network
az provider register --namespace Microsoft.OperationalInsights
az provider register --namespace Microsoft.ManagedIdentity
az provider register --namespace Microsoft.KeyVault
az provider register --namespace Microsoft.Storage
az provider register --namespace Microsoft.ContainerRegistry
az provider register --namespace Microsoft.ServiceBus
az provider register --namespace Microsoft.DBforPostgreSQL
az provider register --namespace Microsoft.App
```

## Run
```bash
az login
az account set --subscription "<subscription-id>"
cp terraform.tfvars.example terraform.tfvars   # then edit subscription_id and resource_group_name
terraform init
terraform fmt
terraform validate
terraform plan -out=tfplan      # READ the plan
terraform apply tfplan
```

## Clean up
```bash
terraform destroy
```
