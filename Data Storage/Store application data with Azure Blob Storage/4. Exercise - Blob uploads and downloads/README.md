## Exercise - Blob Uploads and Downloads

To interact with individual blobs in Blob Storage, use a `BlobClient` object. You can get a `BlobClient` by requesting it with the blob's name from the `BlobContainerClient` in which the blob is located. `BlobClient` has methods to upload, download, and manage individual blobs in Blob Storage.

### Getting a `BlobClient` Object

To get a `BlobClient` by name, call the `GetBlobClient` methods on the `BlobContainerClient` that contains the blob, using the name of the blob. A `BlobClient` object allows you to interact with the blob by uploading, downloading, or managing the blob in Blob Storage.

Moving data to and from a blob is a network operation that takes time. The Azure Storage SDK for .NET provides asynchronous implementations of all methods that require network activity. We recommend using these async implementations whenever possible in your application.

We recommend using streams instead of in-memory structures like byte arrays or strings when you're working with large data objects. This approach avoids buffering the full content in memory before sending it to the target. ASP.NET Core supports reading and writing streams from requests and responses.

### Create New Blobs

To create a new blob, call one of the `Upload` methods on a reference to a blob that doesn't exist in storage. This approach does two things: creates the blob in storage and uploads the data.

```bash
BlobClient blobClient = containerClient.GetBlobClient(name);
var response = await blobClient.UploadAsync(fileStream);
```

## Exercise

Finish your app by adding the upload and download code, then deploy it to Azure App Service for testing.

### Upload

To upload a blob, you implement the `BlobStorage.Save` method. First, you get a `BlobClient` object that represents the blob by calling `GetBlobClient` on a `BlobContainerClient`. Then, use the `UploadAsync` method on the `BlobClient` to save the `Stream` of data passed to this method up to Blob Storage.

In the editor, in `BlobStorage.cs`, replace `Save` with the following code. Use `CTRL+S` to save your work.

```csharp
public Task Save(Stream fileStream, string name)
{
    BlobServiceClient blobServiceClient = new BlobServiceClient(storageConfig.ConnectionString);

    // Get the container (folder) the file will be saved in
    BlobContainerClient containerClient = blobServiceClient.GetBlobContainerClient(storageConfig.FileContainerName);

    // Get the Blob Client used to interact with (including create) the blob
    BlobClient blobClient = containerClient.GetBlobClient(name);

    // Upload the blob
    return blobClient.UploadAsync(fileStream);
}
```
### Download

To download a file, the `OpenReadAsync` method on the `BlobClient` object is used. This method returns a `Stream`, which means that your code doesn't need to load all of the bytes from Blob Storage at once. You just need to return a reference to the blob stream, which ASP.NET Core can use to stream the file to the browser.

Replace `Load` with this code and save your work using `CTRL + S`.

```csharp
public Task<Stream> Load(string name)
{
    BlobServiceClient blobServiceClient = new BlobServiceClient(storageConfig.ConnectionString);

    // Get the container the blobs are saved in
    BlobContainerClient containerClient = blobServiceClient.GetBlobContainerClient(storageConfig.FileContainerName);

    // Get a client to operate on the blob so we can read it.
    BlobClient blobClient = containerClient.GetBlobClient(name);

    return blobClient.OpenReadAsync();
}
```

## Deploy and Run in Azure

Your app is finished. Deploy it and see it work.

1. Create an App Service app and configure it with app settings for your storage account connection string and container name. Get the storage account's connection string with `az storage account show-connection-string`, and set the name of the container to be `files`.

   The app name needs to be globally unique. Choose your own name to fill in `<your-unique-app-name>`. Use the storage account name you created previously to replace `<your-unique-storage-account-name>`. Run each of the following commands in order in Azure CLI:

```bash
   az appservice plan create --name blob-exercise-plan --resource-group learn-c6f41766-8f57-4090-b30e-59a3bacb8f90 --sku FREE --location eastus
   ```

```bash
   az webapp create --name <your-unique-app-name> --plan blob-exercise-plan --resource-group learn-c6f41766-8f57-4090-b30e-59a3bacb8f90
   ```

```bash
   aCONNECTIONSTRING=$(az storage account show-connection-string --name <your-unique-storage-account-name> --resource-group learn-c6f41766-8f57-4090-b30e-59a3bacb8f90 --output tsv)
   ```

```bash
   az webapp config appsettings set --name <your-unique-app-name> --resource-group learn-c6f41766-8f57-4090-b30e-59a3bacb8f90 --settings AzureStorageConfig:ConnectionString=$CONNECTIONSTRING AzureStorageConfig:FileContainerName=files
```

2. Deploy your app. The following commands publish the site to the pub folder, zip it up into site.zip, and deploy the zip to App Service.
```bash
dotnet publish -o pub
cd pub
zip -r ../site.zip *
```
```bash
az webapp deployment source config-zip --src ../site.zip --name <your-unique-app-name> --resource-group learn-c6f41766-8f57-4090-b30e-59a3bacb8f90
```
To see the running app, in a browser, open https://<your-unique-app-name>.azurewebsites.net. It should look like the following image.

3. Try uploading and downloading some files to test the app. After you upload a few files, to see the blobs in the container, run the following code in the shell. Replace <your-unique-storage-account-name> with the storage account name that you created earlier in the module:

```bash
az storage blob list --account-name <your-unique-storage-account-name> --container-name files --query [].{Name:name} --output table
```





