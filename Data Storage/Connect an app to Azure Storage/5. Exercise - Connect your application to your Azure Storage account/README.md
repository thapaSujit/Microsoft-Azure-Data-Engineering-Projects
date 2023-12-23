# Exercise - Connect your application to your Azure Storage account


## Azure Storage client library object model

The Azure Storage client library provides an object model that's used to interact with Azure storage accounts. It's used to quickly connect to an Azure storage account and use the Azure Storage service APIs.

### Azure Storage client library object model

The Azure Storage Blob client library for JavaScript contains many client objects for interacting with Azure Storage Blobs. At the top of this hierarchy is the `BlobServiceClient` object. To use this object in your JavaScript code:

Open your `index.js` file in the code editor and add the following statement immediately after the `require('dotenv').config();` statement: 
```javascript
const { BlobServiceClient } = require("@azure/storage-blob");
```
Now, you need to create a `BlobServiceClient` object in your code by obtaining the storage-account connection string and passing it to the factory method `fromConnectionString` on the `BlobServiceClient` object. Add the following lines of code:

```javascript
const storageAccountConnectionString = process.env.AZURE_STORAGE_CONNECTION_STRING;
const blobServiceClient = BlobServiceClient.fromConnectionString(storageAccountConnectionString);
```
After you have a client object defined in your program, you can use methods on the client object to perform actual work. Methods that make network calls are intentionally asynchronous. The library uses Promises to return asynchronous results. For that reason, you need to mark your main function as async. Replace the main function object with the following code. The line that begins with two forward slashes is a comment.

```javascript
// Function code here
async function main() {
    // Function code here
}
```
ave the changes to index.js.

Now, let's add some code to execute an operation against our storage account. A storage account is organized into one or more containers, which act like folders in your storage account. The blob objects (files) you create in your storage account are stored in one of these containers. You need to create a container in your storage account to store your photos.

The container name must be between 3 and 63 characters long and may only contain lowercase letters and the dash (-) character. For this application, we'll use the name photos.

To create a container in your storage account, you need to obtain a ContainerClient object that represents the container in the storage account. Even though a container doesn't yet exist in our storage account, we can use the ContainerClient to create the container and manage it once it's created.

To obtain the ContainerClient object, call the getContainerClient method on the BlobServiceClient object and provide the name of the container as a parameter. Then, to create the container in your Azure Storage account, use the createIfNotExists method on the containerClient object. Replace the // Function Code here comment line with these statements:
```javascript
// Create a container (folder) if it does not exist
const containerName = 'photos';
const containerClient = blobServiceClient.getContainerClient(containerName);
const containerExists = await containerClient.exists()
if (!containerExists) {
    const createContainerResponse = await containerClient.createIfNotExists();
    console.log(`Create container ${containerName} successfully`, createContainerResponse.succeeded);
} else {
    console.log(`Container ${containerName} already exists`);
}
```

Save the changes to the index.js file.

In the Cloud Shell command line, enter the following command to build and run your program, which creates the container in your storage account:

```bash
node index.js
```
The first time you run the program, you should see a message that the container was created successfully, with a status of true. The second and subsequent times you run your program, you'll see a similar message with a status of false because the container already exists.

You can verify that you've created the container by running the following Azure CLI command. Remember to replace <name> with the name of your storage account.
```bash
az storage container list --account-name <name>
```
