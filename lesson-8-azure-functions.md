# Lesson 8: Azure Functions

## Learning Objectives
- Understand serverless computing and Azure Functions concepts
- Create Function Apps with different hosting plans
- Develop and deploy HTTP-triggered functions
- Implement timer-triggered functions for scheduled tasks
- Use blob storage triggers for event-driven processing
- Integrate Functions with Key Vault and other Azure services
- Configure function bindings for input and output
- Monitor function execution and performance
- Implement durable functions for complex workflows

## Prerequisites
Complete previous lessons. You'll use the same resource group, storage account, Key Vault, and virtual network created earlier.

## Understanding Azure Functions

**📚 What are Azure Functions?**

Azure Functions is a serverless compute service that enables you to run event-triggered code without explicitly provisioning or managing infrastructure. You pay only for the time your code runs.

**🌟 Key Benefits:**
- **Serverless:** No infrastructure management
- **Event-driven:** Triggered by various Azure services
- **Auto-scaling:** Automatically scales based on demand
- **Pay-per-use:** Charged only for execution time
- **Multiple languages:** C#, JavaScript, Python, Java, PowerShell

**🏗️ Trigger Types:**
- **HTTP:** REST APIs and webhooks
- **Timer:** Scheduled tasks (cron expressions)
- **Blob Storage:** File processing
- **Queue Storage:** Message processing
- **Event Hub/Grid:** Event streaming
- **Service Bus:** Enterprise messaging

## Task 1: Create Function App

**👤 Student Assignment:** One student should create the Function App for the team.

**Step 1:** Create Function App (Portal)
- Search for "Function App" in the Azure Portal
- Click "Create"

**Step 2:** Basic Configuration
- **Resource Group:** [Your shared RG]
- **Function App name:** `func-training-[random-number]` (must be globally unique)
- **Publish:** Code
- **Runtime stack:** Node.js
- **Version:** 18 LTS
- **Region:** South Central US
- **Operating System:** Linux
- **Plan type:** Consumption (Serverless)

**Step 3:** Storage Configuration
- Click "Next: Storage"
- **Storage account:** Select your existing storage account from Lesson 4

**Step 4:** Monitoring
- Click "Next: Monitoring"
- **Enable Application Insights:** Yes
- **Application Insights:** Select existing (from Lesson 6) or create new

**Step 5:** Complete Creation
- Click "Review + create"
- Click "Create"
- Wait for deployment to complete (2-3 minutes)

**🔍 Research Challenge:** Find the CLI command to create a Function App.

```bash
az functionapp ______ \
    --____ "func-training-[random-number]" \
    --resource-_____ "YOUR_UNIQUE_RG_NAME" \
    --consumption-____-________ "southcentralus" \
    --runtime ____ \
    --runtime-_______ "18" \
    --functions-_______ "4" \
    --storage-_______ "strtraining[your-numbers]"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az functionapp create \
    --name "func-training-[random-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --consumption-plan-location "southcentralus" \
    --runtime node \
    --runtime-version "18" \
    --functions-version "4" \
    --storage-account "strtraining[your-numbers]"
```

**Runtime Options:**
- `node` - Node.js/JavaScript
- `python` - Python
- `dotnet` - C#/.NET
- `java` - Java

</details>

## Task 2: Create HTTP-Triggered Function

**🟢 Student A:** Create an HTTP-triggered function for API endpoints.

**Step 1:** Navigate to Functions
- Go to your Function App in the portal
- Click "Functions" in the left menu
- Click "+ Create"

**Step 2:** Configure HTTP Trigger
- **Development environment:** Develop in portal
- **Template:** HTTP trigger
- **New Function:** `HttpGreeting`
- **Authorization level:** Anonymous

**Step 3:** Implement Function Code
Click on your new function and replace the code:

```javascript
module.exports = async function (context, req) {
    context.log('HTTP trigger function processed a request.');

    const name = (req.query.name || (req.body && req.body.name));
    const timestamp = new Date().toISOString();
    
    const responseMessage = name
        ? `Hello, ${name}! Welcome to Azure Functions. Current time: ${timestamp}`
        : "Hello! Please pass a name in the query string or request body.";

    context.res = {
        status: 200,
        headers: {
            "Content-Type": "application/json"
        },
        body: {
            message: responseMessage,
            timestamp: timestamp,
            functionName: context.executionContext.functionName,
            invocationId: context.executionContext.invocationId
        }
    };
};
```

**Step 4:** Test Function
- Click "Test/Run" in the function editor
- Add query parameter: `name=YourName`
- Click "Run"
- View the output in the logs

**Step 5:** Get Function URL
- Click "Get Function URL"
- Copy the URL
- Test in browser: `https://func-training-[number].azurewebsites.net/api/HttpGreeting?name=Student`

## Task 3: Create Timer-Triggered Function

**🟠 Student B:** Create a timer-triggered function for scheduled tasks.

**Step 1:** Create Timer Function
- In Functions, click "+ Create"
- **Template:** Timer trigger
- **New Function:** `ScheduledCleanup`
- **Schedule:** `0 */5 * * * *` (every 5 minutes)

**Step 2:** Implement Timer Function
Replace the function code:

```javascript
module.exports = async function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    if (myTimer.isPastDue) {
        context.log('Timer function is running late!');
    }
    
    context.log('Timer trigger function started at:', timeStamp);
    
    // Simulate cleanup task
    const tasksToClean = ['temp-files', 'old-logs', 'expired-cache'];
    
    for (const task of tasksToClean) {
        context.log(`Cleaning: ${task}`);
        // Add actual cleanup logic here
    }
    
    context.log('Timer trigger function completed at:', new Date().toISOString());
};
```

**📚 CRON Expression Format:**
```
{second} {minute} {hour} {day} {month} {day-of-week}

Examples:
- 0 */5 * * * * : Every 5 minutes
- 0 0 * * * * : Every hour
- 0 0 9 * * * : Daily at 9 AM
- 0 0 0 * * 1 : Weekly on Monday
```

**Step 3:** Monitor Execution
- Click "Monitor" in the function
- View execution history
- Check logs for scheduled runs

## Task 4: Enable Managed Identity and Key Vault Access

**Step 1:** Enable System-Assigned Identity
- In your Function App, click "Identity" under "Settings"
- Set **Status** to "On"
- Click "Save"
- Note the Object ID

**Step 2:** Grant Key Vault Access
- Go to your Key Vault
- Click "Access control (IAM)"
- Add role assignment: "Key Vault Secrets User"
- Assign to your Function App's managed identity

**Step 3:** Add Key Vault Reference
- In Function App, go to "Configuration"
- Add application setting:
  - **Name:** `SECRET_VALUE`
  - **Value:** `@Microsoft.KeyVault(VaultName=YOUR_KEYVAULT_NAME;SecretName=api-key)`
- Click "Save"

## Task 5: Create Blob-Triggered Function

**👥 Both Students:** Work together to create a blob-triggered function for file processing.

**Step 1:** Create Blob Container
```bash
az storage container create \
    --name "function-uploads" \
    --account-name "strtraining[your-numbers]" \
    --account-key "[KEY-FROM-LESSON-4]"
```

**Step 2:** Create Blob Trigger Function
- In Functions, click "+ Create"
- **Template:** Azure Blob Storage trigger
- **New Function:** `ProcessUploadedFile`
- **Path:** `function-uploads/{name}`
- **Storage account connection:** Select your storage account

**Step 3:** Implement Blob Processing
Replace the function code:

```javascript
module.exports = async function (context, myBlob) {
    context.log('Blob trigger function processed blob:');
    context.log('- Name:', context.bindingData.name);
    context.log('- Size:', myBlob.length, 'bytes');
    context.log('- Type:', context.bindingData.properties.contentType);
    
    // Convert blob to string (if text file)
    const blobContent = myBlob.toString();
    
    // Process based on file type
    if (context.bindingData.name.endsWith('.json')) {
        try {
            const jsonData = JSON.parse(blobContent);
            context.log('JSON data:', jsonData);
            
            // Example: Store processed result
            context.bindings.outputBlob = {
                content: JSON.stringify({
                    originalFile: context.bindingData.name,
                    processedAt: new Date().toISOString(),
                    recordCount: Array.isArray(jsonData) ? jsonData.length : 1
                }),
                fileName: `processed/${context.bindingData.name}`
            };
        } catch (error) {
            context.log.error('Error parsing JSON:', error);
        }
    } else if (context.bindingData.name.endsWith('.txt')) {
        context.log('Text content preview:', blobContent.substring(0, 100));
        
        // Example: Count words
        const wordCount = blobContent.split(/\s+/).length;
        context.bindings.outputBlob = {
            content: `File: ${context.bindingData.name}\nWord count: ${wordCount}\nProcessed: ${new Date().toISOString()}`,
            fileName: `processed/${context.bindingData.name}.stats`
        };
    }
    
    context.log('Blob processing completed');
};
```

**Step 4:** Add Output Binding
- In the function, click "Integration"
- Click "+ Add output"
- **Binding type:** Azure Blob Storage
- **Blob parameter name:** `outputBlob`
- **Path:** `function-processed/{fileName}`
- **Storage account connection:** Use existing

**Step 5:** Test Blob Trigger
Upload a test file:

```bash
# Create test JSON file
echo '{"name":"Test","value":123}' > test.json

# Upload to trigger function
az storage blob upload \
    --container-name "function-uploads" \
    --name "test.json" \
    --file "test.json" \
    --account-name "strtraining[your-numbers]" \
    --account-key "[KEY-FROM-LESSON-4]"
```

Check function logs to see execution and verify processed file in storage.

## Task 6: Create Function with Multiple Bindings

**🟢 Student A:** Create a function that reads from queue and writes to blob and database.

**Step 1:** Create Queue for Messages
```bash
az storage queue create \
    --name "processing-queue" \
    --account-name "strtraining[your-numbers]" \
    --account-key "[KEY-FROM-LESSON-4]"
```

**Step 2:** Create Queue-Triggered Function
- Create new function with Queue trigger template
- **Name:** `ProcessQueueMessage`
- **Queue name:** `processing-queue`

**Step 3:** Implement Multi-Output Function
```javascript
module.exports = async function (context, queueItem) {
    context.log('Queue trigger function processing:', queueItem);
    
    try {
        // Parse queue message
        const data = typeof queueItem === 'string' ? JSON.parse(queueItem) : queueItem;
        
        // Process data
        const processedData = {
            id: context.bindingData.id,
            originalMessage: data,
            processedAt: new Date().toISOString(),
            status: 'completed'
        };
        
        // Output to blob storage
        context.bindings.outputBlob = JSON.stringify(processedData, null, 2);
        
        // Output to another queue
        context.bindings.outputQueue = {
            id: processedData.id,
            status: processedData.status,
            timestamp: processedData.processedAt
        };
        
        // Log to table storage
        context.bindings.outputTable = {
            PartitionKey: 'processed',
            RowKey: processedData.id,
            data: JSON.stringify(processedData),
            timestamp: processedData.processedAt
        };
        
        context.log('Message processed successfully');
        
    } catch (error) {
        context.log.error('Error processing message:', error);
        throw error;
    }
};
```

**Step 4:** Configure Output Bindings
Add these output bindings in Integration:

1. **Blob Output:**
   - Path: `processed-messages/{id}.json`
   - Parameter name: `outputBlob`

2. **Queue Output:**
   - Queue name: `completed-queue`
   - Parameter name: `outputQueue`

3. **Table Output:**
   - Table name: `ProcessingLog`
   - Parameter name: `outputTable`

## Task 7: Implement Durable Functions

**📚 Durable Functions** enable you to write stateful functions in a serverless environment, perfect for long-running workflows.

**Step 1:** Install Durable Functions Extension
This is typically pre-installed in the portal, but verify in your Function App's "Extensions" section.

**Step 2:** Create Orchestrator Function
Create a new function:
- **Template:** Durable Functions HTTP starter
- **Name:** `StartWorkflow`

**Step 3:** Implement Orchestrator
```javascript
const df = require("durable-functions");

module.exports = async function (context, req) {
    const client = df.getClient(context);
    
    const input = {
        name: req.query.name || req.body?.name || "Guest",
        tasks: ["validate", "process", "notify"]
    };
    
    const instanceId = await client.startNew("WorkflowOrchestrator", undefined, input);
    
    context.log(`Started orchestration with ID = '${instanceId}'.`);
    
    return client.createCheckStatusResponse(context.bindingData.req, instanceId);
};
```

**Step 4:** Create Orchestrator Logic
Create another function:
- **Template:** Durable Functions orchestrator
- **Name:** `WorkflowOrchestrator`

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function* (context) {
    const input = context.df.getInput();
    const outputs = [];
    
    // Run activities in sequence
    outputs.push(yield context.df.callActivity("ValidateInput", input));
    outputs.push(yield context.df.callActivity("ProcessData", input));
    outputs.push(yield context.df.callActivity("SendNotification", input));
    
    return outputs;
});
```

**Step 5:** Create Activity Functions
Create activity functions for each step:

**ValidateInput:**
```javascript
module.exports = async function (context, input) {
    context.log('Validating input:', input);
    // Validation logic
    return { status: "validated", name: input.name };
};
```

## Task 8: Configure Function Networking

**🟠 Student B:** Configure VNet integration for secure connectivity.

**Step 1:** Configure VNet Integration
- In Function App, click "Networking"
- Under "Outbound Traffic", click "VNet integration"
- Click "Add VNet"
- Select your existing VNet and subnet

**Step 2:** Test Internal Connectivity
Create a function to test VNet access:

```javascript
module.exports = async function (context, req) {
    const dns = require('dns').promises;
    
    try {
        // Test internal DNS resolution
        const internalHost = 'your-vm-private-ip';
        const result = await dns.lookup(internalHost);
        
        context.res = {
            body: {
                message: "VNet integration successful",
                internalAccess: true,
                details: result
            }
        };
    } catch (error) {
        context.res = {
            status: 500,
            body: {
                message: "VNet integration test failed",
                error: error.message
            }
        };
    }
};
```

## Task 9: Monitor and Troubleshoot Functions

**Step 1:** View Live Metrics
- In Function App, click "Application Insights"
- Click "Live Metrics"
- Trigger some functions and watch real-time data

**Step 2:** Query Function Logs
- Go to Application Insights
- Click "Logs"
- Run query:
```kusto
traces
| where cloud_RoleName == "func-training-[number]"
| where timestamp > ago(1h)
| order by timestamp desc
| take 100
```

**Step 3:** Set Up Alerts
- Click "Alerts" → "Create alert rule"
- Configure alert for function failures
- Set up email notifications

**🔍 Research Challenge:** Find the CLI command to view function logs.

```bash
az functionapp ___ ____-______ \
    --____ "func-training-[random-number]" \
    --resource-_____ "YOUR_UNIQUE_RG_NAME"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az functionapp log show-stream \
    --name "func-training-[random-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME"
```

This shows live streaming logs from your Function App.

</details>

## Task 10: Deploy Functions via CLI

**Step 1:** Create Local Function Project
```bash
# Install Azure Functions Core Tools (if not installed)
# npm install -g azure-functions-core-tools@4

# Create new function project
mkdir my-functions
cd my-functions
func init --javascript

# Create new HTTP function
func new --name HelloCLI --template "HTTP trigger"
```

**Step 2:** Deploy to Azure
```bash
# Deploy function project
func azure functionapp publish "func-training-[random-number]"
```

## Lesson 8 Verification

Verify your Functions configuration:

```bash
# Show Function App details
az functionapp show \
    --name "func-training-[random-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --output table

# List functions
az functionapp function list \
    --name "func-training-[random-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --output table

# Get function keys
az functionapp function keys list \
    --name "func-training-[random-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --function-name "HttpGreeting"
```

**✅ Success Criteria:**
- ✓ Function App created with consumption plan
- ✓ HTTP-triggered function deployed and tested
- ✓ Timer-triggered function running on schedule
- ✓ Blob-triggered function processing uploads
- ✓ Managed identity enabled and Key Vault integrated
- ✓ Multiple bindings configured for complex scenarios
- ✓ Durable Functions orchestrating workflows
- ✓ VNet integration configured for secure access
- ✓ Application Insights monitoring enabled
- ✓ Functions deployed via portal and CLI

**📚 Key Learnings:**
- Azure Functions provide serverless event-driven compute
- Multiple trigger types enable various integration scenarios
- Bindings simplify input/output operations
- Consumption plan charges only for execution time
- Managed identities provide secure authentication
- Durable Functions enable complex stateful workflows
- VNet integration allows secure network access
- Application Insights provides comprehensive monitoring
- Functions can be developed in portal or deployed via CLI
- Serverless architecture scales automatically based on demand

---

**Previous:** [Lesson 7: Azure SQL Database](lesson-7-azure-sql-database.md)  
**Next:** [Lesson 9: Azure Container Instances](lesson-9-azure-container-instances.md)