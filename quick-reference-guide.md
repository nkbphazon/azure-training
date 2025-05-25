# Quick Reference Guide

## Essential Azure CLI Commands

### Authentication & Account Management
```bash
# Login to Azure
az login

# List subscriptions
az account list --output table

# Set active subscription
az account set --subscription "subscription-id"

# Show current account
az account show
```

### Resource Groups
```bash
# Create resource group
az group create --name "rg-name" --location "southcentralus"

# List resource groups
az group list --output table

# Delete resource group
az group delete --name "rg-name" --yes --no-wait
```

### Virtual Networks
```bash
# Create virtual network
az network vnet create \
    --name "vnet-name" \
    --resource-group "rg-name" \
    --address-prefix "10.0.0.0/16"

# List virtual networks
az network vnet list --resource-group "rg-name" --output table

# Create subnet
az network vnet subnet create \
    --name "subnet-name" \
    --vnet-name "vnet-name" \
    --resource-group "rg-name" \
    --address-prefix "10.0.1.0/24"
```

### Virtual Machines
```bash
# List VMs
az vm list --resource-group "rg-name" --output table

# Check VM power state
az vm list \
    --resource-group "rg-name" \
    --show-details \
    --query "[].{Name:name, PowerState:powerState, PrivateIps:privateIps}" \
    --output table

# Start VM
az vm start --name "vm-name" --resource-group "rg-name"

# Stop VM
az vm stop --name "vm-name" --resource-group "rg-name"
```

### Storage Accounts
```bash
# List storage accounts
az storage account list --resource-group "rg-name" --output table

# Get storage account keys
az storage account keys list \
    --resource-group "rg-name" \
    --account-name "storage-account-name"

# List containers
az storage container list \
    --account-name "storage-account-name" \
    --account-key "storage-key"

# Upload blob
az storage blob upload \
    --container-name "container-name" \
    --name "blob-name" \
    --file "local-file-path" \
    --account-name "storage-account-name" \
    --account-key "storage-key"

# Download blob
az storage blob download \
    --container-name "container-name" \
    --name "blob-name" \
    --file "local-file-path" \
    --account-name "storage-account-name" \
    --account-key "storage-key"
```

### Key Vault
```bash
# List secrets
az keyvault secret list --vault-name "vault-name" --output table

# Show secret
az keyvault secret show \
    --vault-name "vault-name" \
    --name "secret-name" \
    --query "value" \
    --output tsv

# Create secret
az keyvault secret set \
    --vault-name "vault-name" \
    --name "secret-name" \
    --value "secret-value"

# Delete secret
az keyvault secret delete \
    --vault-name "vault-name" \
    --name "secret-name"
```

### App Services
```bash
# Create App Service Plan
az appservice plan create \
    --name "plan-name" \
    --resource-group "rg-name" \
    --location "southcentralus" \
    --sku "B1" \
    --is-linux true

# Create Web App
az webapp create \
    --name "webapp-name" \
    --resource-group "rg-name" \
    --plan "plan-name" \
    --runtime "NODE|18-lts"

# Deploy code
az webapp up \
    --src-path . \
    --name "webapp-name" \
    --resource-group "rg-name"

# View logs
az webapp log tail \
    --name "webapp-name" \
    --resource-group "rg-name"
```

### Azure SQL Database
```bash
# Create SQL Server
az sql server create \
    --name "sql-server-name" \
    --resource-group "rg-name" \
    --location "southcentralus" \
    --admin-user "sqladmin" \
    --admin-password "YourPassword123!"

# Create SQL Database
az sql db create \
    --name "db-name" \
    --server "sql-server-name" \
    --resource-group "rg-name" \
    --edition "Basic" \
    --capacity 5

# Add firewall rule
az sql server firewall-rule create \
    --server "sql-server-name" \
    --resource-group "rg-name" \
    --name "rule-name" \
    --start-ip-address "0.0.0.0" \
    --end-ip-address "255.255.255.255"

# List databases
az sql db list \
    --server "sql-server-name" \
    --resource-group "rg-name" \
    --output table

# Show connection string
az sql db show-connection-string \
    --name "db-name" \
    --server "sql-server-name" \
    --client "ado.net"
```

### Azure Functions
```bash
# Create Function App
az functionapp create \
    --name "func-app-name" \
    --resource-group "rg-name" \
    --consumption-plan-location "southcentralus" \
    --runtime "node" \
    --runtime-version "18" \
    --functions-version "4" \
    --storage-account "storage-account-name"

# List functions
az functionapp function list \
    --name "func-app-name" \
    --resource-group "rg-name" \
    --output table

# Get function URL
az functionapp function show \
    --name "func-app-name" \
    --resource-group "rg-name" \
    --function-name "function-name" \
    --query "invokeUrlTemplate" \
    --output tsv

# View function logs
az functionapp log show-stream \
    --name "func-app-name" \
    --resource-group "rg-name"

# Deploy function code
func azure functionapp publish "func-app-name"
```

### Managed Identity
```bash
# Enable system-assigned identity on VM
az vm identity assign \
    --name "vm-name" \
    --resource-group "rg-name"

# Enable identity on Web App
az webapp identity assign \
    --name "webapp-name" \
    --resource-group "rg-name"

# Enable identity on Function App
az functionapp identity assign \
    --name "func-app-name" \
    --resource-group "rg-name"

# Login with managed identity (from VM)
az login --identity
```

## Azure Regions
```bash
# List all available regions
az account list-locations --output table

# Common region names:
# East US: eastus
# West US: westus
# South Central US: southcentralus
# West Europe: westeurope
# Southeast Asia: southeastasia
```

## Helpful Tips

**Command Structure:**
```
az [group] [subgroup] [command] [parameters]
```

**Getting Help:**
- `az --help` - General help
- `az network --help` - Network commands help
- `az network vnet --help` - VNet specific help

**Output Formats:**
- `--output table` - Human readable
- `--output json` - Machine readable
- `--output tsv` - Tab separated

**Query Examples:**
```bash
# Filter results with JMESPath
az vm list --query "[?powerState=='VM running'].name" --output table

# Get specific properties
az vm list --query "[].{Name:name, Location:location}" --output table

# Count resources
az vm list --query "length(@)" --output tsv
```

## Common Network Configurations

**CIDR Notation:**
- `/24` = 256 IP addresses (e.g., 192.168.1.0/24)
- `/25` = 128 IP addresses (e.g., 192.168.1.0/25)
- `/26` = 64 IP addresses (e.g., 192.168.1.0/26)
- `/28` = 16 IP addresses (e.g., 192.168.1.0/28)

**Reserved IP Addresses in Azure:**
- First 4 IPs in subnet are reserved by Azure
- Last IP in subnet is reserved for broadcast

## Security Best Practices

**Network Security Groups (NSGs):**
- Default rules allow VNet traffic and deny internet traffic
- Lower priority numbers = higher precedence
- Rules are stateful (return traffic automatically allowed)

**Key Vault:**
- Use Azure RBAC instead of access policies when possible
- Enable network restrictions for production environments
- Use managed identities instead of service principals

**Storage Accounts:**
- Disable public blob access unless required
- Use SAS tokens for temporary access
- Rotate storage account keys regularly
- Configure network restrictions

**App Services:**
- Use managed identities for Azure service authentication
- Store secrets in Key Vault, not application settings
- Use deployment slots for zero-downtime deployments
- Enable Application Insights for monitoring

**SQL Database:**
- Use Azure AD authentication when possible
- Store connection strings in Key Vault
- Enable transparent data encryption
- Configure geo-replication for critical databases
- Monitor DTU/vCore utilization

**Azure Functions:**
- Use consumption plan for cost optimization
- Implement retry policies for reliability
- Use Application Insights for monitoring
- Store secrets in Key Vault with managed identity
- Consider Durable Functions for complex workflows

## Troubleshooting Commands

### Database Troubleshooting
```bash
# Test SQL connectivity from Linux
sqlcmd -S server.database.windows.net -U username -P 'password' -d database -Q "SELECT @@VERSION"

# Check SQL Server firewall rules
az sql server firewall-rule list \
    --server "sql-server-name" \
    --resource-group "rg-name" \
    --output table

# View database metrics
az monitor metrics list \
    --resource "/subscriptions/{sub-id}/resourceGroups/{rg}/providers/Microsoft.Sql/servers/{server}/databases/{db}" \
    --metric "dtu_consumption_percent" \
    --interval PT1M
```

### Function App Troubleshooting
```bash
# Check function app status
az functionapp show \
    --name "func-app-name" \
    --resource-group "rg-name" \
    --query "state" \
    --output tsv

# List function keys
az functionapp function keys list \
    --name "func-app-name" \
    --resource-group "rg-name" \
    --function-name "function-name"

# Test function locally
func start
```

### General Troubleshooting
```bash
# Check resource status
az resource list --resource-group "rg-name" --output table

# View activity log
az monitor activity-log list --resource-group "rg-name" --max-events 10

# Check network connectivity
az network watcher test-connectivity \
    --source-resource "vm-name" \
    --dest-address "destination-ip" \
    --resource-group "rg-name"

# Validate ARM template
az deployment group validate \
    --resource-group "rg-name" \
    --template-file "template.json"
```

## Useful Portal Shortcuts
- **Azure Portal:** https://portal.azure.com
- **Cloud Shell:** Click the shell icon (>_) in the portal top bar
- **Resource Groups:** Search "Resource groups" in the portal
- **Virtual Networks:** Search "Virtual networks" in the portal
- **Network Security Groups:** Search "Network security groups"
- **Key Vaults:** Search "Key vaults" in the portal
- **Storage Accounts:** Search "Storage accounts" in the portal
- **App Services:** Search "App Services" in the portal
- **SQL Databases:** Search "SQL databases" in the portal
- **Function Apps:** Search "Function App" in the portal

## Resource Naming Conventions

**Recommended Patterns:**
- Resource Groups: `rg-[project]-[environment]-[region]`
- Virtual Networks: `vnet-[project]-[environment]`
- Subnets: `subnet-[purpose]-[environment]`
- VMs: `vm-[purpose]-[environment]-[number]`
- Storage Accounts: `st[project][environment][random]` (lowercase, no hyphens)
- Key Vaults: `kv-[project]-[environment]-[random]`
- App Services: `app-[project]-[environment]-[random]`
- SQL Servers: `sql-[project]-[environment]-[random]`
- SQL Databases: `db-[project]-[purpose]`
- Function Apps: `func-[project]-[environment]-[random]`

**Examples:**
- `rg-training-dev-scus`
- `vnet-training-dev`
- `subnet-web-dev`
- `vm-web-dev-01`
- `sttrainingdev001`
- `kv-training-dev-001`
- `app-training-dev-001`
- `sql-training-dev-001`
- `db-training-main`
- `func-training-dev-001`

---

**Previous:** [Lesson 8: Azure Functions](lesson-8-azure-functions.md)  
**Back to:** [Training Overview](README.md)
