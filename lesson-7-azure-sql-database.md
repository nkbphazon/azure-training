# Lesson 7: Azure SQL Database

## Learning Objectives
- Understand Azure SQL Database service tiers and deployment options
- Create and configure Azure SQL Database servers and databases
- Configure firewall rules and network access
- Connect to databases from Azure VMs and App Services
- Implement database security with Azure AD authentication
- Use connection strings and secure database credentials
- Monitor database performance and configure backups
- Implement geo-replication for disaster recovery

## Prerequisites
Complete previous lessons. You'll use the same resource group, virtual network, Key Vault, and App Service created earlier.

## Understanding Azure SQL Database

**📚 What is Azure SQL Database?**

Azure SQL Database is a fully managed platform-as-a-service (PaaS) database engine that handles most database management functions without user involvement. It's built on the latest stable version of Microsoft SQL Server.

**🌟 Key Benefits:**
- **Fully Managed:** Automatic patching, backups, and high availability
- **Scalable:** Dynamic scalability with no downtime
- **Secure:** Advanced threat protection and encryption
- **Intelligent:** Built-in intelligence for performance optimization
- **Cost-effective:** Pay for what you use with serverless options

**🏗️ Service Tiers:**
- **Basic:** Development and small applications
- **Standard:** Most production workloads
- **Premium:** High-performance requirements
- **Serverless:** Auto-scaling compute with billing per second

## Task 1: Create Azure SQL Server

**👤 Student Assignment:** One student should create the SQL Server for the team.

**⚠️ Important Note:** Azure SQL Server names must be **globally unique** across all Azure tenants.

**🔍 Research Challenge:** Find the correct parameters to create an Azure SQL Server.

```bash
az sql server ______ \
    --____ "sql-training-[random-number]" \
    --resource-_____ "YOUR_UNIQUE_RG_NAME" \
    --________ "southcentralus" \
    --admin-____ "sqladmin" \
    --admin-________ "TrainingPass123!"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az sql server create \
    --name "sql-training-[random-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --location "southcentralus" \
    --admin-user "sqladmin" \
    --admin-password "TrainingPass123!"
```

**Important:** Save the admin credentials securely. You'll need them to connect to the database.

</details>

**Verify SQL Server creation:**
```bash
az sql server show \
    --name "sql-training-[random-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --output table
```

## Task 2: Create Azure SQL Database

**🟢 Student A:** Create the database on the SQL Server.

**Step 1:** Create Database (Portal)
- Navigate to your SQL Server in the Azure Portal
- Click "Databases" in the left menu
- Click "+ Create database"

**Step 2:** Configure Database
- **Database name:** `db-training`
- **Want to use SQL elastic pool:** No
- **Compute + storage:** Click "Configure database"
  - **Service tier:** Basic
  - **Compute tier:** Provisioned
  - **Click "Apply"**

**Step 3:** Additional Settings
- Click "Additional settings" tab
- **Data source:** None (blank database)
- **Collation:** SQL_Latin1_General_CP1_CI_AS (default)

**Step 4:** Complete Creation
- Click "Review + create"
- Click "Create"
- Wait for deployment to complete (2-3 minutes)

**🔍 Research Challenge:** Find the CLI command to create a database.

```bash
az sql db ______ \
    --____ "db-training" \
    --server "sql-training-[random-number]" \
    --resource-_____ "YOUR_UNIQUE_RG_NAME" \
    --edition "_____" \
    --capacity _
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az sql db create \
    --name "db-training" \
    --server "sql-training-[random-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --edition "Basic" \
    --capacity 5
```

**Edition Options:**
- Basic: 5 DTUs
- Standard: S0-S12 (10-3000 DTUs)
- Premium: P1-P15 (125-4000 DTUs)

</details>

## Task 3: Configure Firewall Rules

**🟠 Student B:** Configure network access to the SQL Server.

**Step 1:** Add Client IP (Portal)
- In your SQL Server, click "Networking" in the left menu
- Under "Firewall rules", click "+ Add your client IPv4 address"
- This adds your current public IP

**Step 2:** Add Azure Services Access
- Toggle "Allow Azure services and resources to access this server" to **Yes**
- This allows Azure VMs and App Services to connect

**Step 3:** Add Virtual Network Rule
- Under "Virtual network rules", click "+ Add existing virtual network"
- **Virtual network:** vnet-training
- **Subnet:** subnet-appservice
- Click "OK"

**Step 4:** Save Configuration
- Click "Save" at the top of the page

**🔍 Research Challenge:** Find the CLI command to add a firewall rule.

```bash
az sql server firewall-rule ______ \
    --server "sql-training-[random-number]" \
    --resource-_____ "YOUR_UNIQUE_RG_NAME" \
    --____ "AllowVMs" \
    --start-__-_______ "192.168.10.0" \
    --end-__-_______ "192.168.10.255"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
az sql server firewall-rule create \
    --server "sql-training-[random-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --name "AllowVMs" \
    --start-ip-address "192.168.10.0" \
    --end-ip-address "192.168.10.255"
```

</details>

## Task 4: Store Database Credentials in Key Vault

**👥 Both Students:** Add database connection information to Key Vault.

**Step 1:** Add SQL Admin Password
```bash
az keyvault secret set \
    --vault-name "YOUR_KEYVAULT_NAME" \
    --name "sql-admin-password" \
    --value "TrainingPass123!"
```

**Step 2:** Create Connection String Secret
First, get your SQL Server FQDN:
```bash
az sql server show \
    --name "sql-training-[random-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --query "fullyQualifiedDomainName" \
    --output tsv
```

Now create the connection string:
```bash
# Replace [SQL-SERVER-FQDN] with your actual server FQDN
CONNECTION_STRING="Server=tcp:[SQL-SERVER-FQDN],1433;Initial Catalog=db-training;Persist Security Info=False;User ID=sqladmin;Password=TrainingPass123!;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"

az keyvault secret set \
    --vault-name "YOUR_KEYVAULT_NAME" \
    --name "sql-connection-string" \
    --value "$CONNECTION_STRING"
```

## Task 5: Connect from Virtual Machine

**💻 VM Serial Console:** Use the Serial Console of your VM to test database connectivity.

**Step 1:** Install SQL Server Command Line Tools
```bash
# Add Microsoft repository
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
curl https://packages.microsoft.com/config/ubuntu/24.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list

# Install tools
sudo apt update
sudo apt install -y mssql-tools unixodbc-dev

# Add to PATH
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
```

**Step 2:** Test Connection
```bash
# Get SQL Server FQDN (from your local machine)
az sql server show \
    --name "sql-training-[random-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --query "fullyQualifiedDomainName" \
    --output tsv
```

```bash
# Connect to database (on VM)
sqlcmd -S [SQL-SERVER-FQDN] -U sqladmin -P 'TrainingPass123!' -d db-training -Q "SELECT @@VERSION"
```

**Step 3:** Create a Test Table
```bash
sqlcmd -S [SQL-SERVER-FQDN] -U sqladmin -P 'TrainingPass123!' -d db-training << EOF
CREATE TABLE TestTable (
    Id INT PRIMARY KEY IDENTITY(1,1),
    Name NVARCHAR(100),
    CreatedDate DATETIME DEFAULT GETDATE()
);

INSERT INTO TestTable (Name) VALUES ('Test from VM');
SELECT * FROM TestTable;
GO
EOF
```

**✅ Success:** You should see the SQL Server version and your test data.

## Task 6: Configure App Service Database Connection

**Step 1:** Update App Service Configuration
- Go to your Web App in the Azure Portal
- Click "Configuration"
- Add new application setting:
  - **Name:** `SQL_CONNECTION_STRING`
  - **Value:** `@Microsoft.KeyVault(VaultName=YOUR_KEYVAULT_NAME;SecretName=sql-connection-string)`
- Click "Save"

**Step 2:** Update Application Code
Create an updated version of your application that uses the database:

```bash
cd webapp-demo

# Install SQL Server driver
cat >> package.json << 'EOF'
,
  "dependencies": {
    "express": "^4.18.0",
    "mssql": "^9.0.0"
  }
EOF

# Update server.js
cat > server.js << 'EOF'
const express = require('express');
const sql = require('mssql');
const app = express();
const port = process.env.PORT || 3000;

app.get('/', async (req, res) => {
    let dbStatus = 'Not connected';
    let recordCount = 0;
    
    if (process.env.SQL_CONNECTION_STRING) {
        try {
            await sql.connect(process.env.SQL_CONNECTION_STRING);
            const result = await sql.query`SELECT COUNT(*) as count FROM TestTable`;
            recordCount = result.recordset[0].count;
            dbStatus = 'Connected';
        } catch (err) {
            dbStatus = `Error: ${err.message}`;
        }
    }
    
    const html = `
    <html>
    <head><title>Azure Training Web App</title></head>
    <body>
        <h1>🚀 Azure App Service with SQL Database</h1>
        <h2>Database Status:</h2>
        <ul>
            <li><strong>Connection:</strong> ${dbStatus}</li>
            <li><strong>Test Table Records:</strong> ${recordCount}</li>
        </ul>
        <h2>Environment Variables:</h2>
        <ul>
            <li><strong>ENVIRONMENT:</strong> ${process.env.ENVIRONMENT || 'Not set'}</li>
            <li><strong>SQL_CONNECTION_STRING:</strong> ${process.env.SQL_CONNECTION_STRING ? '***HIDDEN***' : 'Not set'}</li>
        </ul>
        <p><em>Last updated: ${new Date().toISOString()}</em></p>
    </body>
    </html>
    `;
    res.send(html);
});

app.listen(port, () => {
    console.log(`Server running on port ${port}`);
});
EOF

# Deploy updated application
zip -r app.zip .
az webapp deployment source config-zip \
    --src app.zip \
    --name "webapp-training-[your-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME"
```

**Step 3:** Test Database Integration
- Access your web app URL
- Verify database connection status shows "Connected"
- Check that record count is displayed

## Task 7: Enable Azure AD Authentication

**📚 Why Azure AD Authentication?** Using Azure AD eliminates the need to store passwords and provides centralized identity management.

**Step 1:** Set Azure AD Admin (Portal)
- In your SQL Server, click "Azure Active Directory" in the left menu
- Click "Set admin"
- Search for and select your user account
- Click "Select"
- Click "Save"

**Step 2:** Test Azure AD Authentication
```bash
# Get access token (from your local machine)
az account get-access-token --resource https://database.windows.net/ --query accessToken -o tsv
```

Use this token to connect via Azure AD authentication in supported tools.

## Task 8: Configure Database Monitoring

**Step 1:** Enable Diagnostic Settings
- In your SQL Database, click "Diagnostic settings" under "Monitoring"
- Click "+ Add diagnostic setting"
- **Name:** `sql-diagnostics`
- **Logs:** Check all available options
- **Metrics:** Check all available options
- **Destination:** Send to Log Analytics workspace (create new if needed)
- Click "Save"

**Step 2:** View Database Metrics
- Click "Metrics" in your database
- **Metric:** DTU percentage
- **Aggregation:** Avg
- View the performance utilization

**Step 3:** Configure Alerts
- Click "Alerts" → "+ Create" → "Alert rule"
- **Signal:** DTU percentage
- **Threshold:** Greater than 80%
- Configure action group for notifications

## Task 9: Implement Geo-Replication

**🟢 Student A:** Configure database replication for disaster recovery.

**Step 1:** Configure Geo-Replication (Portal)
- In your SQL Database, click "Replicas" under "Data management"
- Click "+ Create replica"
- **Server:** Create new
  - **Server name:** `sql-training-dr-[random]`
  - **Location:** East US
  - **Authentication:** Use SQL authentication
  - **Admin login:** sqladmin
  - **Password:** TrainingPass123!
- Click "Review + create" → "Create"

**Step 2:** Monitor Replication
- Wait for deployment (5-10 minutes)
- View the replica in the Replicas page
- Check replication lag metrics

**🔍 Research Challenge:** Find the CLI command to create a database replica.

```bash
az sql db replica ______ \
    --____ "db-training" \
    --server "sql-training-[random-number]" \
    --resource-_____ "YOUR_UNIQUE_RG_NAME" \
    --partner-______ "sql-training-dr-[random]"
```

<details>
<summary>🔓 Click to reveal the complete command</summary>

```bash
# First create the DR server
az sql server create \
    --name "sql-training-dr-[random]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --location "eastus" \
    --admin-user "sqladmin" \
    --admin-password "TrainingPass123!"

# Then create the replica
az sql db replica create \
    --name "db-training" \
    --server "sql-training-[random-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --partner-server "sql-training-dr-[random]"
```

</details>

## Task 10: Implement Row-Level Security (Optional)

**📚 Advanced Security:** Row-level security allows you to control access to rows in a table based on user characteristics.

**Step 1:** Create Security Policy
Connect to your database and run:

```sql
-- Create schema for security
CREATE SCHEMA Security;
GO

-- Create users table
CREATE TABLE Security.Users (
    UserName NVARCHAR(50) PRIMARY KEY,
    Department NVARCHAR(50)
);

-- Insert sample users
INSERT INTO Security.Users VALUES 
    ('user1@company.com', 'Sales'),
    ('user2@company.com', 'Marketing');

-- Create sample data table
CREATE TABLE SalesData (
    Id INT IDENTITY(1,1) PRIMARY KEY,
    Department NVARCHAR(50),
    Amount DECIMAL(10,2)
);

-- Insert sample data
INSERT INTO SalesData (Department, Amount) VALUES 
    ('Sales', 1000),
    ('Marketing', 2000),
    ('Sales', 1500);

-- Create security predicate function
CREATE FUNCTION Security.fn_securitypredicate(@Department AS NVARCHAR(50))
    RETURNS TABLE
WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_securitypredicate_result
    WHERE @Department = USER_NAME() 
    OR EXISTS (SELECT 1 FROM Security.Users WHERE UserName = USER_NAME() AND Department = @Department);
GO

-- Create security policy
CREATE SECURITY POLICY DepartmentFilter
    ADD FILTER PREDICATE Security.fn_securitypredicate(Department) 
    ON dbo.SalesData
    WITH (STATE = ON);
GO
```

## Lesson 7 Verification

Verify your SQL Database configuration:

```bash
# Show SQL Server details
az sql server show \
    --name "sql-training-[random-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --output table

# List databases
az sql db list \
    --server "sql-training-[random-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --output table

# Show firewall rules
az sql server firewall-rule list \
    --server "sql-training-[random-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --output table

# Check replica status
az sql db replica list-links \
    --name "db-training" \
    --server "sql-training-[random-number]" \
    --resource-group "YOUR_UNIQUE_RG_NAME" \
    --output table
```

**✅ Success Criteria:**
- ✓ Azure SQL Server created with unique name
- ✓ SQL Database created with Basic tier
- ✓ Firewall rules configured for client and Azure services
- ✓ Virtual network rule added for secure connectivity
- ✓ Database credentials stored in Key Vault
- ✓ Successfully connected from VM using sqlcmd
- ✓ Test table created and data inserted
- ✓ App Service connected to database via Key Vault reference
- ✓ Azure AD authentication configured
- ✓ Database monitoring and alerts configured
- ✓ Geo-replication set up for disaster recovery
- ✓ Row-level security implemented (optional)

**📚 Key Learnings:**
- Azure SQL Database provides fully managed database services
- Multiple authentication methods available (SQL and Azure AD)
- Firewall rules and VNet integration provide network security
- Key Vault integration keeps connection strings secure
- Built-in monitoring and alerting for performance management
- Geo-replication enables disaster recovery scenarios
- Advanced security features like row-level security available
- Seamless integration with App Services and VMs
- Automatic backups and point-in-time restore included

---

**Previous:** [Lesson 6: Azure App Services](lesson-6-azure-app-services.md)  
**Next:** [Lesson 8: Azure Functions](lesson-8-azure-functions.md)