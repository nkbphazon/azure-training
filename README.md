# Azure Cloud Hands-On Training Guide

> **Interactive learning for Azure cloud fundamentals with hands-on exercises**

## Table of Contents

1. [Training Overview](#training-overview)
2. [Lesson 1: Azure CLI Fundamentals](#lesson-1-azure-cli-fundamentals)
   - [Task 1: Installation on Windows](#task-1-installation-on-windows)
   - [Task 2: Authentication](#task-2-authentication)
   - [Task 3: List Subscriptions](#task-3-list-subscriptions)
   - [Task 4: Set Active Subscription](#task-4-set-active-subscription)
3. [Lesson 2: Virtual Networks](#lesson-2-virtual-networks)
   - [Task 1: Create a Resource Group](#task-1-create-a-resource-group)
   - [Task 2: Create Virtual Network](#task-2-create-virtual-network)
   - [Task 3: Create Network Security Groups (Portal)](#task-3-create-network-security-groups-portal)
   - [Task 4: Create Subnets with NSG Association (Portal)](#task-4-create-subnets-with-nsg-association-portal)
4. [Lesson 3: Virtual Machines](#lesson-3-virtual-machines)
   - [Task 1: Deploy Virtual Machines (Portal)](#task-1-deploy-virtual-machines-portal)
   - [Task 2: Access Serial Console](#task-2-access-serial-console)
   - [Task 3: Install Nginx Web Server](#task-3-install-nginx-web-server)
   - [Task 4: Test Local Web Server](#task-4-test-local-web-server)
   - [Task 5: Test Network Connectivity](#task-5-test-network-connectivity)
   - [Task 6: Configure Network Security](#task-6-configure-network-security)
5. [Lesson 4: Azure Storage](#lesson-4-azure-storage)
   - [Task 1: Create Storage Account (Portal)](#task-1-create-storage-account-portal)
   - [Task 2: Azure File Storage Configuration](#task-2-azure-file-storage-configuration)
   - [Task 3: Azure Blob Storage Configuration](#task-3-azure-blob-storage-configuration)
   - [Task 4: Configure Storage Security](#task-4-configure-storage-security)
     - [Understanding Storage Account Keys](#understanding-storage-account-keys)
     - [Subtask 4.1: Create Shared Access Signature (SAS)](#subtask-41-create-shared-access-signature-sas)
     - [Subtask 4.2: Demonstrate Key Rotation and SAS Invalidation](#subtask-42-demonstrate-key-rotation-and-sas-invalidation)
     - [Subtask 4.3: Best Practices for Key Management](#subtask-43-best-practices-for-key-management)
     - [Subtask 4.4: Configure Network Access](#subtask-44-configure-network-access)
   - [Task 5: Storage Performance and Monitoring](#task-5-storage-performance-and-monitoring)
6. [Lesson 5: Key Vault](#lesson-5-key-vault)
   - [Task 1: Create Key Vault (Portal)](#task-1-create-key-vault-portal)
   - [Task 2: Attempt to Add Secret (Intentional Failure)](#task-2-attempt-to-add-secret-intentional-failure)
   - [Task 3: Configure Access Policies](#task-3-configure-access-policies)
   - [Task 4: Successfully Add a Secret](#task-4-successfully-add-a-secret)
   - [Task 5: Retrieve Secret Using Azure CLI](#task-5-retrieve-secret-using-azure-cli)
   - [Task 6: Transition to Azure RBAC](#task-6-transition-to-azure-rbac)
   - [Task 7: List Secrets Using CLI](#task-7-list-secrets-using-cli)
   - [Task 8: Configure Network Access Restrictions](#task-8-configure-network-access-restrictions)
   - [Task 9: Understanding Managed Identities](#task-9-understanding-managed-identities)
   - [Task 10: Enable VM System-Assigned Identity](#task-10-enable-vm-system-assigned-identity)
   - [Task 11: Install Azure CLI on Ubuntu VM](#task-11-install-azure-cli-on-ubuntu-vm)
   - [Task 12: Authenticate Using Managed Identity](#task-12-authenticate-using-managed-identity)
   - [Task 13: Attempt Key Vault Access (Expected Failure)](#task-13-attempt-key-vault-access-expected-failure)
   - [Task 14: Grant VM Access to Key Vault](#task-14-grant-vm-access-to-key-vault)
   - [Task 15: Configure Key Vault Network Access for VMs](#task-15-configure-key-vault-network-access-for-vms)
7. [Quick Reference Guide](#quick-reference-guide)
8. [Conclusion](#conclusion)

---

## Training Overview

This training consists of four progressive lessons designed to build your Azure expertise through practical exercises. Students will work individually for Lesson 1, then pair up for Lessons 2-4.

### Prerequisites
- Windows computer with internet access
- Azure subscription access (provided by instructor)
- Basic familiarity with command-line interfaces

---

## Lesson 1: Azure CLI Fundamentals

### Learning Objectives
By the end of this lesson, you will:
- Understand what Azure CLI is and its purpose
- Install Azure CLI on Windows
- Authenticate with Azure
- Execute basic commands to list subscriptions
- Set the active subscription for future commands

### What is Azure CLI?

Azure CLI (Command Line Interface) is a cross-platform command-line tool that provides a consistent way to interact with Azure services. It allows you to manage Azure resources through commands instead of using the Azure portal's graphical interface.

**Benefits of Azure CLI:**
- Automation and scripting capabilities
- Consistent experience across platforms
- Faster than navigating through the portal
- Infrastructure as Code (IaC) support
- Perfect for DevOps workflows

### Task 1: Installation on Windows

#### Method 1: MSI Installer (Recommended)

**Step 1:** Download the installer
- Visit: `https://aka.ms/installazurecliwindows`
- Download the MSI installer file

**Step 2:** Run the installer
- Double-click the downloaded MSI file
- Follow the installation wizard with default settings

**Step 3:** Verify installation
Open Command Prompt or PowerShell and run:
```bash
az --version
```

#### Method 2: PowerShell (Alternative)

Run PowerShell as Administrator and execute:
```powershell
# Download and install Azure CLI
Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi
Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'
```

> **Note:** You may need to restart your command prompt or PowerShell session after installation.

### Task 2: Authentication

Before using Azure CLI to manage resources, you need to authenticate with your Azure account.

#### Interactive Login (Most Common)

```bash
az login
```

This command will:
1. Open your default web browser
2. Prompt you to sign in to Azure
3. Display your available subscriptions in the terminal after successful authentication

#### Device Code Flow (Alternative)

If you can't open a browser from your current session:
```bash
az login --use-device-code
```

Follow the instructions to enter the device code on another device.

> **Success Indicator:** After authentication, you should see JSON output showing your available subscriptions.

### Task 3: List Subscriptions

Once authenticated, list all subscriptions associated with your account:

**🔍 Research Challenge:** Find the correct parameter to display the output in a readable table format.

```bash
az account list --______ _____
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az account list --output table
```

**Expected Output:**
```
Name                CloudName    SubscriptionId                        State    IsDefault
------------------  -----------  ------------------------------------  -------  -----------
Visual Studio       AzureCloud   12345678-1234-1234-1234-123456789012  Enabled  True
Pay-As-You-Go       AzureCloud   87654321-4321-4321-4321-210987654321  Enabled  False
```

**Output Format Options:**
- `--output table` - Human-readable table
- `--output json` - JSON format (default)
- `--output tsv` - Tab-separated values
- `--output yaml` - YAML format

</details>

### Task 4: Set Active Subscription

Your instructor will provide you with a specific subscription ID. Set it as your active subscription:

**🔍 Research Challenge:** Find the correct parameter to set the subscription.

```bash
az account ___ --___________ "YOUR_SUBSCRIPTION_ID_FROM_INSTRUCTOR"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az account set --subscription "YOUR_SUBSCRIPTION_ID_FROM_INSTRUCTOR"
```

Replace `YOUR_SUBSCRIPTION_ID_FROM_INSTRUCTOR` with the actual subscription ID provided by your instructor.

</details>

**Verify the active subscription:**
```bash
az account show --output table
```

> **Important:** All subsequent Azure CLI commands will operate within this subscription unless you explicitly change it.

### Practice Commands

Try these commands to get familiar with Azure CLI:

```bash
# Get help for az command
az --help

# List all available resource groups
az group list --output table

# Get current account information
az account show

# List all available locations
az account list-locations --output table
```

---

## Lesson 2: Virtual Networks

### Learning Objectives
- Create a resource group using Azure CLI
- Create a virtual network with specific address space
- Create network security groups using Azure portal
- Create subnets and associate them with NSGs
- Understand basic Azure networking concepts

### Pre-Lesson Setup

**🤝 Partner Assignment:** Form pairs of two students each. You'll work together throughout this lesson with specific tasks assigned to each partner.

**🌍 Region Requirement:** All resources must be created in the **South Central US** region for this training.

### Task 1: Create a Resource Group

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

### Task 2: Create Virtual Network

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

### Task 3: Create Network Security Groups (Portal)

**🌐 Azure Portal Instructions:** For this task, both partners will use the Azure Portal instead of CLI.

**Portal URL:** https://portal.azure.com

#### 🟢 Student A - Create NSG-A

**Step 1:** Navigate to Network Security Groups
- Search for "Network Security Groups" in the portal
- Click "Create"

**Step 2:** Configuration
- **Name:** `nsg-student-a`
- **Resource Group:** [Your shared RG]
- **Region:** South Central US

**Step 3:** Complete creation
- Click "Review + Create" then "Create"

#### 🟠 Student B - Create NSG-B

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

### Task 4: Create Subnets with NSG Association (Portal)

**📋 Subnet Requirements:**
- **Subnet A:** 192.168.10.0/25 (128 addresses: .0-.127)
- **Subnet B:** 192.168.10.128/25 (128 addresses: .128-.255)

#### 🟢 Student A - Create Subnet A

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

#### 🟠 Student B - Create Subnet B

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

### Lesson 2 Verification

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

## Lesson 3: Virtual Machines

### Learning Objectives
- Deploy virtual machines using Azure Portal
- Configure VM networking with existing infrastructure
- Access VMs through serial console
- Install and configure web services
- Test network connectivity between VMs
- Implement network security controls

### Prerequisites
Complete Lesson 2 first. You'll need the virtual network and subnets created in the previous lesson.

### Task 1: Deploy Virtual Machines (Portal)

**🌐 Azure Portal Deployment:** Each student will deploy their own VM using the Azure Portal.

**Portal URL:** https://portal.azure.com

#### 🟢 Student A - VM Deployment

**Step 1:** Navigate to Virtual Machines
- Search for "Virtual machines" in the portal
- Click "Create" → "Azure virtual machine"

**Step 2:** Basic Configuration
- **Resource Group:** [Your shared RG]
- **VM Name:** `[YourFirstName]-vm`
- **Region:** South Central US
- **Availability options:** No infrastructure redundancy required
- **Image:** Ubuntu Server 24.04 LTS - x64 Gen2
- **Size:** Standard_B1s

**Step 3:** Authentication
- **Authentication type:** Password
- **Username:** `azureuser`
- **Password:** [Create your own secure password]
- **Public inbound ports:** None

#### 🟠 Student B - VM Deployment

**Step 1:** Navigate to Virtual Machines
- Search for "Virtual machines" in the portal
- Click "Create" → "Azure virtual machine"

**Step 2:** Basic Configuration
- **Resource Group:** [Your shared RG]
- **VM Name:** `[YourFirstName]-vm`
- **Region:** South Central US
- **Availability options:** No infrastructure redundancy required
- **Image:** Ubuntu Server 24.04 LTS - x64 Gen2
- **Size:** Standard_B1s

**Step 3:** Authentication
- **Authentication type:** Password
- **Username:** `azureuser`
- **Password:** [Create your own secure password]
- **Public inbound ports:** None

#### Disk Configuration (Both Students)

**Step 4:** Disks Tab
- Keep all default options
- Click "Next: Networking"

#### Networking Configuration

**🟢 Student A Networking:**
- **Virtual network:** vnet-training
- **Subnet:** subnet-student-a
- **Public IP:** None
- **NIC network security group:** None

**🟠 Student B Networking:**
- **Virtual network:** vnet-training
- **Subnet:** subnet-student-b
- **Public IP:** None
- **NIC network security group:** None

**Step 6:** Complete Deployment
- Click "Review + Create"
- Review settings and click "Create"
- Wait for deployment to complete (3-5 minutes)

> **⏱️ Deployment Time:** VM creation typically takes 3-5 minutes. Both students should wait for their VMs to complete deployment before proceeding.

### Task 2: Access Serial Console

**Step 1:** Navigate to Serial Console
- Go to your VM in the Azure Portal
- In the left menu, find "Help" section
- Click "Serial console"
- Wait for the console to load

**Step 2:** Login to Your VM
- Press Enter to get a login prompt
- Enter username: `azureuser`
- Enter the password you created during VM setup

**Expected Output:**
```
Ubuntu 24.04.1 LTS vm-hostname tty1

vm-hostname login: azureuser
Password: [your-password]

Welcome to Ubuntu 24.04.1 LTS (GNU/Linux 6.8.0-1018-azure x86_64)
azureuser@vm-hostname:~$
```

**✅ Success:** You should see a Ubuntu command prompt with your username. If login fails, double-check your password.

### Task 3: Install Nginx Web Server

**Step 1:** Update Package Repository
```bash
sudo apt update
```

**Step 2:** Install Nginx
```bash
sudo apt install nginx -y
```

**Step 3:** Start and Enable Nginx
```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

**Step 4:** Verify Nginx Status
```bash
sudo systemctl status nginx
```
You should see "active (running)" in the status output.

**Step 5:** Create a Custom Welcome Page
```bash
echo "Hello from [YourName]'s VM!" | sudo tee /var/www/html/index.html
```
Replace `[YourName]` with your actual name.

### Task 4: Test Local Web Server

**Step 1:** Test Web Server Locally
```bash
curl http://localhost
```
**Expected output:** `Hello from [YourName]'s VM!`

**Step 2:** Check Your VM's Private IP Address
```bash
ip addr show eth0 | grep inet
```
Note down your IP address - it should be in the 192.168.10.x range.

**📊 Expected IP Ranges:**
- **Student A:** IP between 192.168.10.4 - 192.168.10.126
- **Student B:** IP between 192.168.10.132 - 192.168.10.254

### Task 5: Test Network Connectivity

**👥 Partner Exercise:** Work with your partner to test connectivity between your VMs.

**Step 1:** Share IP Addresses
- Tell your partner your VM's private IP address
- Note down your partner's IP address

**Step 2:** Test Network Connectivity
```bash
# Test ping connectivity
ping -c 4 [PARTNER_IP_ADDRESS]

# Test web server access
curl http://[PARTNER_IP_ADDRESS]
```
Replace `[PARTNER_IP_ADDRESS]` with your partner's actual IP address.

**✅ Success Criteria:** You should be able to ping your partner's VM AND retrieve their custom web page content.

### Task 6: Configure Network Security

**🌐 Azure Portal NSG Configuration:** Use the Azure Portal to configure Network Security Group rules to block web traffic between VMs.

#### 🟢 Student A - Block Student B

**Step 1:** Navigate to NSG
- Search for "Network security groups"
- Click on "nsg-student-a"
- Go to "Inbound security rules"

**Step 2:** Add Deny Rule
- Click "+ Add"
- **Source:** IP Addresses
- **Source IP addresses:** 192.168.10.128/25
- **Destination port ranges:** 80
- **Protocol:** TCP
- **Action:** Deny
- **Priority:** 100
- **Name:** DenyWebFromStudentB

#### 🟠 Student B - Block Student A

**Step 1:** Navigate to NSG
- Search for "Network security groups"
- Click on "nsg-student-b"
- Go to "Inbound security rules"

**Step 2:** Add Deny Rule
- Click "+ Add"
- **Source:** IP Addresses
- **Source IP addresses:** 192.168.10.0/25
- **Destination port ranges:** 80
- **Protocol:** TCP
- **Action:** Deny
- **Priority:** 100
- **Name:** DenyWebFromStudentA

**Step 3:** Test the Security Rules
Wait 1-2 minutes for the NSG rules to take effect, then test:

```bash
# This should still work (ping uses ICMP, not blocked)
ping -c 4 [PARTNER_IP_ADDRESS]

# This should now be blocked (HTTP traffic on port 80)
curl --connect-timeout 10 http://[PARTNER_IP_ADDRESS]
```

**✅ Expected Result:** Ping should work, but curl should timeout or be refused, proving the NSG rule is blocking HTTP traffic.

### Lesson 3 Verification

Use Azure CLI to confirm your VMs are running:

```bash
# List all VMs in your resource group
az vm list \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --output table

# Check VM power state
az vm list \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --show-details \
    --query "[].{Name:name, PowerState:powerState, PrivateIps:privateIps}" \
    --output table
```

**✅ Success Criteria:**
- ✓ Both VMs deployed successfully with Ubuntu 24.04
- ✓ Serial console access working for both students
- ✓ Nginx installed and serving custom web pages
- ✓ Local curl test successful on both VMs
- ✓ Initial cross-VM connectivity confirmed
- ✓ NSG rules blocking HTTP traffic between VMs
- ✓ Ping still works between VMs (ICMP not blocked)

**📚 Learning Points:**
- VMs can communicate within the same virtual network by default
- Network Security Groups provide layer 4 (transport) security
- NSG rules are evaluated by priority (lower numbers = higher priority)
- Different protocols (ICMP vs HTTP) can be controlled independently

---

## Lesson 4: Azure Storage

### Learning Objectives
- Understand Azure Storage account types and replication options
- Create and configure Azure Storage accounts
- Work with Azure File Storage for shared file systems
- Manage Azure Blob Storage for object storage
- Configure storage access controls and networking
- Mount Azure File Shares on virtual machines
- Upload and download files using Azure CLI and portal

### Prerequisites
Complete previous lessons. You'll use the same resource group and virtual machines created earlier.

### Understanding Azure Storage

**📚 Azure Storage Types:**
- **Blob Storage:** Object storage for unstructured data (documents, images, videos)
- **File Storage:** Fully managed file shares accessible via SMB protocol
- **Queue Storage:** Message storage for reliable messaging between components
- **Table Storage:** NoSQL key-value store for structured data

**🔄 Replication Options:**
- **LRS:** Locally Redundant Storage (3 copies in same datacenter)
- **ZRS:** Zone Redundant Storage (3 copies across availability zones)
- **GRS:** Geo-Redundant Storage (6 copies across two regions)
- **RA-GRS:** Read-Access Geo-Redundant Storage (GRS + read access to secondary)

### Task 1: Create Storage Account (Portal)

**👤 Student Assignment:** One student should create the Storage Account for the team. Both students will use it for different exercises.

**⚠️ Important Note:** Storage account names must be **globally unique**, 3-24 characters, and contain only lowercase letters and numbers.

**Step 1:** Navigate to Storage Accounts
- Search for "Storage accounts" in the Azure Portal
- Click "Create"

**Step 2:** Basic Configuration
- **Resource Group:** [Your shared RG]
- **Storage account name:** `strtraining[random-numbers]`
- **Region:** South Central US
- **Performance:** Standard
- **Redundancy:** Locally-redundant storage (LRS)

*Example name: strtraining2024567*

**Step 3:** Advanced Configuration
- Click "Advanced" tab
- **Require secure transfer:** Enabled
- **Allow Blob public access:** Disabled
- **Minimum TLS version:** Version 1.2
- Leave other settings as default

**Step 4:** Networking Configuration
- Click "Networking" tab
- **Connectivity method:** Enable public access from all networks
- Leave other settings as default

**Step 5:** Complete Creation
- Click "Review + Create"
- Click "Create"
- Wait for deployment to complete

**✅ Success:** Storage account should be created successfully. Note the exact name for use in later commands.

### Task 2: Azure File Storage Configuration

#### Subtask 2.1: Create File Share (Portal)

**🟢 Student A:** Create and configure the file share.

**Step 1:** Navigate to File Shares
- Go to your Storage Account in the portal
- Click "File shares" in the left menu under "Data storage"
- Click "+ File share"

**Step 2:** Create File Share
- **Name:** `shared-documents`
- **Tier:** Transaction optimized
- **Quota:** 5 GiB
- Click "Create"

**Step 3:** Verify File Share Creation
- You should see the file share listed
- Click on the file share name to explore its contents (should be empty initially)

#### Subtask 2.2: Upload Files to File Share (Portal)

**🟢 Student A:** Upload sample files to the file share.

**Step 1:** Navigate to File Share Contents
- Click on the "shared-documents" file share
- You should see an empty directory

**Step 2:** Create Directories and Upload Files
- Click "+ Add directory"
- **Name:** `team-documents`
- Click "OK"

**Step 3:** Upload Sample Files
- Click on the "team-documents" directory
- Click "Upload"
- Create a simple text file on your computer with content: "Hello from Student A"
- Save it as "student-a-notes.txt"
- Upload this file
- Click "Upload"

#### Subtask 2.3: Get File Share Connection Information

**🟢 Student A:** Retrieve connection information for mounting.

**Step 1:** Get Connection Script
- In the file share, click "Connect" at the top
- Select "Windows" tab
- Copy the PowerShell script shown (you'll use parts of this for Linux)
- Note the storage account name and file share name

**🔍 Research Challenge:** Find the Azure CLI command to show storage account connection strings.

```bash
az storage account ____-____________-______ \
    --____ "strtraining[your-numbers]" \
    --resource-_____ "YOUR_UNIQUE_RG_NAME"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az storage account show-connection-string \
    --name "strtraining[your-numbers]" \
    --resource-group "YOUR_UNIQUE_RG_NAME"
```

Note down the connection string for later use.

</details>

#### Subtask 2.4: Mount File Share on Virtual Machine

**👥 Both Students:** Each student will mount the file share on their respective VM.

**💻 VM Serial Console:** Use the Serial Console of your VM to execute these commands.

**Step 1:** Install CIFS Utilities
```bash
sudo apt update
sudo apt install cifs-utils -y
```

**Step 2:** Create Mount Point
```bash
sudo mkdir -p /mnt/azurefiles
```

**Step 3:** Get Storage Account Key (CLI from your local machine)

**🔍 Research Challenge:** Find the command to list storage account keys.

```bash
az storage account keys ____ \
    --______-____ "YOUR_UNIQUE_RG_NAME" \
    --account-____ "strtraining[your-numbers]"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az storage account keys list \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --account-name "strtraining[your-numbers]"
```

Copy the "value" of key1 for use in the mount command.

</details>

**Step 4:** Mount the File Share (On VM)
```bash
# Replace [STORAGE-ACCOUNT] with your storage account name
# Replace [ACCESS-KEY] with the key1 value from previous step
sudo mount -t cifs //[STORAGE-ACCOUNT].file.core.windows.net/shared-documents /mnt/azurefiles -o vers=3.0,username=[STORAGE-ACCOUNT],password=[ACCESS-KEY],dir_mode=0777,file_mode=0777,serverino
```

**Step 5:** Verify Mount and Access Files
```bash
# List contents of mounted file share
ls -la /mnt/azurefiles/

# Navigate to team-documents directory
ls -la /mnt/azurefiles/team-documents/

# Read the file uploaded by Student A
cat /mnt/azurefiles/team-documents/student-a-notes.txt
```

#### Subtask 2.5: Create Files from Both VMs

**🟢 Student A:** Create a file from your VM
```bash
echo "Hello from Student A's VM - $(date)" | sudo tee /mnt/azurefiles/team-documents/vm-student-a.txt
```

**🟠 Student B:** Create a file from your VM
```bash
echo "Hello from Student B's VM - $(date)" | sudo tee /mnt/azurefiles/team-documents/vm-student-b.txt
```

**👥 Both Students:** Verify you can see each other's files
```bash
ls -la /mnt/azurefiles/team-documents/
cat /mnt/azurefiles/team-documents/vm-student-*.txt
```

**✅ Success Criteria:** Both students should be able to see and read files created by their partner, demonstrating shared file access.

### Task 3: Azure Blob Storage Configuration

#### Subtask 3.1: Create Blob Container (Portal)

**🟠 Student B:** Create and configure blob containers.

**Step 1:** Navigate to Blob Containers
- Go to your Storage Account in the portal
- Click "Containers" in the left menu under "Data storage"
- Click "+ Container"

**Step 2:** Create Container
- **Name:** `private-documents`
- **Public access level:** Private (no anonymous access)
- Click "Create"

**Step 3:** Create Second Container
- Click "+ Container" again
- **Name:** `project-files`
- **Public access level:** Private (no anonymous access)
- Click "Create"

#### Subtask 3.2: Upload Files to Blob Storage (Portal)

**🟠 Student B:** Upload files to different containers.

**Step 1:** Upload to Private Container
- Click on the "private-documents" container
- Click "Upload"
- Create a text file on your computer with content: "Confidential document from Student B"
- Save it as "confidential.txt"
- Upload this file
- Click "Upload"

**Step 2:** Upload to Project Container
- Go back and click on the "project-files" container
- Click "Upload"
- Create a text file on your computer with content: "Project documentation from Student B"
- Save it as "project-info.txt"
- Upload this file
- Click "Upload"

#### Subtask 3.3: Test Container Access

**👥 Both Students:** Test access to different containers.

**Step 1:** Try Direct Blob URL
- In the "private-documents" container, click on your uploaded file
- Copy the "URL" shown in the properties
- Open this URL in a new browser tab - it should show an access denied error

**Step 2:** Verify Private Access
- In the "project-files" container, click on your uploaded file
- Copy the "URL" shown in the properties
- Open this URL in a new browser tab - it should also show an access denied error

**📚 Understanding Access Levels:**
- **Private:** No anonymous access, authentication required for all access
- All containers are private by default when blob public access is disabled
- Access requires authentication via account keys, SAS tokens, or Azure AD

#### Subtask 3.4: Use Azure CLI for Blob Operations

**👥 Both Students:** Use CLI to interact with blob storage.

**Step 1:** List Containers

**🔍 Research Challenge:** Find the command to list blob containers.

```bash
az storage container ____ \
    --account-____ "strtraining[your-numbers]" \
    --account-___ "[KEY-FROM-EARLIER]"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az storage container list \
    --account-name "strtraining[your-numbers]" \
    --account-key "[KEY-FROM-EARLIER]"
```

Use the storage account key you retrieved in the File Storage section.

</details>

**Step 2:** List Blobs in Container

**🔍 Research Challenge:** Find the command to list blobs in a specific container.

```bash
az storage blob ____ \
    --container-____ "private-documents" \
    --account-____ "strtraining[your-numbers]" \
    --account-___ "[KEY-FROM-EARLIER]"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az storage blob list \
    --container-name "private-documents" \
    --account-name "strtraining[your-numbers]" \
    --account-key "[KEY-FROM-EARLIER]"
```

</details>

**Step 3:** Upload File via CLI
Create a new text file from your VM:

```bash
echo "File uploaded via CLI from $(hostname)" > cli-upload.txt
```

**🔍 Research Challenge:** Find the command to upload a blob via CLI.

```bash
az storage blob ______ \
    --container-____ "private-documents" \
    --____ "cli-upload.txt" \
    --____ "cli-upload.txt" \
    --account-____ "strtraining[your-numbers]" \
    --account-___ "[KEY-FROM-EARLIER]"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az storage blob upload \
    --container-name "private-documents" \
    --name "cli-upload.txt" \
    --file "cli-upload.txt" \
    --account-name "strtraining[your-numbers]" \
    --account-key "[KEY-FROM-EARLIER]"
```

</details>

**Step 4:** Download File via CLI

**🔍 Research Challenge:** Find the command to download a blob via CLI.

```bash
az storage blob ________ \
    --container-____ "private-documents" \
    --____ "confidential.txt" \
    --____ "downloaded-confidential.txt" \
    --account-____ "strtraining[your-numbers]" \
    --account-___ "[KEY-FROM-EARLIER]"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az storage blob download \
    --container-name "private-documents" \
    --name "confidential.txt" \
    --file "downloaded-confidential.txt" \
    --account-name "strtraining[your-numbers]" \
    --account-key "[KEY-FROM-EARLIER]"
```

Verify the download:
```bash
cat downloaded-confidential.txt
```

</details>

### Task 4: Configure Storage Security

#### Understanding Storage Account Keys

**📚 Storage Account Keys Overview:**

Azure Storage accounts use two 512-bit storage account keys for authentication. These keys provide full access to your storage account and all its data.

**🔑 Key Characteristics:**
- **Two keys provided:** key1 and key2 (for zero-downtime rotation)
- **Full access:** Both keys provide complete access to all storage services
- **Base64 encoded:** Keys are long, randomly generated strings
- **Regeneratable:** Keys can be rotated/regenerated for security

**🔄 Key Rotation Strategy:**
1. Applications use key1 for daily operations
2. When rotation needed, regenerate key2
3. Update applications to use key2
4. Regenerate key1 (invalidates old key1)
5. Applications now use key2, ready for next rotation cycle

**🎫 SAS Token Relationship:**
- SAS tokens are cryptographically signed using account keys
- If the key used to sign a SAS token is regenerated, the SAS token becomes invalid
- This provides immediate revocation capability for compromised tokens

#### Subtask 4.1: Create Shared Access Signature (SAS)

**🟢 Student A:** Generate SAS tokens for secure access and learn about key rotation.

**Step 1:** View Current Storage Account Keys
```bash
az storage account keys list \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --account-name "strtraining[your-numbers]" \
    --output table
```

You should see two keys: key1 and key2. Note that both have the same permissions.

**Step 2:** Create Container-Level SAS (Portal)
- Go to the "private-documents" container
- Click "Shared access tokens" in the left menu
- **Permissions:** Check Read and List
- **Start time:** Current time
- **Expiry time:** 2 hours from now
- **Note:** Observe that the SAS is signed with one of your account keys
- Click "Generate SAS token and URL"
- Copy the "Blob SAS URL"

**Step 3:** Test SAS Access
- Open the SAS URL in a browser
- It should allow access to list container contents despite being a private container
- **Save this URL** - you'll use it to test key rotation effects

**Step 4:** Create Blob-Level SAS via CLI

**🔍 Research Challenge:** Find the command to generate a SAS token for a specific blob.

```bash
az storage blob generate-___ \
    --container-____ "private-documents" \
    --____ "confidential.txt" \
    --permissions r \
    --expiry $(date -u -d "2 hours" '+%Y-%m-%dT%H:%MZ') \
    --account-____ "strtraining[your-numbers]" \
    --account-___ "[KEY-FROM-EARLIER]"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az storage blob generate-sas \
    --container-name "private-documents" \
    --name "confidential.txt" \
    --permissions r \
    --expiry $(date -u -d "2 hours" '+%Y-%m-%dT%H:%MZ') \
    --account-name "strtraining[your-numbers]" \
    --account-key "[KEY-FROM-EARLIER]"
```

This returns a SAS token that can be appended to the blob URL for temporary access.

**Save this SAS token** - you'll test its validity after key rotation.

</details>

#### Subtask 4.2: Demonstrate Key Rotation and SAS Invalidation

**🟠 Student B:** Learn about key rotation and its security implications.

**Step 1:** Test Current SAS Token Validity
Before rotating keys, verify that the SAS tokens created by Student A are working:
- Use the SAS URL from Step 3 in a browser - it should work
- Test the blob-level SAS token by constructing the full URL:
```
https://strtraining[your-numbers].blob.core.windows.net/private-documents/confidential.txt?[SAS-TOKEN-FROM-STUDENT-A]
```

**Step 2:** Check Which Key Was Used for SAS
The SAS tokens were likely created using key1. Let's verify which key we used:
```bash
# Show both keys - note which one matches the key used for SAS generation
az storage account keys list \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --account-name "strtraining[your-numbers]" \
    --query "[].{KeyName:keyName,Value:value}" \
    --output table
```

**Step 3:** Rotate the Storage Account Key

**⚠️ Warning:** This will invalidate any SAS tokens signed with the regenerated key!

**🔍 Research Challenge:** Find the command to regenerate a storage account key.

```bash
az storage account keys _______ \
    --resource-_____ "YOUR_UNIQUE_RG_NAME" \
    --account-____ "strtraining[your-numbers]" \
    --key key1
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az storage account keys renew \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --account-name "strtraining[your-numbers]" \
    --key key1
```

**Alternative using Portal:**
1. Go to your Storage Account
2. Click "Access keys" under "Security + networking"
3. Click "Rotate key" next to key1
4. Confirm the rotation

</details>

**Step 4:** Verify Key Rotation
```bash
# Check that key1 has a new value
az storage account keys list \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --account-name "strtraining[your-numbers]" \
    --query "[].{KeyName:keyName,Value:value}" \
    --output table
```

The key1 value should now be different from before.

**Step 5:** Test SAS Token Invalidation
Now test the SAS tokens created by Student A:

- Try the container-level SAS URL in a browser - it should now return an "Authentication failed" error
- Try the blob-level SAS URL - it should also fail

**📚 What Happened:**
- The SAS tokens were cryptographically signed with the old key1
- When key1 was regenerated, those signatures became invalid
- Azure can no longer verify the authenticity of the SAS tokens
- This demonstrates immediate revocation capability for security incidents

**Step 6:** Create New SAS Token with New Key
```bash
# Create a new SAS token using the rotated key
az storage blob generate-sas \
    --container-name "private-documents" \
    --name "confidential.txt" \
    --permissions r \
    --expiry $(date -u -d "1 hour" '+%Y-%m-%dT%H:%MZ') \
    --account-name "strtraining[your-numbers]" \
    --account-key "[NEW-KEY1-VALUE]"
```

This new SAS token should work because it's signed with the current key1.

#### Subtask 4.3: Best Practices for Key Management

**👥 Both Students:** Understand key management best practices.

**📋 Key Rotation Best Practices:**

1. **Regular Rotation Schedule:**
   - Rotate keys every 90 days minimum
   - More frequently for high-security environments
   - Immediately after suspected compromise

2. **Zero-Downtime Rotation Process:**
   - Use key2 while rotating key1
   - Update applications to use key2
   - Then rotate key1
   - Alternating pattern ensures continuity

3. **Monitoring and Alerting:**
   - Monitor SAS token usage patterns
   - Alert on authentication failures (may indicate compromised tokens)
   - Track key rotation dates

4. **Emergency Procedures:**
   - Document steps for immediate key rotation
   - Have emergency contacts for after-hours incidents
   - Test rotation procedures regularly

**🔐 Alternative Authentication Methods:**
- **Azure AD authentication:** More secure than account keys
- **Managed identities:** Best for Azure resources
- **User delegation SAS:** Signed with Azure AD credentials, not account keys

#### Subtask 4.4: Configure Network Access

**🟠 Student B:** Implement network restrictions.

**Step 1:** Configure Firewall (Portal)
- Go to your Storage Account
- Click "Networking" in the left menu under "Security + networking"
- Select "Enabled from selected virtual networks and IP addresses"
- Under "Virtual networks", click "+ Add existing virtual network"
- **Virtual network:** vnet-training
- **Subnets:** Select both subnet-student-a and subnet-student-b
- Click "Add"

**Step 2:** Add Your Current IP
- Under "Firewall", click "+ Add your client IP address"
- This adds your current public IP to the allowed list
- Click "Save"

**Step 3:** Test Network Restrictions
Wait 2-3 minutes for changes to take effect, then test:

```bash
# This should still work from your VM (allowed subnet)
az storage container list \
    --account-name "strtraining[your-numbers]" \
    --account-key "[KEY-FROM-EARLIER]"
```

**📚 Network Access Options:**
- **All networks:** Open access (default)
- **Selected networks:** Only specified IPs and VNets
- **Private endpoints:** Only through private connections

### Task 5: Storage Performance and Monitoring

#### Subtask 5.1: Monitor Storage Metrics

**👥 Both Students:** Explore storage monitoring capabilities.

**Step 1:** View Storage Metrics (Portal)
- In your Storage Account, click "Metrics" under "Monitoring"
- **Metric:** Transactions
- **Aggregation:** Count
- **Time range:** Last hour
- Click "Apply"

**Step 2:** View Storage Insights
- Click "Insights" under "Monitoring"
- Explore the overview of storage usage, performance, and availability
- Notice the breakdown by storage service (Blob, File, etc.)

#### Subtask 5.2: Configure Lifecycle Management

**🟢 Student A:** Set up automated blob lifecycle policies.

**Step 1:** Create Lifecycle Policy (Portal)
- In your Storage Account, click "Lifecycle management" under "Data management"
- Click "+ Add a rule"
- **Rule name:** `delete-old-files`
- **Rule scope:** Limit blobs with filters
- Click "Next"

**Step 2:** Configure Rule Conditions
- **Blob type:** Block blobs
- **If:** Base blobs were last modified more than [30] days ago
- **Then:** Delete the blob
- Click "Next"

**Step 3:** Apply to Containers
- **Blob prefix:** Leave empty (applies to all)
- **Container names:** `private-documents,project-files`
- Click "Review + add"
- Click "Add"

**📚 Lifecycle Actions:**
- **Tier to cool:** Move to cheaper storage tier
- **Tier to archive:** Move to lowest cost tier (higher access latency)
- **Delete:** Remove blobs permanently

### Lesson 4 Verification

#### File Storage Verification
```bash
# From your VM, verify file share is still mounted and accessible
ls -la /mnt/azurefiles/team-documents/
cat /mnt/azurefiles/team-documents/vm-student-*.txt
```

#### Blob Storage Verification
```bash
# List containers
az storage container list \
    --account-name "strtraining[your-numbers]" \
    --account-key "[KEY-FROM-EARLIER]" \
    --output table

# List blobs in private container
az storage blob list \
    --container-name "private-documents" \
    --account-name "strtraining[your-numbers]" \
    --account-key "[KEY-FROM-EARLIER]" \
    --output table
```

**✅ Success Criteria:**
- ✓ Storage account created with appropriate configuration
- ✓ File share created and accessible from both VMs
- ✓ File share mounted on virtual machines using CIFS
- ✓ Both students can read/write shared files
- ✓ Blob containers created with different access levels
- ✓ Public vs private access working as expected
- ✓ Files uploaded and downloaded via CLI
- ✓ SAS tokens generated and functional
- ✓ Network restrictions configured and tested
- ✓ Storage monitoring and metrics explored
- ✓ Lifecycle management policy created

**📚 Key Learnings:**
- Azure File Storage provides SMB-compatible shared file systems
- Blob Storage offers scalable object storage with different access tiers
- Access levels control anonymous vs authenticated access
- SAS tokens provide time-limited access without sharing account keys
- Network restrictions add additional security layers
- Lifecycle management automates cost optimization
- Multiple authentication methods available (account keys, SAS, Azure AD)
- Storage performance and monitoring tools help optimize usage

---

## Lesson 5: Key Vault

### Learning Objectives
- Create an Azure Key Vault with access policies
- Understand control plane vs data plane permissions
- Configure Key Vault access policies
- Store and retrieve secrets using CLI
- Transition from access policies to Azure RBAC
- Implement network access restrictions
- Use managed identities for secure access

### Prerequisites
Complete previous lessons. You'll use the same resource group created earlier.

### Understanding Control Plane vs Data Plane

**⚠️ Important Concept:** Azure Key Vault has two distinct permission layers:

**🔧 Control Plane:** Managing the Key Vault resource itself (create, delete, configure)
- Your Contributor role grants this access
- Controls who can manage the vault settings

**🔐 Data Plane:** Accessing the contents within Key Vault (secrets, keys, certificates)
- Requires separate permissions via access policies or RBAC roles
- Controls who can read/write actual secrets

### Task 1: Create Key Vault (Portal)

**👤 Student Assignment:** One student should create the Key Vault for the team. The other student will be granted access in later steps.

**⚠️ Important Note:** Key Vault names must be **globally unique** across all Azure tenants. You may need to try multiple names if your first choice is taken.

**Step 1:** Navigate to Key Vault
- Search for "Key vaults" in the Azure Portal
- Click "Create"

**Step 2:** Basic Configuration
- **Resource Group:** [Your shared RG]
- **Key vault name:** `kv-training-[random-number]`
- **Region:** South Central US
- **Pricing tier:** Standard

*Example name: kv-training-2024-5678*

**Step 3:** Access Configuration (Important!)
- Click "Access configuration" tab
- **Permission model:** Vault access policy
- Leave other settings as default

**Step 4:** Complete Creation
- Click "Review + Create"
- Click "Create"
- Wait for deployment to complete

**✅ Success:** Key Vault should be created successfully. Note the exact name for use in later commands.

### Task 2: Attempt to Add Secret (Intentional Failure)

**❌ Expected Failure!** This step is designed to fail. This demonstrates the difference between control plane and data plane permissions.

**Step 1:** Try to Add a Secret (Portal)
- Go to your Key Vault in the portal
- Click "Secrets" in the left menu
- Click "+ Generate/Import"
- Try to create a secret with any name and value

**Step 2:** Expected Error
You should see an error message similar to:
```
"The user, group or application does not have secrets set permission 
on key vault 'kv-training-xxxx'. For help resolving this issue, 
please see https://go.microsoft.com/fwlink/?linkid=2125287"
```

**📚 Understanding the Error:**
- **Control Plane:** You can create and manage the vault (Contributor role)
- **Data Plane:** You cannot access vault contents without explicit permissions
- This separation provides enhanced security by design

### Task 3: Configure Access Policies

**👥 Both Students:** Each student should be granted access to the Key Vault. The student who created the vault should add both users.

**Step 1:** Navigate to Access Policies
- In your Key Vault, click "Access policies" in the left menu
- Click "+ Create"

**Step 2:** Configure Permissions
- **Secret permissions:** Check the boxes for Get, List, Set
- Leave Key permissions and Certificate permissions unchecked
- Click "Next"

**Step 3:** Select Principal
- Click "Select principal"
- Search for your username or email address
- Select your user account
- Click "Select"

**Step 4:** Complete Policy Creation
- Click "Next" until you reach "Review + create"
- Click "Create"
- **Repeat steps 1-4 for your partner**

**⏱️ Wait Time:** Access policy changes may take 1-2 minutes to take effect. Wait before proceeding to the next task.

### Task 4: Successfully Add a Secret

**Step 1:** Create a Secret (Portal)
- Go back to "Secrets" in your Key Vault
- Click "+ Generate/Import"
- **Upload options:** Manual
- **Name:** `database-password`
- **Value:** `SuperSecretPassword123!`
- Click "Create"

**Step 2:** Verify Secret Creation
- You should see the secret listed in the Secrets page
- Click on the secret name to view its details
- Click "Show Secret Value" to verify the content

**✅ Success:** The secret should be created successfully now that you have proper data plane permissions.

### Task 5: Retrieve Secret Using Azure CLI

**🔍 Research Challenge:** Find the correct parameters to retrieve a secret value.

```bash
az keyvault secret ____ \
    --vault-____ "YOUR_KEYVAULT_NAME" \
    --____ "database-password" \
    --_____ "value" \
    --______ tsv
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az keyvault secret show \
    --vault-name "YOUR_KEYVAULT_NAME" \
    --name "database-password" \
    --query "value" \
    --output tsv
```

Replace `YOUR_KEYVAULT_NAME` with your actual Key Vault name.

**Alternative: Get Full Secret Details**
```bash
az keyvault secret show \
    --vault-name "YOUR_KEYVAULT_NAME" \
    --name "database-password"
```

This shows all metadata including creation date, version, etc.

</details>

**✅ Expected Output:** You should see "SuperSecretPassword123!" returned by the CLI command.

### Task 6: Transition to Azure RBAC

**📚 Why Azure RBAC?** Azure RBAC is more secure and scalable than access policies. It provides better integration with Azure AD and follows the principle of least privilege more effectively.

**Step 1:** Assign RBAC Role (Portal)
- In your Key Vault, click "Access control (IAM)" in the left menu
- Click "+ Add" → "Add role assignment"
- **Role:** Search for and select "Key Vault Secrets Officer"
- Click "Next"

**Step 2:** Assign to Users
- **Assign access to:** User, group, or service principal
- Click "+ Select members"
- Search for and select both student usernames
- Click "Select"
- Click "Review + assign" → "Assign"

**Step 3:** Update Permission Model
- Go to "Access configuration" in your Key Vault
- Change **Permission model** to "Azure role-based access control"
- Click "Save"

**⚠️ Important:** After switching to RBAC, the old access policies will no longer be effective. Only RBAC role assignments will control access.

### Task 7: List Secrets Using CLI

**🔍 Research Challenge:** Find the correct command to list all secrets in a vault.

```bash
az keyvault secret ____ \
    --vault-____ "YOUR_KEYVAULT_NAME" \
    --______ table
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az keyvault secret list \
    --vault-name "YOUR_KEYVAULT_NAME" \
    --output table
```

</details>

**Test Secret Retrieval Still Works:**
```bash
az keyvault secret show \
    --vault-name "YOUR_KEYVAULT_NAME" \
    --name "database-password" \
    --query "value" \
    --output tsv
```

**✅ Verification:** You should be able to list and retrieve secrets using the new RBAC permissions.

### Task 8: Configure Network Access Restrictions

**⚠️ Instructor Discussion Required:** Discuss with your instructor which trusted networks to configure. Common options include your current public IP, corporate network ranges, or Azure VNet integration.

**Step 1:** Check Current Public IP
```bash
# Check your current public IP
curl https://api.ipify.org
```
Note this IP address for use in network restrictions.

**Step 2:** Configure Network Access (Portal)
- In your Key Vault, click "Networking" in the left menu
- Under "Firewalls and virtual networks":
- Select "Enable from selected networks"
- Add your current IP address in the "Firewall" section
- Click "Save"

**Step 3:** Test Network Restrictions
```bash
# This should still work from your allowed IP
az keyvault secret list \
    --vault-name "YOUR_KEYVAULT_NAME" \
    --output table
```

If you restricted access properly, this should work from your current location but not from other networks.

**📚 Network Security Options:**
- **Disable public access:** Only private endpoints can access
- **Selected networks:** Only specified IP ranges/VNets
- **All networks:** Open to internet (least secure)

### Task 9: Understanding Managed Identities

**📚 What are Managed Identities?** Managed identities provide Azure services with an automatically managed identity in Azure AD. This eliminates the need to store credentials in code or configuration files.

#### System-Assigned Identity
- **Lifecycle:** Tied to the Azure resource (VM, App Service, etc.)
- **Creation:** Automatically created when enabled
- **Deletion:** Automatically deleted when resource is deleted
- **Use case:** Single resource needs identity
- **Uniqueness:** One per resource

#### User-Assigned Identity
- **Lifecycle:** Independent Azure resource
- **Creation:** Created as standalone resource
- **Deletion:** Must be explicitly deleted
- **Use case:** Multiple resources share same identity
- **Uniqueness:** Can be assigned to multiple resources

**✅ Benefits of Managed Identities:**
- No credentials stored in code or configuration
- Automatic credential rotation by Azure
- Simplified access management using Azure RBAC
- Reduced security risk from credential exposure

### Task 10: Enable VM System-Assigned Identity

**Step 1:** Navigate to VM Identity Settings
- Go to your Virtual Machine in the Azure Portal
- Click "Identity" in the left menu under "Security"
- Click on the "System assigned" tab

**Step 2:** Enable System-Assigned Identity
- Set **Status** to "On"
- Click "Save"
- Click "Yes" when prompted to confirm
- Wait for the operation to complete

**Step 3:** Note the Object ID
- After enabling, you'll see an **Object (principal) ID**
- Copy this ID - you'll need it for role assignments
- This ID represents your VM's identity in Azure AD

**ℹ️ Note:** If the system-assigned identity is already enabled, you'll see the status as "On" and can proceed to the next task.

### Task 11: Install Azure CLI on Ubuntu VM

**💻 VM Serial Console:** Use the Serial Console of your VM (from Lesson 3) to execute these commands.

**Step 1:** Update Package Repository
```bash
sudo apt update
```

**Step 2:** Install Prerequisites
```bash
sudo apt install -y ca-certificates curl apt-transport-https lsb-release gnupg
```

**Step 3:** Add Microsoft Repository Key
```bash
curl -sL https://packages.microsoft.com/keys/microsoft.asc | \
    gpg --dearmor | \
    sudo tee /etc/apt/trusted.gpg.d/microsoft.gpg > /dev/null
```

**Step 4:** Add Azure CLI Repository
```bash
AZ_REPO=$(lsb_release -cs)
echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
    sudo tee /etc/apt/sources.list.d/azure-cli.list
```

**Step 5:** Install Azure CLI
```bash
sudo apt update && sudo apt install -y azure-cli
```

**Step 6:** Verify Installation
```bash
az --version
```
You should see the Azure CLI version information.

### Task 12: Authenticate Using Managed Identity

**Step 1:** Login with Managed Identity
```bash
az login --identity
```
This uses the VM's system-assigned managed identity to authenticate.

**Step 2:** Verify Authentication
```bash
az account show
```
This should show your subscription details, confirming successful authentication.

**✅ Success:** Your VM is now authenticated to Azure using its managed identity - no credentials stored on the VM!

### Task 13: Attempt Key Vault Access (Expected Failure)

**❌ Expected Failure!** This step is designed to fail. The VM's identity hasn't been granted access to the Key Vault yet.

**Step 1:** Try to List Secrets
```bash
az keyvault secret list --vault-name "YOUR_KEYVAULT_NAME"
```
Replace `YOUR_KEYVAULT_NAME` with your actual Key Vault name.

**Step 2:** Expected Error
You should see an error similar to:
```
The user, group or application does not have secrets list permission 
on key vault 'YOUR_KEYVAULT_NAME'
```

**📚 Why This Fails:** Even though the VM can authenticate to Azure, it hasn't been granted specific permissions to access the Key Vault's data plane.

### Task 14: Grant VM Access to Key Vault

**⚠️ Expected Additional Failure!** Even after granting permissions, this may still fail due to network restrictions. This is intentional to demonstrate layered security.

**Step 1:** Assign Key Vault Secrets User Role (Portal)
- In your Key Vault, go to "Access control (IAM)"
- Click "+ Add" → "Add role assignment"
- **Role:** "Key Vault Secrets User"
- Click "Next"

**Step 2:** Assign to VM Identity
- **Assign access to:** Managed identity
- Click "+ Select members"
- **Managed identity:** Virtual machine
- **Subscription:** Your subscription
- Select your VM from the list
- Click "Select" → "Review + assign" → "Assign"

**Step 3:** Test Access Again (Still Expected to Fail)
```bash
az keyvault secret list --vault-name "YOUR_KEYVAULT_NAME"
```
This will likely still fail due to network restrictions blocking the VM's subnet.

**📚 Network Restriction Impact:** The VM has the right permissions now, but the Key Vault firewall only allows your current IP address, not the VM's subnet.

### Task 15: Configure Key Vault Network Access for VMs

**Step 1:** Add Virtual Network to Key Vault Firewall (Portal)
- In your Key Vault, go to "Networking"
- Under "Virtual networks", click "+ Add a virtual network"
- **Virtual network:** vnet-training
- **Subnets:** Select both subnet-student-a and subnet-student-b
- Click "Add"

**Step 2:** Save Network Configuration
- Click "Save" at the bottom of the networking page
- Wait for the configuration to be applied (1-2 minutes)

**Step 3:** Test Access from VM (Should Now Work)
Return to your VM's serial console and try again:

```bash
# List secrets (should work now)
az keyvault secret list --vault-name "YOUR_KEYVAULT_NAME" --output table

# Retrieve specific secret
az keyvault secret show \
    --vault-name "YOUR_KEYVAULT_NAME" \
    --name "database-password" \
    --query "value" \
    --output tsv
```

**✅ Success:** Your VM can now access Key Vault secrets using its managed identity through the secured network!

### Lesson 4 Verification

Verify Key Vault configuration:

```bash
# Show Key Vault details
az keyvault show \
    --name "YOUR_KEYVAULT_NAME" \
    --output table

# List all secrets
az keyvault secret list \
    --vault-name "YOUR_KEYVAULT_NAME" \
    --output table

# Test secret retrieval
az keyvault secret show \
    --vault-name "YOUR_KEYVAULT_NAME" \
    --name "database-password" \
    --query "value" \
    --output tsv
```

**✅ Success Criteria:**
- ✓ Key Vault created with globally unique name
- ✓ Initial secret creation failure understood (control vs data plane)
- ✓ Access policies configured for both students
- ✓ Secret successfully created and retrieved
- ✓ Azure RBAC roles assigned correctly
- ✓ Permission model switched to RBAC
- ✓ Network access restrictions configured
- ✓ VM system-assigned identity enabled
- ✓ Azure CLI installed on Ubuntu VM
- ✓ VM authenticated using managed identity
- ✓ VM granted Key Vault Secrets User role
- ✓ Network firewall configured to allow VM subnets
- ✓ VM can retrieve secrets using CLI with managed identity

**📚 Key Learnings:**
- Control plane permissions (Contributor) ≠ data plane permissions
- Azure RBAC is more secure and scalable than access policies
- Network restrictions add an additional security layer
- Managed identities eliminate the need for stored credentials
- System-assigned identities are tied to the resource lifecycle
- User-assigned identities can be shared across resources
- Multiple security layers work together (RBAC + Network + Identity)
- VMs can securely access Azure services without credentials

---

## Quick Reference Guide

### Essential Azure CLI Commands

#### Authentication & Account Management
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

#### Resource Groups
```bash
# Create resource group
az group create --name "rg-name" --location "southcentralus"

# List resource groups
az group list --output table

# Delete resource group
az group delete --name "rg-name" --yes --no-wait
```

#### Virtual Networks
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

#### Key Vault
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
```

### Azure Regions
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

### Helpful Tips

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

### Useful Portal Shortcuts
- **Azure Portal:** https://portal.azure.com
- **Cloud Shell:** Click the shell icon (>_) in the portal top bar
- **Resource Groups:** Search "Resource groups" in the portal
- **Virtual Networks:** Search "Virtual networks" in the portal
- **Network Security Groups:** Search "Network security groups"

---

## Conclusion

Congratulations! You've completed a comprehensive hands-on journey through Azure's core services. You've learned how to:

1. **Use Azure CLI** - The foundation for automation and scripting
2. **Build Virtual Networks** - Create secure, isolated network environments
3. **Deploy Virtual Machines** - Provision and configure compute resources
4. **Manage Azure Storage** - Implement file shares and secure blob storage
5. **Implement Key Vault** - Securely manage secrets and credentials

### Next Steps

To continue your Azure learning journey:

1. **Explore Azure Resource Manager (ARM) templates** for Infrastructure as Code
2. **Learn about Azure App Services** for web application hosting
3. **Investigate Azure Active Directory** for identity management
4. **Study Azure Database services** for managed data solutions
5. **Discover Azure Monitor** for observability and logging

### Best Practices Learned

- Always use the principle of least privilege for security
- Implement network segmentation and security groups
- Use managed identities instead of storing credentials
- Follow consistent naming conventions for resources
- Leverage Azure RBAC for fine-grained access control
- Apply network restrictions for additional security layers
- Disable public access for storage when not required
- Use shared access signatures for temporary, controlled access
- Implement lifecycle management for cost optimization
- Rotate storage account keys regularly for security
- Understand the relationship between keys and SAS token validity
- Use dual-key rotation strategy for zero-downtime key management

**Keep practicing and building!** The hands-on experience you've gained is invaluable for real-world Azure implementations.