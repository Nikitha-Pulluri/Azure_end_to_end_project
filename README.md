# Azure_end_to_end_project

I did an Azure end to end project using a YouTube video. I wanted to create a readme for the project. 

Step 1. Setting up Azure Environment 
-I first downloaded a dataset from Microsoft (https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver17&tabs=ssms) 
-And then downloaded SQL Server SSMS using (https://learn.microsoft.com/en-us/ssms/install/install?view=sql-server-ver17) this link. 
-Inserted the data into sql server
-Now this SQL Server acts as On Prem data for my project
-I then took a subscription (Azure subscription 1) in Azure which is a student 30 day free trial
-I created a resource group
-I that resource group I created data factory (intech-df-np), Synapse (intech-synapse-np), key vault (intech-keyvault-np), storage account (intechstoragenp), databricks (Intech-databricks)


Step 2. Copying data from SQL Server
-In the data factory I started creating a pipeline and imported Copy Data component and started creating new source (
SqlServerTableOnPrem) as SQL Server
-I created a new Linked Service(SqlServerOnPremLinkedServices) and created New Self Hosted integration runtime (SHIR) as the data is from outside the azure
-I now created a sink(ParquetSink) as Azure Data Lake and created a new linked service (AzureDataLakeStorageLinkedServices) and create Integration runtime (AutoResolveIntegrationRuntime) as the sink is within Azure. 
-Now I get the databse from SQL Server to Azure data lake

step 3. Creating a pipeline in Data Factory
-craeted a new pipeline Named copy_all_tables and added a lookup activity 
-added another activity for each upon success of lookup
-within the foreach we added copy data activity 
-We added source as SqlServerTableOnPrem and sink as ParquetSink for the copy data activity.

Step 4. Data Transformations
-Launched the databricks and added a cluster with single node
