# Example - Use compute transformations within Azure Data Factory

In some cases, the code-free transformation at scale may not meet your requirements. You can use Azure Data Factory to ingest raw data collected from different sources and work with a range of compute resources such as Azure Databricks, Azure HDInsight, or other compute resources to restructure it as per your requirements.

## ADF and Azure Databricks

As an example, the integration of Azure Databricks with ADF allows you to add Databricks notebooks within an ADF pipeline to leverage the analytical and data transformation capabilities of Databricks. You can add a notebook within your data workflow to structure and transform raw data loaded into ADF from different sources. Once the data is transformed using Databricks, you can then load it to any data warehouse source.

Data ingestion and transformation using the collective capabilities of ADF and Azure Databricks essentially involves the following steps:

1. Create Azure storage account - The fist step is to create an Azure storage account to store your ingested and transformed data.

2. Create an Azure Data Factory - Once you have your storage account setup, you need to create your Azure Data Factory using Azure portal.

3. Create data workflow pipeline - After your storage and ADF is up and running, you start by creating a pipeline, where the first step is to copy data from your source using ADF's Copy activity. Copy Activity allows you to copy data from different on-premises and cloud sources.

4. Add Databricks notebook to pipeline - Once your data is copied to ADF, you add your Databricks notebook to the pipeline, after copy activity. This notebook may contain syntax and code to transform and clean raw data as required.

5. Perform analysis on data - Now that your data is cleaned up and structured into the required format, you can use Databricks notebooks to further train or analyze it to output required results.

You have learned what Azure Data Factory is and how its integration with Azure Databricks helps you to load and transform your data. Now let's create an end-to-end sample data workflow.

## Integrating Azure Databricks notebooks with Azure Data Factory pipeline

There are a number of tasks that needs to be performed to integrate Azure Databricks notebooks with Azure Data Factory pipeline as follows:

1. Generate a Databricks Access Token.

- In the Azure portal, click on Resource groups and then click on awrgstudxx, and then click on awdbwsstudxx where xx are the initials of your name.

- Click on Launch Workspace

- Click the user profile icon in the upper right corner of your Databricks workspace.

- Click User Settings.

- Go to the Access Tokens tab, and click the Generate New Token button.

- Enter a description in the comment "For ADF Integration" and set the lifetime period of 10 days and click on Generate

- Copy the generated token and store in Notepad, and then click on Done.

2. Generate a Databricks Notebook

- On the left of the screen, click on the Workspace icon, then click on the arrow next to the word Workspace, and click on Create and then click on Folder. Name the folder adftutorial, and click on Create Folder. The adftutorial folder appears in the Workspace.

- Click on the drop down arrow next to adftutorial, and then click Create, and then click Notebook.

- In the Create Notebook dialog box, type the name of mynotebook, and ensure that the language states Python, and then click on Create. The notebook with the title of mynotebook appears/

3. Create Linked Services

- In Microsoft Edge, click on the tab for the portal In the Azure portal, and return to Azure Data Factory.

- In the xx-data-factory screen, click on Author & Monitor. Another tab opens up to author an Azure Data Factory solution.

- On the left hand side of the screen, click on the Author icon. This opens up the Data Factory designer.

- At the bottom of the screen, click on Connections, and then click on + New.

- In the New Linked Service, at the top of the screen, click on Compute, and then click on Azure Databricks, and then click on Continue.

4. Create a Pipeline that uses Databricks Notebook Activity.

- On the left hand side of the screen, under Factory Resources, click on the + icon, and then click on Pipeline. This opens up a tab with a Pipeline designer.

- At the bottom of the pipeline designer, click on the parameters tab, and then click on + New

- Create a parameter with the Name of name, with a type of string

- Under the Activities menu, expand out Databricks.

- Click and drag Notebook onto the canvas.

- In the Notebook1, click on Validate, next to the Save as template button. As window appears on the right of the screen that states "Your Pipeline has been validated. No errors were found." Click on the >> to close the window.

- Click on the Publish All to publish the linked service and pipeline. 

5. Trigger a Pipeline Run

- In the Notebook1, click on Add trigger, and click on Trigger Now next to the Debug button.

- The Pipeline Run dialog box asks for the name parameter. Use /path/filename as the parameter here. Click Finish. A red circle appear above the Notebook1 activity in the canvas.

6. Monitor the Pipeline

- On the left of the screen, click on the Monitor tab. Confirm that you see a pipeline run. It takes approximately 5-8 minutes to create a Databricks job cluster, where the notebook is executed.

- Select Refresh periodically to check the status of the pipeline run.

- To see activity runs associated with the pipeline run, select View Activity Runs in the Actions column.

7. Verify the output

- In Microsoft Edge, click on the tab mynotebook - Databricks

- In the Azure Databricks workspace, click on Clusters and you can see the Job status as pending execution, running, or terminated.

- Click on the cluster awdbclstudxx, and then click on the Event Log to view the activities.

