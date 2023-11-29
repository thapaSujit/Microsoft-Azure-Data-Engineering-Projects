## Exercise - Add Azure Storage configuration to your app

### Choose your development language

Let's add support to our Node.js application to retrieve a connection string from a configuration file. We'll start by adding the necessary plumbing to manage a configuration from our JavaScript file.

#### Create an .env configuration file

In the Azure Cloud Shell session, enter the following command to make sure you're in the correct working directory for your project.

```bash
cd PhotoSharingApp
```
#### Enter the following command to create a file named .env:
```bash
touch .env
```
#### Open the project in the Cloud Shell editor:
```bash
code .
```
#### In the editor, select the .env file, and add the following text.

Note: You might need to select the refresh button in code to see the new files.
```bash
AZURE_STORAGE_CONNECTION_STRING=<value>
```

Tip: The AZURE_STORAGE_CONNECTION_STRING is a hard-coded environment variable that's used for Storage APIs to look up access keys.

Save the file using the keyboard shortcut (Ctrl+S) or select the ellipsis icon (...) in the title bar of the editor, and then select Save from the context menu.

Now, we need to use an Azure command to obtain the actual storage account connection string. In Cloud Shell session, run the following command, replacing <name> with the storage account name that you created in the previous exercise.

```bash
az storage account show-connection-string \
  --resource-group learn-d5fa5a35-40fa-4506-a4c7-6f6e359dc240 \
  --query connectionString \
  --name <name>
```
#### The response is a connection string bounded by quotes, which looks much like the following example:
```bash
"DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName=storage1ab;AccountKey=QtSCGB...7AeoW0Hw=="
```

Copy the connection string, and replace <value> in the .env file with this connection string.

Press Ctrl+S to save the file.

Add support to read an environment configuration file
Node.js apps can include support to read from the .env file by adding the dotenv package.

In Cloud Shell session, run the following command to add a dependency to the dotenv package using npm:

```bash
npm install dotenv --save
```

Add code to read the configuration file
Now that we've added the required libraries to enable reading the configuration, we need to enable that functionality in our application.

In the editor, open the index.js file.

At the top of the file is a line of code #!/usr/bin/env node. Underneath that line, add the following line of code:
```bash
require('dotenv').config();
```

Press Ctrl+S to save the file.

Now that we have the JSON config all wired up, we can start adding code to use our storage account.

