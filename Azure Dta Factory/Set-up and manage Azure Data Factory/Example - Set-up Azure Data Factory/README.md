# Example - Set-up Azure Data Factory

Setting up Azure Data Factory is a straightforward process within the Azure portal. To get started, you'll need the following information:

## Required Information:

1. **Name:** The name of the Azure Data Factory instance.

2. **Subscription:** The subscription in which the Azure Data Factory instance is created.

3. **Resource Group:** The resource group where the Azure Data Factory instance will reside.

4. **Version:** Select V2 for the latest features.

5. **Location:** The datacenter location in which the Azure Data Factory instance is stored.

## Git Integration:

Enabling Git provides the capability to integrate the code you create with a Git repository, allowing you to source control the code.

### Configuration Details:

- **Git URL:** [Provide Git URL]
  
- **Repository Name:** [Your Repository Name]

- **Branch Name:** [Your Branch Name]

- **Root Folder:** [Your Root Folder]

## Getting Started:

1. Open the [Azure portal](https://portal.azure.com/).

2. Navigate to Azure Data Factory.

3. Enter the required information mentioned above.

4. Optionally, enable Git and provide the Git configuration details.

5. Save your configuration.

6. Your Azure Data Factory instance is now set up and ready to use!

# Azure Data Factory Setup

Setting up Azure Data Factory can also be done programmatically. In this example, we'll use PowerShell to create an Azure Data Factory instance.

## Prerequisites:

Before running the script, make sure you have the following:

- Azure subscription ID
- Azure PowerShell module installed

## PowerShell Script:

### PART I: Creating an Azure Data Factory

```powershell
######################################################################
##                PART I: Creating an Azure Data Factory            ##
######################################################################

# Sign in to Azure and set the WINDOWS AZURE subscription to work with
$SubscriptionId = "add your subscription in the quotes"

Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId

# Register the Microsoft Azure Data Factory resource provider
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory

# Define resource group name and location parameters
$resourceGroupName = "cto_ignite"
$rglocation = "West US 2"

# Create Azure Data Factory
New-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Name "ctoigniteADF" -Location $rglocation
```
