# Lesson 1: Azure CLI Fundamentals

## Learning Objectives
By the end of this lesson, you will:
- Understand what Azure CLI is and its purpose
- Install Azure CLI on Windows
- Authenticate with Azure
- Execute basic commands to list subscriptions
- Set the active subscription for future commands

## What is Azure CLI?

Azure CLI (Command Line Interface) is a cross-platform command-line tool that provides a consistent way to interact with Azure services. It allows you to manage Azure resources through commands instead of using the Azure portal's graphical interface.

**Benefits of Azure CLI:**
- Automation and scripting capabilities
- Consistent experience across platforms
- Faster than navigating through the portal
- Infrastructure as Code (IaC) support
- Perfect for DevOps workflows

## Task 1: Azure CLI Installation

**📖 Installation Documentation:** Please refer to the official Microsoft documentation for Azure CLI installation instructions:

- **Windows Installation:** [Install Azure CLI on Windows](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows)
- **macOS Installation:** [Install Azure CLI on macOS](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-macos)
- **Linux Installation:** [Install Azure CLI on Linux](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-linux)

**🔍 Quick Installation Options:**
- **Windows:** Use the MSI installer from the official documentation
- **macOS:** Use Homebrew: `brew install azure-cli`
- **Linux:** Use package managers or install script as documented

**✅ Verify Installation:**
After installation, verify Azure CLI is working by running:
```bash
az --version
```

> **Note:** You may need to restart your command prompt or terminal session after installation.

## Task 2: Authentication

Before using Azure CLI to manage resources, you need to authenticate with your Azure account.

### Interactive Login (Most Common)

```bash
az login
```

This command will:
1. Open your default web browser
2. Prompt you to sign in to Azure
3. Display your available subscriptions in the terminal after successful authentication

### Device Code Flow (Alternative)

If you can't open a browser from your current session:
```bash
az login --use-device-code
```

Follow the instructions to enter the device code on another device.

> **Success Indicator:** After authentication, you should see JSON output showing your available subscriptions.

## Task 3: List Subscriptions

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

## Task 4: Set Active Subscription

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

## Practice Commands

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

**Next:** [Lesson 2: Virtual Networks](lesson-2-virtual-networks.md)
