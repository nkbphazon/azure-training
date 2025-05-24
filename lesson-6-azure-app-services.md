# Lesson 6: Azure App Services

## Learning Objectives
- Understand Azure App Services and its benefits for web application hosting
- Create and configure App Service Plans with different pricing tiers
- Deploy web applications using Azure Portal and CLI
- Configure application settings and environment variables
- Integrate App Services with Azure Key Vault for secure secret management
- Enable and configure managed identities for App Services
- Implement deployment slots for staging and production environments
- Configure VNet integration for secure network connectivity
- Monitor application performance and configure auto-scaling
- Set up continuous deployment from source control

## Prerequisites
Complete previous lessons. You'll use the same resource group, Key Vault, and virtual network created earlier.

## Understanding Azure App Services

**📚 What is Azure App Service?**

Azure App Service is a fully managed platform-as-a-service (PaaS) offering for building, deploying, and scaling web applications. It supports multiple programming languages and frameworks without managing the underlying infrastructure.

**🌟 Key Benefits:**
- **Fully Managed:** No server management required
- **Auto-scaling:** Automatically scale based on demand
- **Built-in DevOps:** Integrated CI/CD capabilities
- **Security:** Built-in authentication and authorization
- **Global Scale:** Deploy to multiple regions worldwide
- **Multiple Languages:** .NET, Java, Node.js, Python, PHP, Ruby

**🏗️ App Service Components:**
- **App Service Plan:** Defines compute resources (CPU, memory, storage)
- **Web App:** Your application running on the App Service Plan
- **Deployment Slots:** Separate environments for staging and production
- **Custom Domains:** Use your own domain names
- **SSL Certificates:** Secure your applications with HTTPS

## Task 1: Create App Service Plan

**👤 Student Assignment:** One student should create the App Service Plan for the team.

**🔍 Research Challenge:** Find the correct parameters to create an App Service Plan using Azure CLI.

```bash
az appservice plan ______ \
    --____ "asp-training-plan" \
    --resource-_____ "YOUR_UNIQUE_RG_NAME" \
    --________ "southcentralus" \
    --___ "B1" \
    --is-_____ true
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az appservice plan create \
    --name "asp-training-plan" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --location "southcentralus" \
    --sku "B1" \
    --is-linux true
```

**SKU Options:**
- **F1:** Free tier (limited resources)
- **B1:** Basic tier (good for development)
- **S1:** Standard tier (production workloads)
- **P1V2:** Premium tier (high performance)

</details>

**Verify App Service Plan creation:**
```bash
az appservice plan show \
    --name "asp-training-plan" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --output table
```

**✅ Success:** You should see the App Service Plan listed with "Ready" status.

## Task 2: Deploy Web Application (Portal)

**🌐 Azure Portal Deployment:** Create a web application using the Azure Portal.

**Step 1:** Navigate to App Services
- Search for "App Services" in the Azure Portal
- Click "Create" → "Web App"

**Step 2:** Basic Configuration
- **Resource Group:** [Your shared RG]
- **Name:** `webapp-training-[random-number]` (must be globally unique)
- **Publish:** Code
- **Runtime stack:** Node.js 18 LTS
- **Operating System:** Linux
- **Region:** South Central US

**Step 3:** App Service Plan
- **App Service Plan:** asp-training-plan (select existing)
- Click "Review + create"
- Click "Create"

**⏱️ Deployment Time:** Web app creation typically takes 2-3 minutes.

**Step 4:** Verify Deployment
- Once deployed, click "Go to resource"
- Click the URL shown in the overview page
- You should see a default "Your web app is running and waiting for your content" page

## Task 3: Configure Application Settings

**Step 1:** Navigate to Configuration
- In your Web App, click "Configuration" in the left menu under "Settings"
- Click the "Application settings" tab

**Step 2:** Add Application Settings
- Click "+ New application setting"
- **Name:** `ENVIRONMENT`
- **Value:** `development`
- Click "OK"

**Step 3:** Add Database Connection String
- Click "+ New application setting"
- **Name:** `DATABASE_URL`
- **Value:** `postgresql://localhost:5432/trainingdb`
- Click "OK"

**Step 4:** Save Configuration
- Click "Save" at the top
- Click "Continue" when prompted about restart

**📚 Application Settings vs Connection Strings:**
- **Application Settings:** General configuration values, environment variables
- **Connection Strings:** Database and external service connections
- Both are encrypted at rest and transmitted over encrypted channels

## Task 4: Integrate with Key Vault

**📚 Why Key Vault Integration?** Instead of storing sensitive values directly in application settings, reference them from Key Vault for enhanced security.

**Step 1:** Add Additional Secrets to Key Vault
First, add some application-specific secrets to your existing Key Vault:

```bash
# Add API key secret
az keyvault secret set \
    --vault-name "YOUR_KEYVAULT_NAME" \
    --name "api-key" \
    --value "sk-1234567890abcdef"

# Add database password secret
az keyvault secret set \
    --vault-name "YOUR_KEYVAULT_NAME" \
    --name "db-password" \
    --value "MySecureDbPassword123!"
```

**Step 2:** Verify Secrets Creation
```bash
az keyvault secret list \
    --vault-name "YOUR_KEYVAULT_NAME" \
    --output table
```

You should see your new secrets listed along with the existing `database-password`.

## Task 5: Enable System-Assigned Managed Identity

**Step 1:** Enable Managed Identity (Portal)
- In your Web App, click "Identity" in the left menu under "Settings"
- Click the "System assigned" tab
- Set **Status** to "On"
- Click "Save"
- Click "Yes" when prompted to confirm

**Step 2:** Note the Object ID
- After enabling, you'll see an **Object (principal) ID**
- Copy this ID - you'll need it for Key Vault access

**🔍 Research Challenge:** Find the Azure CLI command to enable system-assigned identity for a web app.

```bash
az webapp ________ assign \
    --____ "webapp-training-[your-number]" \
    --resource-_____ "YOUR_UNIQUE_RG_NAME"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az webapp identity assign \
    --name "webapp-training-[your-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME"
```

This command enables the system-assigned managed identity via CLI.

</details>

## Task 6: Configure Key Vault References

**Step 1:** Grant Web App Access to Key Vault
- In your Key Vault, go to "Access control (IAM)"
- Click "+ Add" → "Add role assignment"
- **Role:** "Key Vault Secrets User"
- Click "Next"
- **Assign access to:** Managed identity
- Click "+ Select members"
- **Managed identity:** App Service
- Select your web app from the list
- Click "Select" → "Review + assign" → "Assign"

**Step 2:** Configure Key Vault References in App Settings
- Return to your Web App's "Configuration" page
- Click "+ New application setting"
- **Name:** `API_KEY`
- **Value:** `@Microsoft.KeyVault(VaultName=YOUR_KEYVAULT_NAME;SecretName=api-key)`
- Click "OK"

**Step 3:** Add Database Password Reference
- Click "+ New application setting"
- **Name:** `DB_PASSWORD`
- **Value:** `@Microsoft.KeyVault(VaultName=YOUR_KEYVAULT_NAME;SecretName=db-password)`
- Click "OK"

**Step 4:** Save Configuration
- Click "Save"
- Click "Continue" when prompted

**📚 Key Vault Reference Format:**
```
@Microsoft.KeyVault(VaultName=<vault-name>;SecretName=<secret-name>)
```

## Task 7: Deploy Application Code

**Step 1:** Create Sample Application Code
Create a simple Node.js application to test the configuration:

```bash
# Create a temporary directory for your app
mkdir webapp-demo
cd webapp-demo

# Create package.json
cat > package.json << 'EOF'
{
  "name": "azure-training-webapp",
  "version": "1.0.0",
  "description": "Azure App Service Training Demo",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.0"
  }
}
EOF

# Create server.js
cat > server.js << 'EOF'
const express = require('express');
const app = express();
const port = process.env.PORT || 3000;

app.get('/', (req, res) => {
    const html = `
    <html>
    <head><title>Azure Training Web App</title></head>
    <body>
        <h1>🚀 Azure App Service Training Demo</h1>
        <h2>Environment Variables:</h2>
        <ul>
            <li><strong>ENVIRONMENT:</strong> ${process.env.ENVIRONMENT || 'Not set'}</li>
            <li><strong>DATABASE_URL:</strong> ${process.env.DATABASE_URL || 'Not set'}</li>
            <li><strong>API_KEY:</strong> ${process.env.API_KEY ? '***HIDDEN***' : 'Not set'}</li>
            <li><strong>DB_PASSWORD:</strong> ${process.env.DB_PASSWORD ? '***HIDDEN***' : 'Not set'}</li>
        </ul>
        <h2>System Information:</h2>
        <ul>
            <li><strong>Node.js Version:</strong> ${process.version}</li>
            <li><strong>Platform:</strong> ${process.platform}</li>
            <li><strong>Hostname:</strong> ${require('os').hostname()}</li>
        </ul>
        <p><em>Deployed at: ${new Date().toISOString()}</em></p>
    </body>
    </html>
    `;
    res.send(html);
});

app.get('/health', (req, res) => {
    res.json({ 
        status: 'healthy', 
        timestamp: new Date().toISOString(),
        environment: process.env.ENVIRONMENT 
    });
});

app.listen(port, () => {
    console.log(`Server running on port ${port}`);
    console.log(`Environment: ${process.env.ENVIRONMENT}`);
});
EOF
```

**Step 2:** Deploy Using Azure CLI

**🔍 Research Challenge:** Find the command to deploy code to an Azure Web App.

```bash
az webapp __ \
    --src-_____ . \
    --____ "webapp-training-[your-number]" \
    --resource-_____ "YOUR_UNIQUE_RG_NAME"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az webapp up \
    --src-path . \
    --name "webapp-training-[your-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME"
```

This command packages and deploys your application code to the web app.

**Alternative: ZIP Deployment**
```bash
# Create ZIP file
zip -r app.zip .

# Deploy ZIP file
az webapp deployment source config-zip \
    --src app.zip \
    --name "webapp-training-[your-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME"
```

</details>

## Task 8: Test Application and Key Vault Integration

**Step 1:** Access Your Web Application
- Go to your Web App in the Azure Portal
- Click the URL in the overview page
- You should see your custom application with environment variables displayed

**Step 2:** Verify Key Vault Integration
- Check that `API_KEY` and `DB_PASSWORD` show as "***HIDDEN***"
- This confirms the Key Vault references are working

**Step 3:** Test Health Endpoint
- Add `/health` to your web app URL
- You should see a JSON response with status and environment information

**Step 4:** Check Application Logs
```bash
# View live log stream
az webapp log tail \
    --name "webapp-training-[your-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME"
```

**✅ Success Criteria:**
- Web application loads successfully
- Environment variables are displayed correctly
- Key Vault references show as hidden
- Health endpoint returns JSON response

## Task 9: Configure Custom Domain (Optional)

**⚠️ Note:** This task requires a domain name you own. Skip if you don't have one available.

**Step 1:** Add Custom Domain (Portal)
- In your Web App, click "Custom domains" in the left menu
- Click "+ Add custom domain"
- Enter your domain name (e.g., `training.yourdomain.com`)
- Follow the verification steps provided

**Step 2:** Configure DNS
- Add the required DNS records to your domain provider
- Verify domain ownership

**📚 Custom Domain Benefits:**
- Professional appearance
- Brand consistency
- SSL certificate support
- Better SEO

## Task 10: Create Deployment Slots

**📚 What are Deployment Slots?** Deployment slots are separate environments within the same App Service that allow you to deploy different versions of your application.

**Step 1:** Create Staging Slot (Portal)
- In your Web App, click "Deployment slots" in the left menu
- Click "+ Add Slot"
- **Name:** `staging`
- **Clone settings from:** [Your production slot]
- Click "Add"

**Step 2:** Configure Staging Environment
- Click on the staging slot
- Go to "Configuration"
- Modify the `ENVIRONMENT` setting to `staging`
- Click "Save"

**Step 3:** Deploy to Staging Slot
Modify your application slightly for testing:

```bash
# Update server.js to show slot information
sed -i 's/Azure App Service Training Demo/Azure App Service Training Demo - STAGING/' server.js

# Deploy to staging slot
az webapp deployment source config-zip \
    --src app.zip \
    --name "webapp-training-[your-number]" \
    --slot "staging" \
    --resource-group "YOUR_UNIQUE_RG_NAME"
```

**Step 4:** Test Staging Slot
- Access the staging slot URL (shown in the portal)
- Verify it shows "STAGING" in the title

**Step 5:** Swap Slots
- In the "Deployment slots" page, click "Swap"
- **Source:** staging
- **Target:** production
- Click "Swap"

**📚 Slot Swap Benefits:**
- Zero-downtime deployments
- Easy rollback capability
- Warm-up before going live
- A/B testing capabilities

## Task 11: Configure VNet Integration

**📚 VNet Integration** allows your App Service to securely access resources in your virtual network, such as databases or internal APIs.

**Step 1:** Create App Service Subnet
First, create a dedicated subnet for App Service integration:

**🔍 Research Challenge:** Find the command to create a subnet for App Service VNet integration.

```bash
az network vnet subnet ______ \
    --____ "subnet-appservice" \
    --vnet-____ "vnet-training" \
    --resource-_____ "YOUR_UNIQUE_RG_NAME" \
    --address-______ "192.168.10.240/28"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az network vnet subnet create \
    --name "subnet-appservice" \
    --vnet-name "vnet-training" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --address-prefix "192.168.10.240/28"
```

**Subnet Requirements for App Service:**
- Minimum /28 subnet (16 addresses)
- Must be dedicated to App Service
- Cannot have other resources

</details>

**Step 2:** Configure VNet Integration (Portal)
- In your Web App, click "Networking" in the left menu
- Under "Outbound Traffic", click "VNet integration"
- Click "+ Add VNet"
- **Virtual Network:** vnet-training
- **Subnet:** subnet-appservice
- Click "OK"

**Step 3:** Test VNet Connectivity
Add a new endpoint to test internal connectivity:

```bash
# Add to server.js before app.listen()
cat >> server.js << 'EOF'

app.get('/network-test', async (req, res) => {
    const dns = require('dns').promises;
    try {
        // Test internal DNS resolution
        const addresses = await dns.lookup('google.com');
        res.json({
            status: 'VNet integration working',
            external_dns: addresses,
            timestamp: new Date().toISOString()
        });
    } catch (error) {
        res.status(500).json({
            status: 'VNet integration error',
            error: error.message
        });
    }
});
EOF

# Redeploy
zip -r app.zip .
az webapp deployment source config-zip \
    --src app.zip \
    --name "webapp-training-[your-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME"
```

## Task 12: Monitor Application Performance

**Step 1:** Enable Application Insights
- In your Web App, click "Application Insights" in the left menu
- Click "Turn on Application Insights"
- **Application Insights:** Create new
- **Name:** `ai-webapp-training`
- Click "Apply"

**Step 2:** View Application Metrics
- After a few minutes, return to Application Insights
- Explore the "Overview" dashboard
- Check "Live Metrics" for real-time data

**Step 3:** Configure Alerts
- Click "Alerts" in Application Insights
- Click "+ Create" → "Alert rule"
- Configure an alert for response time > 5 seconds
- Set up email notification

**📚 Application Insights Features:**
- **Performance monitoring:** Response times, throughput
- **Dependency tracking:** Database and external API calls
- **Exception tracking:** Automatic error detection
- **Custom telemetry:** Add your own metrics

## Task 13: Configure Auto-scaling

**Step 1:** Configure Scale-out Rules (Portal)
- In your App Service Plan, click "Scale out (App Service plan)" in the left menu
- Click "Custom autoscale"
- **Autoscale setting name:** `webapp-autoscale`

**Step 2:** Add Scale-out Rule
- Click "+ Add a rule"
- **Metric source:** Current resource
- **Metric:** CPU Percentage
- **Operator:** Greater than
- **Threshold:** 70
- **Duration:** 5 minutes
- **Action:** Increase count by 1
- Click "Add"

**Step 3:** Add Scale-in Rule
- Click "+ Add a rule"
- **Metric:** CPU Percentage
- **Operator:** Less than
- **Threshold:** 30
- **Duration:** 5 minutes
- **Action:** Decrease count by 1
- Click "Add"

**Step 4:** Set Instance Limits
- **Minimum:** 1
- **Maximum:** 3
- **Default:** 1
- Click "Save"

**📚 Auto-scaling Benefits:**
- **Cost optimization:** Scale down during low usage
- **Performance:** Scale up during high demand
- **Automatic:** No manual intervention required
- **Multiple metrics:** CPU, memory, HTTP queue length

## Task 14: Implement Continuous Deployment

**Step 1:** Set up GitHub Repository (Optional)
If you have a GitHub account, you can set up continuous deployment:

- Create a new repository on GitHub
- Push your application code to the repository

**Step 2:** Configure Deployment Center (Portal)
- In your Web App, click "Deployment Center" in the left menu
- **Source:** GitHub (or Local Git)
- Follow the authentication and repository selection steps
- Configure build and deployment settings

**Step 3:** Test Continuous Deployment
- Make a change to your application code
- Commit and push to your repository
- Watch the automatic deployment in the Deployment Center

**🔍 Research Challenge:** Find the command to configure local Git deployment.

```bash
az webapp deployment source ______ \
    --____ "webapp-training-[your-number]" \
    --resource-_____ "YOUR_UNIQUE_RG_NAME" \
    --repo-___ "LocalGit"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az webapp deployment source config-local-git \
    --name "webapp-training-[your-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME"
```

This sets up local Git deployment for your web app.

</details>

## Lesson 6 Verification

Verify your App Service configuration:

```bash
# Show web app details
az webapp show \
    --name "webapp-training-[your-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --output table

# List deployment slots
az webapp deployment slot list \
    --name "webapp-training-[your-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --output table

# Check app settings
az webapp config appsettings list \
    --name "webapp-training-[your-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --output table
```

**✅ Success Criteria:**
- ✓ App Service Plan created with appropriate SKU
- ✓ Web application deployed and accessible
- ✓ Application settings configured correctly
- ✓ Key Vault integration working with managed identity
- ✓ Custom application code deployed successfully
- ✓ Key Vault references showing as hidden values
- ✓ Deployment slots created and tested
- ✓ VNet integration configured
- ✓ Application Insights enabled for monitoring
- ✓ Auto-scaling rules configured
- ✓ Continuous deployment set up (optional)

**📚 Key Learnings:**
- App Services provide fully managed web application hosting
- App Service Plans define the compute resources and pricing tier
- Managed identities eliminate the need for stored credentials
- Key Vault references provide secure access to secrets
- Deployment slots enable zero-downtime deployments
- VNet integration allows secure access to internal resources
- Application Insights provides comprehensive monitoring
- Auto-scaling optimizes cost and performance automatically
- Multiple deployment options support different development workflows
- Configuration can be managed through Portal, CLI, and ARM templates

**Keep practicing and building!** The hands-on experience you've gained is invaluable for real-world Azure implementations.

---

**Previous:** [Lesson 5: Key Vault](lesson-5-key-vault.md)  
**Next:** [Quick Reference Guide](quick-reference-guide.md)
