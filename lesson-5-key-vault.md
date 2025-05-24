# Lesson 5: Key Vault

## Learning Objectives
- Create an Azure Key Vault with access policies
- Understand control plane vs data plane permissions
- Configure Key Vault access policies
- Store and retrieve secrets using CLI
- Transition from access policies to Azure RBAC
- Implement network access restrictions
- Use managed identities for secure access

## Prerequisites
Complete previous lessons. You'll use the same resource group created earlier.

## Understanding Control Plane vs Data Plane

**⚠️ Important Concept:** Azure Key Vault has two distinct permission layers:

**🔧 Control Plane:** Managing the Key Vault resource itself (create, delete, configure)
- Your Contributor role grants this access
- Controls who can manage the vault settings

**🔐 Data Plane:** Accessing the contents within Key Vault (secrets, keys, certificates)
- Requires separate permissions via access policies or RBAC roles
- Controls who can read/write actual secrets

## Task 1: Create Key Vault (Portal)

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

## Task 2: Attempt to Add Secret (Intentional Failure)

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

## Task 3: Configure Access Policies

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

## Task 4: Successfully Add a Secret

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

## Task 5: Retrieve Secret Using Azure CLI

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

## Task 6: Transition to Azure RBAC

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

## Task 7: List Secrets Using CLI

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

## Task 8: Configure Network Access Restrictions

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

## Task 9: Understanding Managed Identities

**📚 What are Managed Identities?** Managed identities provide Azure services with an automatically managed identity in Azure AD. This eliminates the need to store credentials in code or configuration files.

### System-Assigned Identity
- **Lifecycle:** Tied to the Azure resource (VM, App Service, etc.)
- **Creation:** Automatically created when enabled
- **Deletion:** Automatically deleted when resource is deleted
- **Use case:** Single resource needs identity
- **Uniqueness:** One per resource

### User-Assigned Identity
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

## Task 10: Enable VM System-Assigned Identity

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

## Task 11: Install Azure CLI on Ubuntu VM

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

## Task 12: Authenticate Using Managed Identity

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

## Task 13: Attempt Key Vault Access (Expected Failure)

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

## Task 14: Grant VM Access to Key Vault

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

## Task 15: Configure Key Vault Network Access for VMs

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

## Lesson 5 Verification

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

**Previous:** [Lesson 4: Azure Storage](lesson-4-azure-storage.md)  
**Next:** [Lesson 6: Azure App Services](lesson-6-azure-app-services.md)
