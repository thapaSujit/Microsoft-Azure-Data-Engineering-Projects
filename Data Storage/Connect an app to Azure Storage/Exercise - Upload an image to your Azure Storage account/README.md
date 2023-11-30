## Exercise - Upload an image to your Azure Storage account

Let's add code to upload an image to our Azure Storage account. In this example, we're going to upload the following image to our Azure Storage container. If you're working on your local machine, right-click this image and save it to the same folder as where you have your application.

If you're working in the Microsoft Learn Sandbox environment, run the following command in the application folder to download the image into your sandbox:

```bash
wget https://github.com/MicrosoftDocs/mslearn-connect-app-to-azure-storage/blob/main/images/docs-and-friends-selfie-stick.png?raw=true -O docs-and-friends-selfie-stick.png
```

## Upload an image to blob storage
To work with blob objects in your Azure Storage container, you use a BlockBlobClient object. The BlockBlobClient object has methods to upload, download, list, and delete blob objects in a container. To get a BlockBlobObject, call the method getBlockBlobClient on the ContainerClient object. Then, you can use the uploadFile method to upload your image to Azure Storage.

Add this code to your index.js program file immediately after the code that creates the container:

```bash
const filename = 'docs-and-friends-selfie-stick.png';
const blockBlobClient = containerClient.getBlockBlobClient(filename);
blockBlobClient.uploadFile(filename);
```
## List objects in an Azure Blob Storage container
To verify that our code is working, we can call the listBlobsFlat method on the ContainerClient object in our program. Add this code to the index.js file, then save the file:
```bash
let blobs = containerClient.listBlobsFlat();
let blob = await blobs.next();
while (!blob.done) {
    console.log(`${blob.value.name} --> Created: ${blob.value.properties.createdOn}   Size: ${blob.value.properties.contentLength}`);
    blob = await blobs.next();
}
```
This code will print all the blobs in our Azure Blob Storage container with the date the blob was created and its size. For our program, this code should print one row representing the single image we've uploaded.

