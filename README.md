# Azure Cloud Hands-On Training Guide

> **Interactive learning for Azure cloud fundamentals with hands-on exercises**

## Training Overview

This comprehensive training program consists of six progressive lessons designed to build your Azure expertise through practical, hands-on exercises. Students will work individually for Lesson 1, then pair up for Lessons 2-6.

### Prerequisites
- Windows computer with internet access
- Azure subscription access (provided by instructor)
- Basic familiarity with command-line interfaces

### Training Structure
- **Duration:** 8 lessons (approximately 12-16 hours total)
- **Format:** Hands-on exercises with Azure CLI and Portal
- **Approach:** Progressive learning with real-world scenarios
- **Region:** All resources created in South Central US

---

## Lessons

### [Lesson 1: Azure CLI Fundamentals](lesson-1-azure-cli-fundamentals.md)
**Duration:** 1-2 hours | **Format:** Individual work

Learn the foundation of Azure automation and management.

**Learning Objectives:**
- Install and configure Azure CLI
- Authenticate with Azure subscriptions
- Execute basic Azure CLI commands
- Understand command structure and output formats

**Key Skills:**
- Azure CLI installation and setup
- Authentication methods
- Subscription management
- Basic command syntax

---

### [Lesson 2: Virtual Networks](lesson-2-virtual-networks.md)
**Duration:** 1-2 hours | **Format:** Partner work

Build secure, isolated network environments in Azure.

**Learning Objectives:**
- Create resource groups and virtual networks
- Configure network security groups (NSGs)
- Design and implement subnet architecture
- Understand Azure networking fundamentals

**Key Skills:**
- Resource group management
- Virtual network creation
- Subnet design and CIDR notation
- Network security group configuration

---

### [Lesson 3: Virtual Machines](lesson-3-virtual-machines.md)
**Duration:** 1-2 hours | **Format:** Partner work

Deploy and manage virtual machines with secure networking.

**Learning Objectives:**
- Deploy Ubuntu VMs using Azure Portal
- Access VMs through serial console
- Install and configure web services
- Test network connectivity and security

**Key Skills:**
- VM deployment and configuration
- Serial console access
- Web server installation (Nginx)
- Network connectivity testing
- Security rule implementation

---

### [Lesson 4: Azure Storage](lesson-4-azure-storage.md)
**Duration:** 2-3 hours | **Format:** Partner work

Master Azure's storage services for files and objects.

**Learning Objectives:**
- Create and configure storage accounts
- Implement Azure File Storage with SMB mounting
- Manage Azure Blob Storage containers
- Configure storage security and access controls
- Understand key rotation and SAS tokens

**Key Skills:**
- Storage account creation and configuration
- File share mounting on VMs
- Blob container management
- SAS token generation and management
- Storage key rotation strategies
- Network access restrictions

---

### [Lesson 5: Key Vault](lesson-5-key-vault.md)
**Duration:** 2-3 hours | **Format:** Partner work

Implement secure secret management with Azure Key Vault.

**Learning Objectives:**
- Understand control plane vs data plane permissions
- Create and configure Azure Key Vault
- Implement access policies and Azure RBAC
- Use managed identities for secure access
- Configure network restrictions

**Key Skills:**
- Key Vault creation and configuration
- Access policy vs RBAC models
- Secret management via CLI and Portal
- Managed identity implementation
- Network security configuration
- VM-to-Key Vault secure access

---

### [Lesson 6: Azure App Services](lesson-6-azure-app-services.md)
**Duration:** 2-3 hours | **Format:** Partner work

Deploy and manage web applications with Azure App Services.

**Learning Objectives:**
- Create App Service Plans and Web Apps
- Configure application settings and Key Vault integration
- Implement deployment slots for staging/production
- Configure VNet integration and monitoring
- Set up auto-scaling and continuous deployment

**Key Skills:**
- App Service Plan creation and management
- Web application deployment
- Key Vault reference configuration
- Deployment slot management
- VNet integration setup
- Application monitoring with Application Insights
- Auto-scaling configuration

---

### [Lesson 7: Azure SQL Database](lesson-7-azure-sql-database.md)
**Duration:** 2-3 hours | **Format:** Partner work

Implement fully managed relational database services.

**Learning Objectives:**
- Create and configure Azure SQL servers and databases
- Configure firewall rules and network access
- Connect from VMs and App Services using secure methods
- Implement Azure AD authentication
- Store connection strings in Key Vault
- Monitor database performance and configure alerts
- Implement geo-replication for disaster recovery

**Key Skills:**
- SQL Server and database creation
- Network security configuration
- Connection string management
- Azure AD integration
- Performance monitoring and tuning
- Geo-replication setup
- Row-level security implementation

---

### [Lesson 8: Azure Functions](lesson-8-azure-functions.md)
**Duration:** 2-3 hours | **Format:** Partner work

Build serverless event-driven applications.

**Learning Objectives:**
- Understand serverless computing concepts
- Create Function Apps with consumption plans
- Develop HTTP, Timer, and Blob-triggered functions
- Configure function bindings for inputs and outputs
- Integrate with Key Vault using managed identities
- Implement Durable Functions for workflows
- Monitor function performance and troubleshoot issues

**Key Skills:**
- Function App creation and configuration
- Multiple trigger type implementation
- Binding configuration
- Managed identity integration
- Durable Functions orchestration
- Application Insights monitoring
- CLI deployment techniques

---

## Additional Resources

### [Quick Reference Guide](quick-reference-guide.md)
Essential Azure CLI commands, best practices, and troubleshooting tips for quick reference during and after training.

**Includes:**
- Common Azure CLI commands
- Security best practices
- Troubleshooting commands
- Resource naming conventions
- Portal shortcuts

---

## Learning Path

### Recommended Sequence
1. **Start with Lesson 1** - Essential foundation for all subsequent lessons
2. **Progress through Lessons 2-6** - Each lesson builds on previous knowledge
3. **Use Quick Reference** - Keep handy throughout training
4. **Practice regularly** - Hands-on experience is key to retention

### Key Concepts Covered

**Infrastructure:**
- Resource groups and organization
- Virtual networks and subnets
- Network security groups
- Virtual machines

**Security:**
- Azure RBAC and access policies
- Managed identities
- Key Vault secret management
- Network access restrictions

**Storage:**
- Storage accounts and replication
- File shares and blob containers
- Access keys and SAS tokens
- Lifecycle management

**Applications:**
- App Service Plans and Web Apps
- Application configuration
- Deployment strategies
- Monitoring and scaling

**Databases:**
- Azure SQL Database
- Connection security
- Geo-replication
- Performance optimization

**Serverless:**
- Azure Functions
- Event-driven architecture
- Consumption-based billing
- Durable workflows

### Best Practices Learned

Throughout this training, you'll learn and implement Azure best practices:

- **Security:** Principle of least privilege, managed identities, network restrictions
- **Organization:** Consistent naming conventions, resource grouping
- **Automation:** Azure CLI usage, Infrastructure as Code concepts
- **Monitoring:** Application Insights, metrics and alerting
- **Cost Management:** Appropriate SKU selection, auto-scaling, lifecycle policies
- **Database Security:** Azure AD authentication, firewall rules, encryption
- **Serverless Architecture:** Event-driven design, consumption billing, automatic scaling

---

## Getting Started

1. **Ensure Prerequisites:** Verify you have the required access and tools
2. **Begin with Lesson 1:** [Azure CLI Fundamentals](lesson-1-azure-cli-fundamentals.md)
3. **Find a Partner:** For Lessons 2-6, pair up with another student
4. **Keep Reference Handy:** Bookmark the [Quick Reference Guide](quick-reference-guide.md)

## Support

- **Instructor:** Available for questions and guidance throughout training
- **Documentation:** Each lesson includes links to official Azure documentation
- **Troubleshooting:** Quick Reference Guide includes common solutions

---

**Ready to start your Azure journey?** Begin with [Lesson 1: Azure CLI Fundamentals](lesson-1-azure-cli-fundamentals.md)

---

*This training guide provides hands-on experience with core Azure services. The practical exercises and real-world scenarios will give you the confidence to implement Azure solutions in production environments.*
