## Exercise - Add the Storage Client Library to Your App

**Completed: 100 XP  
Estimated Time: 5 minutes**

Sandbox activated! Time remaining:   
You have used 3 of 10 sandboxes for today. More sandboxes will be available tomorrow.

### Choose Your Development Language

Let's integrate the Azure Storage Blob Client Library for JavaScript into your application. The Node.js client library is available through the Node Package manager (npm). You'll want to add the `@azure/storage-blob` package to your `packages.json` file.

### Add the Azure Storage Package

In Azure Cloud Shell, `cd` to the `PhotoSharingApp` directory if you aren't already there.

Enter the following code to add the `@azure/storage-blob` package to the application. Make sure to supply the `--save` option so it persists to `packages.json`.

```bash
npm install @azure/storage-blob --save
```
You'll see some console activity while the client library and all the required dependencies are downloaded. When the download is complete, let's build and run the app again to make sure everything is ready to go. Enter the following command:

```bash
node index.js
```
As before, the return output "Hello, World!" appears.
