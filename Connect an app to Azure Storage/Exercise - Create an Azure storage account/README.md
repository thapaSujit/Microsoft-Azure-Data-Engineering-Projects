## Exercise - Create an Azure Storage Account

Now that we have an app, we need an Azure storage account to work with.

### Use the Azure CLI to Create an Azure Storage Account

We'll use the `az storage account create` command to create a new storage account. There are several parameters to control the storage account's configuration.

| Option         | Description                                                                                                                                                          |
|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `--name`        | A storage account name. The name is used to generate the public URL used to access the data in the account. It must be unique across all existing storage account names in Azure. It must be 3 to 24 characters long, and can contain only lowercase letters and numbers. |
| `--resource-group` | Use `learn-d5fa5a35-40fa-4506-a4c7-6f6e359dc240` to place the storage account into the free sandbox.                                                              |
| `--location`   | Select a location near you (see **Select a location**, following).                                                                                                   |
| `--sku`         | The storage account performance and replication model. Options include `Premium_LRS`, `Standard_GRS`, `Standard_LRS`, `Standard_RAGRS`, and `Standard_ZRS`. Use "Standard_LRS" for this example.                                       |

Use this table to craft a command line in Cloud Shell on the right to create the account. Use a unique name, e.g., "photostore" with your initials and a random number. You'll get an error if it's not unique.

Normally, you'd create a new resource group to hold your app resources, but in this case, use the provided sandbox resource group `learn-d5fa5a35-40fa-4506-a4c7-6f6e359dc240`.

Use "Standard_LRS" for the sku. This will use standard storage with local replication, which is fine for this example.

#### Select a Location

The free sandbox allows you to create resources in a subset of the Azure global regions. Select a region from this list when you create resources:

- westus2
- southcentralus
- centralus
- eastus
- westeurope
- southeastasia
- japaneast
- brazilsouth
- australiasoutheast
- centralindia

**Tip**: You can select a location with the `--location` parameter. If you don't supply one, the storage account will be created in the same location as your resource group. Because this is a more basic exercise, you can omit the parameter from the following command if you prefer.

#### Create a Storage Account

Use the following example command to create a storage account. Remember to replace `<name>` with your unique storage account name.

```bash
az storage account create \
  --resource-group learn-d5fa5a35-40fa-4506-a4c7-6f6e359dc240 \
  --location westus \
  --sku Standard_LRS \
  --name <name>
```
