# Azure End-to-End Data Engineering Project

## 📘 Introduction

This project demonstrates a complete data engineering workflow on Microsoft Azure. Data is sourced from an on-premise SQL Server and ingested into Azure, transformed using Databricks, modeled with Synapse Analytics, and visualized through Power BI. This exercise enhances data orchestration, security, transformation, and analytics capabilities.

---

## 🏗️ Project Architecture

![Architecture Diagram](./A_flowchart_in_the_image_illustrates_an_Azure_end-.png)

---

## 🛠️ Tools Used

* **SQL Server Management Studio (SSMS)**
* **Azure Data Factory (ADF)**
* **Azure Key Vault**
* **Azure Data Lake Storage Gen2**
* **Azure Synapse Analytics (Serverless SQL Pool)**
* **Azure Databricks**
* **Power BI Desktop**
* **Self-Hosted Integration Runtime (SHIR)**

---

## 📃 Step-by-Step Implementation

### Step 1: Setting Up the Azure Environment

* Downloaded the **AdventureWorks** dataset from Microsoft documentation.
* Installed **SQL Server Management Studio (SSMS)** to create an on-premise SQL Server database.
* Inserted the sample data into the SQL Server tables.
* This SQL Server instance served as the **on-premises data source**.
* Created an Azure subscription (30-day student trial).
* Created a new **resource group** in Azure.
* Created the required Azure resources within the group:

  * Azure Data Factory (`intech-df-np`)
  * Azure Synapse Analytics (`intech-synapse-np`)
  * Azure Key Vault (`intech-keyvault-np`)
  * Azure Storage Account (`intechstoragenp`)
  * Azure Databricks Workspace (`intech-databricks`)

### Step 2: Copying Data from SQL Server

* In Azure Data Factory, created a pipeline with the **Copy Data** activity.
* Created **Linked Services**:

  * **Source**: SQL Server using `SqlServerOnPremLinkedServices` with **Self-Hosted Integration Runtime** since the data is external.
  * **Sink**: Azure Data Lake using `AzureDataLakeStorageLinkedServices` with default Integration Runtime.
* Configured the **copy activity** to move data from SQL Server to Azure Data Lake in **Parquet** format.

### Step 3: Creating an ADF Pipeline

* Developed a pipeline named `copy_all_tables`.
* Included the following activities:

  * **Lookup activity** to fetch the list of tables.
  * **ForEach activity** to iterate through each table.
  * **Copy Data activity** within the loop to copy each table from SQL Server to Azure Data Lake.

### Step 4: Data Transformations with Databricks

* Launched **Azure Databricks** and created a single-node cluster.
* Created and executed notebooks:

  * **`storagemount`**: Mounted the Azure Data Lake to Databricks.
  * **`bronze_to_silver`**: Cleaned and formatted raw data (e.g., converting `ModifiedDate` to `yyyy-MM-dd`).
  * **`silver_to_gold`**: Renamed columns to snake\_case and performed final transformations.
* Integrated Databricks notebooks into ADF pipeline for automation.

### Step 5: Data Loading into Synapse

* In **Synapse Studio**, created a **serverless SQL database** named `gold_db`.
* Linked the storage account to Synapse for querying Delta tables.
* Verified table presence in the Gold Layer by querying Delta format files.
* Created views using:

  ```sql
  CREATE VIEW view_name AS SELECT * FROM OPENROWSET(...)
  ```
* Developed a stored procedure to dynamically create views from all gold tables.
* Executed Synapse pipeline to expose all views for Power BI.

### Step 6: Data Reporting with Power BI

* Opened Power BI and connected using the **Azure Synapse SQL endpoint**.
* Used Microsoft account for authentication.
* Loaded all SQL views (Gold Layer) into Power BI.
* Reviewed and corrected relationships:

  * Adjusted incorrect one-to-one relationships.
  * Ensured proper one-to-many joins.
* Built an interactive dashboard:

  * **Cards** for Total Products and Total Sales
  * **Donut Chart** to show gender split based on title field
  * **Slicers** for Title and Product Category

### Step 7: Automation and Monitoring

* Scheduled ADF pipelines to run daily for automation.
* Enabled monitoring in ADF and Synapse for real-time pipeline status.

### Step 8: Security and Governance

* Configured **Azure Entra ID** for **Role-Based Access Control (RBAC)**.
* Used **Azure Key Vault** to securely manage credentials and secrets.

### Step 9: End-to-End Testing

* Added new records in SQL Server.
* Verified that pipelines ran successfully.
* Ensured new data was reflected in Power BI dashboard.

---

## 🔍 Resource Explanation

### On-Prem SQL Server

Acts as the initial data source. Data is manually loaded into this system using SSMS. It mimics a typical enterprise environment where legacy systems are still on-premise.

### Azure Data Factory (ADF)

Used for orchestrating data movement and transformations. Pipelines are built to move data from SQL Server to the data lake and to execute Databricks notebooks.

### Azure Key Vault

Stores secrets like database connection strings securely. Helps manage access to sensitive credentials without hardcoding them in pipeline definitions.

### Azure Data Lake Gen2

Cloud-based storage layer where data is stored in stages (bronze, silver, gold). Supports hierarchical data storage and is highly scalable.

### Azure Synapse Analytics

A unified analytics service that allows querying data directly from Data Lake using serverless SQL. Views are created for reporting without needing to move data.

### Azure Databricks

Performs all data transformations using Spark. Responsible for cleaning, formatting, renaming, and preparing data from bronze to gold layers.

### Power BI

Data visualization tool used to build dashboards. Connects to Synapse to access cleaned data and build interactive visuals.

### Self-Hosted Integration Runtime (SHIR)

A bridge that allows Data Factory to connect to on-prem data sources. Required for secure data movement from SQL Server to Azure.

### Bronze Layer

Raw data copied as-is from SQL Server without any transformation. Acts as the staging zone.

### Silver Layer

Partially cleaned and standardized data. Format corrections and type conversions are done here.

### Gold Layer

Fully transformed, query-ready data structured for analytics and reporting. This layer is directly queried by Synapse and Power BI.

---

## 📚 Resources

* Dataset: [AdventureWorks Sample](https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver17&tabs=ssms)
* SSMS: [Install Link](https://learn.microsoft.com/en-us/ssms/install/install?view=sql-server-ver17)
* YouTube Video: [Mr. K's Tutorial](https://youtu.be/ygJ11fzq_ik?si=wN9ww46PZvwx_la7)
* GitHub Reference: [Luke Byrne's Project](https://github.com/lukejbyrne/rg-data-engineering-project?email=pullurinikitha1515%40gmail.com)
