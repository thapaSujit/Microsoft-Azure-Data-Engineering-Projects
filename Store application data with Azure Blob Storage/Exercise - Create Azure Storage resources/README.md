# Designing Azure Blob Storage Usage for the App

## Overview

After determining how data will be stored across storage accounts, containers, and blobs, it's essential to plan the Azure resources needed to support the app.

## Storage Accounts

Storage account creation is typically an administrative or management activity that occurs before deploying and running the app. Use deployment or environment setup scripts, Azure Resource Manager templates, or manual setup for creating storage accounts. Generally, apps, except administrative tools, shouldn't have permissions to create storage accounts.

## Containers

Unlike storage account creation, container creation is a lightweight activity suitable for execution within an app. Apps often create and delete containers as part of their tasks.

For apps relying on a predefined set of containers with hard-coded or preconfigured names, allowing the app to create necessary containers on startup or first usage is a practical approach. This way, the app can handle container creation dynamically, eliminating the need for both the app and the deployment process to know container names in advance.

## Exercise

In this exercise, you will finalize an unfinished app by adding code to utilize Azure Blob Storage. While this exercise focuses more on exploring the Blob Storage API than designing an organizational and naming scheme, it's crucial to consider how the app stores data.

Your app works like a shared folder that accepts file uploads and makes them available for download. It doesn't use a database for organizing blobs. Instead, it sanitizes the names of uploaded files and uses them as blob names directly. All uploaded files are stored in a single container.

The code you start with compiles and runs. The parts responsible for storing and loading data are empty. After you complete the code, deploy the app to Azure App Service and test it.

## Storage account
Use Azure Cloud Shell with the Azure CLI to create a storage account. You need to provide a unique name for the storage account. Make a note of it for later. Replace <your-unique-storage-account-name> with a name you choose. Storage account names must be between 3 and 24 characters in length and use numbers and lower-case letters only.

To create the storage account, run this command.
```bash
az storage account create \
  --kind StorageV2 \
  --resource-group learn-c6f41766-8f57-4090-b30e-59a3bacb8f90 \
  --location eastus \
  --name <your-unique-storage-account-name>
```

## Container
The app you work with in this module uses a single container. Follow the best practice of letting the app create the container at startup. However, you can create containers from the Azure CLI. If you'd like to see the documentation, run the az storage container create -h command in Cloud Shell.


