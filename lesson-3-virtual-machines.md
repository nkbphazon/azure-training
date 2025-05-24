# Lesson 3: Virtual Machines

## Learning Objectives
- Deploy virtual machines using Azure Portal
- Configure VM networking with existing infrastructure
- Access VMs through serial console
- Install and configure web services
- Test network connectivity between VMs
- Implement network security controls

## Prerequisites
Complete Lesson 2 first. You'll need the virtual network and subnets created in the previous lesson.

## Task 1: Deploy Virtual Machines (Portal)

**🌐 Azure Portal Deployment:** Each student will deploy their own VM using the Azure Portal.

**Portal URL:** https://portal.azure.com

### 🟢 Student A - VM Deployment

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

### 🟠 Student B - VM Deployment

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

### Disk Configuration (Both Students)

**Step 4:** Disks Tab
- Keep all default options
- Click "Next: Networking"

### Networking Configuration

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

## Task 2: Access Serial Console

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

## Task 3: Install Nginx Web Server

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

## Task 4: Test Local Web Server

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

## Task 5: Test Network Connectivity

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

## Task 6: Configure Network Security

**🌐 Azure Portal NSG Configuration:** Use the Azure Portal to configure Network Security Group rules to block web traffic between VMs.

### 🟢 Student A - Block Student B

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

### 🟠 Student B - Block Student A

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

## Lesson 3 Verification

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

**Previous:** [Lesson 2: Virtual Networks](lesson-2-virtual-networks.md)  
**Next:** [Lesson 4: Azure Storage](lesson-4-azure-storage.md)
