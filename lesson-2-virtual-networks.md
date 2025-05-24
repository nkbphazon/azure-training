# Lesson 2: Virtual Networks

## Learning Objectives
- Create a resource group using Azure CLI
- Create a virtual network with specific address space
- Create network security groups using Azure portal
- Create subnets and associate them with NSGs
- Understand basic Azure networking concepts

## Pre-Lesson Setup

**🤝 Partner Assignment:** Form pairs of two students each. You'll work together throughout this lesson with specific tasks assigned to each partner.

**🌍 Region Requirement:** All resources must be created in the **South Central US** region for this training.

## Task 1: Create a Resource Group

**👥 Both Students:** Decide on a unique resource group name together.

**Naming Convention:** Use the format: `rg-[lastname1]-[lastname2]-[date]`  
**Example:** `rg-smith-jones-20250524`

**🔵 One Student:** Create the resource group using Azure CLI.

**🔍 Research Challenge:** Find the correct parameters to create a resource group in South Central US.

```bash
az group ______ \
    --____ "YOUR_UNIQUE_RG_NAME" \
    --________ "___________"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az group create \
    --name "YOUR_UNIQUE_RG_NAME" \
    --location "southcentralus"
```

**Location options for South Central US:**
- `southcentralus`
- `South Central US`

</details>

**Verify creation:**
```bash
az group show \
    --name "YOUR_UNIQUE_RG_NAME" \
    --output table
```

**✅ Checkpoint:** Both partners should be able to see the resource group in the Azure portal at portal.azure.com

## Task 2: Create Virtual Network

**🔵 One Student:** Create a virtual network with address space 192.168.10.0/24.

**🔍 Research Challenge:** Find the correct parameters to create a VNet with the specified address space.

```bash
az network vnet ______ \
    --____ "vnet-training" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --________ "southcentralus" \
    --address-_______ "___.___.___.___/___"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az network vnet create \
    --name "vnet-training" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --location "southcentralus" \
    --address-prefix "192.168.10.0/24"
```

</details>

**Verify VNet creation:**
```bash
az network vnet list \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --output table
```

**📚 Understanding CIDR:**
- `/24` means the first 24 bits are network bits
- This provides 256 IP addresses (192.168.10.0 - 192.168.10.255)
- Azure reserves the first 4 and last 1 IP addresses

## Task 3: Create Network Security Groups (Portal)

**🌐 Azure Portal Instructions:** For this task, both partners will use the Azure Portal instead of CLI.

**Portal URL:** https://portal.azure.com

### 🟢 Student A - Create NSG-A

**Step 1:** Navigate to Network Security Groups
- Search for "Network Security Groups" in the portal
- Click "Create"

**Step 2:** Configuration
- **Name:** `nsg-student-a`
- **Resource Group:** [Your shared RG]
- **Region:** South Central US

**Step 3:** Complete creation
- Click "Review + Create" then "Create"

### 🟠 Student B - Create NSG-B

**Step 1:** Navigate to Network Security Groups
- Search for "Network Security Groups" in the portal
- Click "Create"

**Step 2:** Configuration
- **Name:** `nsg-student-b`
- **Resource Group:** [Your shared RG]
- **Region:** South Central US

**Step 3:** Complete creation
- Click "Review + Create" then "Create"

> **⏱️ Wait Time:** NSG creation typically takes 1-2 minutes. Wait for both to complete before proceeding.

## Task 4: Create Subnets with NSG Association (Portal)

**📋 Subnet Requirements:**
- **Subnet A:** 192.168.10.0/25 (128 addresses: .0-.127)
- **Subnet B:** 192.168.10.128/25 (128 addresses: .128-.255)

### 🟢 Student A - Create Subnet A

**Step 1:** Navigate to your VNet
- Go to Virtual Networks in the portal
- Click on "vnet-training"
- Click "Subnets" in the left menu

**Step 2:** Add Subnet
- Click "+ Subnet"
- **Name:** `subnet-student-a`
- **Address range:** `192.168.10.0/25`
- **Network Security Group:** `nsg-student-a`

**Step 3:** Save
- Click "Save"

### 🟠 Student B - Create Subnet B

**Step 1:** Navigate to your VNet
- Go to Virtual Networks in the portal
- Click on "vnet-training"
- Click "Subnets" in the left menu

**Step 2:** Add Subnet
- Click "+ Subnet"
- **Name:** `subnet-student-b`
- **Address range:** `192.168.10.128/25`
- **Network Security Group:** `nsg-student-b`

**Step 3:** Save
- Click "Save"

## Lesson 2 Verification

Verify all resources using CLI:

```bash
# List all resources in your resource group
az resource list --resource-group "YOUR_UNIQUE_RG_NAME" --output table

# Check VNet details
az network vnet show \
    --name "vnet-training" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --output table

# List subnets
az network vnet subnet list \
    --vnet-name "vnet-training" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --output table
```

**✅ Success Criteria:**
- ✓ Resource group created
- ✓ Virtual network with 192.168.10.0/24 address space
- ✓ Two network security groups
- ✓ Two subnets with correct address ranges
- ✓ NSGs properly associated with respective subnets

---

**Previous:** [Lesson 1: Azure CLI Fundamentals](lesson-1-azure-cli-fundamentals.md)  
**Next:** [Lesson 3: Virtual Machines](lesson-3-virtual-machines.md)
