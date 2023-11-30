## Explore Azure Storage Services

**Completed: 100 XP  
Estimated Time: 5 minutes**

Let's start by taking a quick look at Azure storage services, data styles, and accounts.

### Azure Storage Overview

Microsoft Azure Storage is a managed service that provides durable, secure, and scalable storage in the cloud. Let's break down these terms.

- **Managed**: Microsoft Azure handles maintenance and any critical problems for you.
  
- **Durable**: Redundancy ensures that your data is safe if there are transient hardware failures. You can also replicate data across datacenters or geographical regions for extra protection from local catastrophe or natural disaster. Data replicated in this way remains highly available if there's an unexpected outage.

- **Secure**: Azure Storage encrypts all data written to the service. Azure Storage provides you with fine-grained control over who has access to your data.

- **Scalable**: Azure Storage is designed to be massively scalable to meet the data storage and performance needs of today's applications. A single Azure subscription can host up to 250 storage accounts per region, each of which has a maximum storage account capacity of five PiB.

### Data Types in Azure Storage Services

Azure storage includes four types of data:

1. **Blobs**: A massively scalable object store for text and binary data; can include support for Azure Data Lake Storage Gen2.
   
2. **Files**: Managed file shares for cloud or on-premises deployments.
   
3. **Queues**: A messaging store for reliable messaging between application components.
   
4. **Table Storage**: A NoSQL store for schema-less storage of structured data. Table Storage isn't covered in this module.

You can access all of these data types in Azure Storage from anywhere in the world over HTTP or HTTPS. Microsoft provides SDKs for Azure Storage services in various programming languages, and in a REST API. You can also visually explore your data right in the Azure portal.

#### Blobs

Azure Blob Storage is an object storage solution optimized for storing massive amounts of unstructured data, such as text or binary data. Blob Storage is ideal for:

- Serving images or documents directly to a browser, including full static websites.
   
- Storing files for distributed access.
   
- Streaming video and audio.
   
- Storing data for backup and restoration, disaster recovery, and archiving.
   
- Storing data for analysis by an on-premises or Azure-hosted service.

Azure Storage supports three kinds of blobs:

- **Block blobs**: Used to hold text or binary files up to ~5 TB (50,000 blocks of 100 MB) in size. The primary use case for block blobs is the storage of files that are read from beginning to end, such as media files or image files for websites.

- **Page blobs**: Used to hold random-access files up to 8 TB in size. Page blobs are used primarily as the backing storage for the VHDs used to provide durable disks for Azure Virtual Machines (Azure VMs).

- **Append blobs**: Made up of blocks like block blobs, but they're optimized for append operations. These blobs are frequently used for logging information from one or more sources into the same blob.

#### Files

Azure File storage allows you to set up highly available network file shares that you can access by using the standard Server Message Block (SMB) protocol. This means that multiple VMs can share the same files with both read and write access. You can also read the files using the REST interface or the storage client libraries. You can also associate a unique URL to any file to allow fine-grained access to a private file for a set period of time.

File shares can be used for many common scenarios:

- Storing shared configuration files for VMs, tools, or utilities so that everyone is using the same version.
   
- Log files such as diagnostics, metrics, and crash dumps.
   
- Shared data between on-premises applications and Azure VMs to allow migration of apps to the cloud over a period of time.

#### Queues

Azure Queue Storage is used to store and retrieve messages. Queue messages can be up to 64 KB in size, and a queue can contain millions of messages. Queues are used to store lists of messages to be processed asynchronously.

You can use queues to loosely connect different parts of your application together. For example, we could perform image processing on the photos uploaded by our users. Perhaps we want to provide some sort of face-detection or tagging capability, so people can search through all the images they've stored in our service. We could use queues to pass messages to our image-processing service to let it know that new images have been uploaded and are ready for processing. This sort of architecture allows you to develop and update each part of the service independently.

#### Table Storage

For more information about Azure Table Storage, see [Table Storage Overview](#).

### Azure Storage Accounts

To access any of these services from an application, you have to create a storage account. The storage account provides a unique namespace in Azure to store and access your data objects. A storage account contains any blobs, files, queues, tables, and VM disks that you create under that account.

#### Create a Storage Account

You can create an Azure storage account using the Azure portal, Azure PowerShell, or Azure CLI. Azure Storage provides three distinct account options with different pricing and features supported.

1. **General-purpose v2 (GPv2)**: General-purpose v2 (GPv2) accounts are storage accounts that support all of the latest features for blobs, files, queues, and tables. Pricing for GPv2 accounts has been designed to deliver the lowest per-gigabyte prices.

2. **General-purpose v1 (GPv1)**: General-purpose v1 (GPv1) accounts provide access to all Azure Storage services, but might not have the latest features or the lowest per-gigabyte pricing.

3. **Blob Storage Accounts**: A legacy account type, blob storage accounts support all the same block blob features as GPv2, but they're limited to supporting only block and append blobs. Pricing is broadly similar to pricing for general-purpose v2 accounts.
