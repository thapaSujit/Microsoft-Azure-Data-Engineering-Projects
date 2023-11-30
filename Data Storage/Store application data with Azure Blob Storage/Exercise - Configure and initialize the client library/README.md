## Exercise - Configure and Initialize the Client Library

The typical workflow for apps that use Azure Blob storage is as follows:

1. **Retrieve Configuration:** At startup, load the storage account configuration, typically a storage account connection string.

2. **Initialize Client:** To initialize the Azure Storage client library, use the connection string. This initialization creates the objects that the app uses to work with the Blob storage API.

3. **Use:** To operate on containers and blobs, make API calls by using the client library.

### Configure Your Connection String

Before you run your app, get the connection string for the storage account you use. You can use any Azure management interface to get it, including the Azure portal, the Azure CLI, and Azure PowerShell. When you set up the web app to run your code near the end of this module, use the Azure CLI to get the connection string for the storage account that you created earlier.

Storage account connection strings include the account key. Consider the account key a secret. Store it securely. Here, you store the connection string in an App Service app setting. App Service app settings are a secure place for app secrets. This design doesn't support local development and isn't a robust, end-to-end solution on its own.

### Initialize the Blob Storage Object Model

In the Azure Storage SDK for .NET, the standard pattern for using Blob storage is as follows:

1. Instantiate a new `BlobServiceClient` object and provide the connection string to your storage account.

2. To get a `BlobContainerClient`, call `GetBlobContainerClient` on the `BlobServiceClient` with the name of the container you want to interact with or create.

In code, these steps look like this:
```csharp
// Instantiate a new BlobServiceClient
BlobServiceClient blobServiceClient = new BlobServiceClient(connectionString);

// Get a BlobContainerClient
BlobContainerClient blobContainerClient = blobServiceClient.GetBlobContainerClient("your-container-name");
```

In code, these steps look like this.
```csharp
BlobServiceClient blobServiceClient = new BlobServiceClient(storageConfig.ConnectionString);
BlobContainerClient containerClient = blobServiceClient.GetBlobContainerClient(storageConfig.FileContainerName);
```

None of this initialization code makes calls over the network. This fact means that some exceptions that occur because of incorrect information aren't thrown until later. For example, if an incorrectly formatted connection string is supplied to the constructor of the BlobServiceClient class, an exception is thrown immediately. However, if the connection string points to a storage account that doesn't exist, no exception is thrown until you attempt an operation against the storage account.

## Create Containers at Startup

To create a container when your app starts or when the app first tries to use a container, call `CreateIfNotExistsAsync` on a `BlobContainerClient`.

`CreateIfNotExistsAsync` doesn't throw an exception if the container already exists, but it does make a network call to Azure Blob Storage. Call it once during initialization, not every time you try to use a container.

### Exercise

#### Clone and Explore the Unfinished App

1. First, clone the starter app from GitHub. To get a copy of the source code and open it in the editor, run the following commands in Azure Shell CLI:

```bash
git clone https://github.com/MicrosoftDocs/mslearn-store-data-in-azure.git
cd mslearn-store-data-in-azure/store-app-data-with-azure-blob-storage/src/start
code .
```

2. In the editor, open the file `Controllers/FilesController.cs`. There's no work to do here, but have a quick look at what the app does.

This controller implements an API with three actions:

- **Index:** (GET /api/Files) returns a list of URLs, one for each file that's been uploaded. The app front end calls this method to build a list of hyperlinks to the uploaded files.

- **Upload:** (POST /api/Files) receives an uploaded file and saves it.

- **Download:** (GET /api/Files/{filename}) downloads an individual file by its name.

To do its work, each method uses an `IStorage` instance called `storage`. There's an incomplete implementation of `IStorage` in `Models/BlobStorage.cs` to fill in.

#### Add the NuGet package
Add a reference to the Azure Storage SDK. Run the following commands in Azure Shell CLI:

```bash
dotnet add package Azure.Storage.Blobs
dotnet restore
```
This command ensures you're using the newest version of the Blob Storage client library.

#### Configure
The configuration values you need are the storage-account connection string and the name of the container the app uses to store files. In this module, you're only going to run the app in Azure App Service. Follow App Service best practice and store the values in App Service app settings. You do that when you create the App Service instance. There's nothing you need to do at the moment.

When it comes to using the configuration, the starter app includes the plumbing you need. The IOptions<AzureStorageConfig> constructor parameter in BlobStorage has two properties: the storage-account connection string and the name of the container your app uses to store blobs. There's code in the ConfigureServices method of Startup.cs that loads the values from configuration when the app starts.

#### Initialize
In the editor, open Models/BlobStorage.cs. At the top of the file, add the following using statements to prepare it for the code that you're going to add.

```bash
using Azure;
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
```

Locate the Initialize method. Your app calls this method when it uses BlobStorage for the first time. If you're curious, you can look at ConfigureServices in Startup.cs to see how call is done.

Initialize is where you want to create your container if it doesn't already exist. Replace the current implementation of Initialize with the following code, and save your work using CTRL+S.

```bash
public Task Initialize()
{
    BlobServiceClient blobServiceClient = new BlobServiceClient(storageConfig.ConnectionString);
    BlobContainerClient containerClient = blobServiceClient.GetBlobContainerClient(storageConfig.FileContainerName);
    return containerClient.CreateIfNotExistsAsync();
}
```
