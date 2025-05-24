# Lesson 4: Azure Storage

## Learning Objectives
- Understand Azure Storage account types and replication options
- Create and configure Azure Storage accounts
- Work with Azure File Storage for shared file systems
- Manage Azure Blob Storage for object storage
- Configure storage access controls and networking
- Mount Azure File Shares on virtual machines
- Upload and download files using Azure CLI and portal

## Prerequisites
Complete previous lessons. You'll use the same resource group and virtual machines created earlier.

## Understanding Azure Storage

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

## Task 1: Create Storage Account (Portal)

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

## Task 2: Azure File Storage Configuration

### Subtask 2.1: Create File Share (Portal)

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

### Subtask 2.2: Upload Files to File Share (Portal)

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

### Subtask 2.3: Get File Share Connection Information

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

### Subtask 2.4: Mount File Share on Virtual Machine

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

### Subtask 2.5: Create Files from Both VMs

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

## Task 3: Azure Blob Storage Configuration

### Subtask 3.1: Create Blob Container (Portal)

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

### Subtask 3.2: Upload Files to Blob Storage (Portal)

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

### Subtask 3.3: Test Container Access

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

### Subtask 3.4: Use Azure CLI for Blob Operations

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

## Task 4: Configure Storage Security

### Understanding Storage Account Keys

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

### Subtask 4.1: Create Shared Access Signature (SAS)

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

### Subtask 4.2: Demonstrate Key Rotation and SAS Invalidation

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

### Subtask 4.3: Best Practices for Key Management

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

### Subtask 4.4: Configure Network Access

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

## Task 5: Storage Performance and Monitoring

### Subtask 5.1: Monitor Storage Metrics

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

### Subtask 5.2: Configure Lifecycle Management

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

## Lesson 4 Verification

### File Storage Verification
```bash
# From your VM, verify file share is still mounted and accessible
ls -la /mnt/azurefiles/team-documents/
cat /mnt/azurefiles/team-documents/vm-student-*.txt
```

### Blob Storage Verification
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

**Previous:** [Lesson 3: Virtual Machines](lesson-3-virtual-machines.md)  
**Next:** [Lesson 5: Key Vault](lesson-5-key-vault.md)
