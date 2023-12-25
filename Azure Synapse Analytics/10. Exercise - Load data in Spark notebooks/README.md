# Load data in Spark notebooks

There are several options available for ingesting data into a notebook. Currently, it is possible to load data from an Azure Storage Account, and an Azure Synapse Analytics dedicated SQL pool.

Some examples for reading data in a notebook are as follows:

- Read a CSV from Azure Data Lake Store Gen2 as an Apache Spark DataFrame

- Read a CSV from Azure Storage Account as an Apache Spark DataFrame

- Read data from the primary storage account

**Example 1: Read a CSV file from an Azure Data Lake Store Gen2 store as an Apache Spark DataFrame.**
The following code is used to read a CSV file from an Azure Data Lake Store Gen2 store as an Apache Spark DataFrame.

```bash
from pyspark.sql import SparkSession
from pyspark.sql.types import *
account_name = "Your account name"
container_name = "Your container name"
relative_path = "Your path"
adls_path = 'abfss://%s@%s.dfs.core.windows.net/%s' % (container_name, account_name, relative_path)

spark.conf.set("fs.azure.account.auth.type.%s.dfs.core.windows.net" %account_name, "SharedKey")
spark.conf.set("fs.azure.account.key.%s.dfs.core.windows.net" %account_name ,"Your ADLS Gen2 Primary Key")

df1 = spark.read.option('header', 'true') \
                .option('delimiter', ',') \
                .csv(adls_path + '/Testfile.csv')
```

There are parameter name values that you need to replace in the above code to ensure that it works, including:

- account_name: Replace "Your account name" with the storage account name you wish to use

- container_name: Replace "Your container name" with the storage container you wish to use

- relative_path: Replace "Your path" with the relative path of where the file is stored

- adls_path: The adls_path is defined by passing through the above parameters.

**Example 2: Read a CSV file from Azure Storage Account as a Spark DataFrame.**

The following code is used to read a CSV file from Azure Storage Account as an Apache Spark DataFrame.

```bash
from pyspark.sql import SparkSession
from pyspark.sql.types import *

blob_account_name = "Your blob account name"
blob_container_name = "Your blob container name"
blob_relative_path = "Your blob relative path"
blob_sas_token = "Your blob sas token"

wasbs_path = 'wasbs://%s@%s.blob.core.windows.net/%s' % (blob_container_name, blob_account_name, blob_relative_path)
spark.conf.set('fs.azure.sas.%s.%s.blob.core.windows.net' % (blob_container_name, blob_account_name), blob_sas_token)

df = spark.read.option("header", "true") \
            .option("delimiter","|") \
            .schema(schema) \
            .csv(wasbs_path)
```

There are parameter name values that you need to replace in the above code to ensure that it works, including:

- blob_account_name: Replace "Your blob account name" with the name of your blob account.

- blob_container_name: Replace "Your blob container" with the name of the blob container the file is in.

- blob_relative_path: Replace "Your blob relative path" with the name of the relative path pointing to the csv you want to read.

- blob_sas_token: Replace "Your blob sas token" with the blob SAS key.

**Example 3: Read data from the primary storage account**

The third possibility is to read data from the primary storage account using the Data tab in the Azure Synapse Studio environment. 

Right-click on a file and select New notebook to view a new notebook with the data generated.

## Ingest and explore Parquet files from a data lake with Apache Spark for Azure Synapse

Tailwind Traders has Parquet files stored in their data lake. They need to quickly access the files and explore them using Apache Spark.

You recommend that they use the Data hub to view the Parquet files in the connected storage account, and then use the new notebook context menu to create a new Synapse notebook that loads a Spark DataFrame with the contents of a selected Parquet file.

You can complete this task using the following steps:

1. Open the Azure Synapse Studio.

2. Select the Data hub.

3. Select the Linked tab (1) and expand the primary data lake storage account (the name may differ from what you see here; it is the first storage account listed). 

Select the wwi-02 container (2) and browser to the sale-small/Year=2010/Quarter=Q4/Month=12/Day=20101231 folder (3). 

Right-click the Parquet file (4) and select New notebook (5).

A new notebook is generated with PySpark code to load the data in a Spark DataFrame and display 100 rows with the header.

4. Ensure the Spark pool is attached to the notebook.

5. Select Configure session at the bottom-left of the notebook to change the Spark configuration for this session.

6. Set the number of Executors to 3 (1), then select Apply (2).

We have just set the number of executors allocated to SparkPool01 for the session.

7. Add the following code beneath the code in the cell to define a variable named datalake whose value is the name of the primary storage account (replace the REPLACE_WITH_YOUR_DATALAKE_NAME value with the name of the storage account in line 2):
        - datalake = 'REPLACE_WITH_YOUR_DATALAKE_NAME'

8. Select Run all on the notebook toolbar to execute the notebook.

Note: The first time you run a notebook in a Spark pool, Synapse creates a new session. This can take approximately 3-5 minutes. To run just the cell, either hover over the cell and select the Run cell icon to the left of the cell, or select the cell and then type Ctrl+Enter on your keyboard.

9. After the cell run is complete, change the View to Chart in the cell output.

By default, the cell outputs to a table view when you use the display() function. You can see in the output the sales transaction data stored in the Parquet file for December 31, 2010. Select the Chart visualization to see a different view of the data.

10. Select the View options button to the right.