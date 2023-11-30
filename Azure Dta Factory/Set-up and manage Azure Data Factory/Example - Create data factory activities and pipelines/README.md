# Example - Create data factory activities and pipelines

Activities within Azure Data Factory define the actions that will be performed on the data and there are three categories including:
1. Data movement activities

2. Data transformation activities

3. Control activities

1. Data movement activities
Data movement activities simply move data from one data store to another. You can use the Copy Activity to perform data movement activities, or by using JSON. There are a wide range of data stores that are supported as a source and as a sink. This list is ever increasing, and you can find the latest information online.

2. Data transformation activities
Data transformation activities can be performed natively within the authoring tool of Azure Data Factory using the Mapping Data Flow. Alternatively, you can call a compute resource to change or enhance data through transformation, or perform analysis of the data. These include compute technologies such as Azure Databricks, Azure Batch, SQL Database and Azure Synapse Analytics, Machine Learning Services, Azure Virtual machines and HDInsight. You can make use of any existing SQL Server Integration Services (SSIS) Packages stored in a Catalog to execute in Azure

3. Control activities
When graphically authoring ADF solutions, you can use the control flow within the design to orchestrate pipeline activities that include chaining activities in a sequence, branching, defining parameters at the pipeline level, and passing arguments while invoking the pipeline on-demand or from a trigger. The current capabilities include:

| Control Activity         | Description                                                                                                              |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------|
| Execute Pipeline Activity | Allows a Data Factory pipeline to invoke another pipeline.                                                                |
| ForEach Activity          | Defines a repeating control flow in your pipeline. Used to iterate over a collection and execute specified activities in a loop. |
| Web Activity              | Can be used to call a custom REST endpoint from a Data Factory pipeline.                                                  |
| Lookup Activity           | Can be used to read or look up a record/table name/value from any external source. Output can be referenced by succeeding activities. |
| Get Metadata Activity     | Can be used to retrieve metadata of any data in Azure Data Factory.                                                       |
| Until Activity            | Implements Do-Until loop, executing a set of activities in a loop until the specified condition evaluates to true. Can have a timeout value. |
| If Condition Activity     | Branches based on a condition that evaluates to true or false, similar to an if statement in programming languages.         |
| Wait Activity             | Makes the pipeline wait for a specified period of time before continuing with the execution of subsequent activities.       |

## Activities and pipelines
# Defining activities
When using JSON notation, the activities section can have one or more activities defined within it. There are two main types of activities: Execution and Control Activities. Execution (also known as Compute) activities include data movement and data transformation activities. They have the following top-level structure:

```bash
{
    "name": "Execution Activity Name",
    "description": "description",
    "type": "<ActivityType>",
    "typeProperties":
    {
    },
    "linkedServiceName": "MyLinkedService",
    "policy":
    {
    },
    "dependsOn":
    {
    }
}
```
### The following table describes properties in the above JSON:

| Property            | Description                                                           | Required                                      |
|---------------------|-----------------------------------------------------------------------|-----------------------------------------------|
| name                | Name of the activity.                                                 | Yes                                           |
| description         | Text describing what the activity is used for.                       | Yes                                           |
| type                | Defines the type of the activity.                                     | Yes                                           |
| linkedServiceName   | Name of the linked service used by the activity.                      | Yes (HDInsight, Machine Learning Batch Scoring Activity, Stored Procedure Activity) |
| typeProperties      | Properties in the typeProperties section depend on each type of activity. | No                                          |
| policy              | Policies that affect the run-time behavior of the activity. Includes timeout and retry behavior. | No                                          |
| dependsOn           | Defines activity dependencies, and how subsequent activities depend on previous activities. | No                                          |

### Defining control activities
A Control Activity in Data Factory is defined in JSON format as follows:

```bash
{
    "name": "Control Activity Name",
    "description": "description",
    "type": "<ActivityType>",
    "typeProperties":
    {
    },
    "dependsOn":
    {
    }
}
```
### The following table describes properties in the above JSON:

| Property           | Description                                                          | Required                                      |
|--------------------|----------------------------------------------------------------------|-----------------------------------------------|
| name               | Name of the activity.                                                 | Yes                                           |
| description        | Text describing what the activity is used for.                       | Yes                                           |
| type               | Defines the type of the activity.                                     | Yes                                           |
| linkedServiceName  | Name of the linked service used by the activity.                      | Yes (HDInsight, Machine Learning Batch Scoring Activity, Stored Procedure Activity) |
| typeProperties     | Properties in the typeProperties section depend on each type of activity. | No                                          |
| policy             | Policies that affect the run-time behavior of the activity. Includes timeout and retry behavior. | No                                          |
| dependsOn          | Defines activity dependencies, and how subsequent activities depend on previous activities. | No                                          |

### Defining pipelines

```bash
{
    "name": "PipelineName",
    "properties":
    {
        "description": "pipeline description",
        "activities":
        [
        ],
        "parameters": {
         }
    }
}
```
### The following table describes properties in the above JSON:
Here is the sample output:

| Property     | Description                                            | Required |
|--------------|--------------------------------------------------------|----------|
| name         | Name of the pipeline.                                  | Yes      |
| description  | Text describing what the pipeline is used for.         | No       |
| activities   | The activities section can have one or more activities defined within it. | Yes      |
| parameters   | The parameters section can have one or more parameters defined within the pipeline, making your pipeline flexible for reuse. | No       |

## Example
The following JSON defines pipeline named "MyFirstPipeline" that contains one activity type of HDInsightHive that will call a query from a script name "partitionweblogs.hql" that is stored in the linked service named "StorageLinkedService", with an input named "AzureBlobInput" and an output named "AzureBlobOutput". It executes this against the compute resource defined in the linked service named "HDInsightOnDemandLinkedService"

The pipeline is scheduled to execute on a monthly basis, and will attempt to execute 3 times should it fail.



```bash
{
    "name": "MyFirstPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@ctostorageaccount.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@ctostorageaccount.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
              },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2017-04-01T00:00:00Z",
        "end": "2017-04-02T00:00:00Z",
        "isPaused": false,
        "hubName": "ctogetstarteddf_hub",
        "pipelineMode": "Scheduled"
    }
}
```
