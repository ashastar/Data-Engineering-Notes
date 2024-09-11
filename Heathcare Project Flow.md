# Healthcare Database Schema

## Database Name: `HealthcareManagementDB`

### Tables in the Database:

1. **Table Name:** `Patients`
   - **Description:** Stores patient personal information.
   - **Columns:**
     - `PatientID` (INT, Primary Key)
     - `FirstName` (VARCHAR)
     - `LastName` (VARCHAR)
     - `DOB` (DATE)
     - `Gender` (VARCHAR)
     - `ContactNumber` (VARCHAR)
     - `Email` (VARCHAR)
     - `Address` (VARCHAR)
     - `InsuranceNumber` (VARCHAR)

2. **Table Name:** `Doctors`
   - **Description:** Stores doctor information.
   - **Columns:**
     - `DoctorID` (INT, Primary Key)
     - `FirstName` (VARCHAR)
     - `LastName` (VARCHAR)
     - `Specialization` (VARCHAR)
     - `PhoneNumber` (VARCHAR)
     - `Email` (VARCHAR)
     - `Address` (VARCHAR)

3. **Table Name:** `Appointments`
   - **Description:** Stores information about patient appointments.
   - **Columns:**
     - `AppointmentID` (INT, Primary Key)
     - `PatientID` (INT, Foreign Key from Patients)
     - `DoctorID` (INT, Foreign Key from Doctors)
     - `AppointmentDate` (DATETIME)
     - `Status` (VARCHAR) — e.g., Scheduled, Completed, Canceled
     - `Notes` (TEXT)

4. **Table Name:** `MedicalRecords`
   - **Description:** Stores patient medical history and records.
   - **Columns:**
     - `RecordID` (INT, Primary Key)
     - `PatientID` (INT, Foreign Key from Patients)
     - `DoctorID` (INT, Foreign Key from Doctors)
     - `VisitDate` (DATETIME)
     - `Diagnosis` (VARCHAR)
     - `Treatment` (TEXT)
     - `Prescription` (TEXT)

5. **Table Name:** `Billing`
   - **Description:** Stores billing details for services provided to patients.
   - **Columns:**
     - `BillID` (INT, Primary Key)
     - `PatientID` (INT, Foreign Key from Patients)
     - `AppointmentID` (INT, Foreign Key from Appointments)
     - `Amount` (DECIMAL)
     - `DateIssued` (DATETIME)
     - `Status` (VARCHAR) — e.g., Paid, Unpaid, Pending

6. **Table Name:** `Medications`
   - **Description:** Stores information about available medications.
   - **Columns:**
     - `MedicationID` (INT, Primary Key)
     - `Name` (VARCHAR)
     - `Dosage` (VARCHAR)
     - `Description` (TEXT)
     - `SideEffects` (TEXT)

7. **Table Name:** `Insurance`
   - **Description:** Stores details about insurance providers.
   - **Columns:**
     - `InsuranceID` (INT, Primary Key)
     - `PatientID` (INT, Foreign Key from Patients)
     - `ProviderName` (VARCHAR)
     - `PolicyNumber` (VARCHAR)
     - `CoverageDetails` (TEXT)

8. **Table Name:** `LabTests`
   - **Description:** Stores information about lab tests prescribed to patients.
   - **Columns:**
     - `TestID` (INT, Primary Key)
     - `PatientID` (INT, Foreign Key from Patients)
     - `DoctorID` (INT, Foreign Key from Doctors)
     - `TestName` (VARCHAR)
     - `TestDate` (DATETIME)
     - `Results` (TEXT)

### Relationships:
- **Patients ↔ Appointments**: A patient can have many appointments.
- **Doctors ↔ Appointments**: A doctor can have many appointments.
- **Patients ↔ MedicalRecords**: A patient can have many medical records.
- **Patients ↔ Billing**: A patient can have many bills.
- **Doctors ↔ MedicalRecords**: A doctor can be linked to many medical records.
- **Patients ↔ LabTests**: A patient can undergo many lab tests.


# End-to-End Azure Data Engineering in Healthcare

## Project Overview
This project demonstrates an end-to-end Azure data engineering solution for a healthcare management system. It covers the entire pipeline, from data ingestion from on-premises healthcare data to Power BI reporting. The goal is to showcase how data can be ingested, transformed, and reported using Azure technologies in the context of healthcare.

### Business Objective
This project provides a learning opportunity to understand healthcare data engineering practices. The focus is on ETL pipeline techniques and automating the process of migrating local healthcare data to the cloud. The data includes patient records, medical appointments, prescriptions, and billing information.

## Healthcare Data Flow

1. **Data Sources:**
   - On-premises SQL Server with a healthcare database (`HealthcareManagementDB`).
   - Tables:
     - `Patients` (Patient information)
     - `Doctors` (Doctor information)
     - `Appointments` (Appointment schedules)
     - `MedicalRecords` (Medical history)
     - `Billing` (Invoices for patient services)
     - `Medications` (Medication data)
     - `LabTests` (Lab test results)
     - `Insurance` (Patient insurance details)

2. **Tools & Technologies:**
   - Azure Data Factory (ADF) for ETL
   - Azure Databricks (PySpark) for data transformation
   - Azure Data Lake Storage (ADLS) for storing raw data
   - Azure Synapse Analytics for data warehousing and querying
   - Power BI for reporting and visualization
   - Azure Key Vault for securely storing credentials
## 1. Data Ingestion

Data ingestion is the first step in moving healthcare data from the local SQL database to Azure.

- **Objective:** Ingest data from on-premises healthcare database to Azure.
- **Tools:** Azure Data Factory (ADF) and Self-Hosted Integration Runtime.
### Steps:
1. **Install Self-Hosted Integration Runtime**: Install this runtime to enable secure communication between Azure Data Factory and the on-premises SQL Server.
2. **Establish Connection**: Set up a linked service in Azure Data Factory to connect to the local SQL Server.
3. **Copy Pipeline Setup**: Configure an ADF pipeline to copy data from the on-premises SQL database to Azure Data Lake Storage (ADLS). The data is stored in the **bronze** layer of the data lake.

### Data Flow:
- SQL Database → Azure Data Factory → Azure Data Lake (Bronze Layer)
---
## 2. Data Transformation

After ingesting data into the bronze layer, the next step is to transform the data based on the Medallion architecture (Bronze, Silver, Gold). The **bronze** layer contains raw data, the **silver** layer contains cleansed and validated data, and the **gold** layer holds the final version of the data, optimized for analytics.

- **Objective:** Clean, transform, and aggregate healthcare data for analysis.
- **Tools:** Azure Databricks (PySpark), Azure Data Lake.

### Steps:
1. **Mount ADLS in Azure Databricks**: Use Databricks to mount the Azure Data Lake Storage (ADLS) where the ingested data resides.
2. **Transform Data from Bronze to Silver**: Clean and standardize data. For example, patient records can be cleansed for any missing data and normalized into consistent formats.
   - Transformations include removing duplicates, correcting patient contact information, and standardizing medical record formats.
3. **Transform Data from Silver to Gold**: Aggregate and optimize data for analytical purposes.
   - Example: Combine patient, appointment, and billing data to analyze healthcare costs, doctor performance, or patient demographics.

### Data Flow:
- Bronze (Raw Data) → Silver (Cleansed Data) → Gold (Aggregated Data)
---
## 3. Data Loading

Once the data is transformed, it is loaded into Azure Synapse Analytics for further querying and analysis.

- **Objective:** Load transformed healthcare data into Azure Synapse Analytics for advanced analysis and reporting.
- **Tools:** Azure Synapse Analytics, Azure Data Lake Storage.

### Steps:
1. **Link Azure Synapse to ADLS Gold Layer**: Establish a connection between Azure Synapse and the **gold** layer in ADLS.
2. **Create SQL Views**: Use stored procedures or direct SQL to create views of the transformed data, such as:
   - `Patient_Overview`: A view that combines patient, medical records, and appointment history.
   - `Financial_Report`: A view that summarizes billing information across departments and doctors.
3. **Store SQL Views in Serverless SQL Pool**: Store the views in Synapse's serverless SQL pool to allow fast, scalable querying.

### Data Flow:
- Gold Layer (ADLS) → Azure Synapse Analytics (SQL Views)
---
## 4. Data Reporting

The final step is to create healthcare reports using Power BI, which connects directly to the Azure Synapse SQL pool.

- **Objective:** Visualize healthcare data for insights into hospital performance, patient demographics, and financial data.
- **Tools:** Power BI, Azure Synapse Analytics.

### Steps:
1. **Connect Power BI to Azure Synapse**: Use DirectQuery to connect Power BI to the serverless SQL pool in Azure Synapse.
2. **Develop Power BI Reports**: Build interactive dashboards and reports using Power BI to visualize healthcare KPIs:
   - **Patient Demographics**: Age, gender, and condition distribution.
   - **Appointment History**: Average waiting times, doctor performance.
   - **Financial Reports**: Revenue by department, unpaid invoices.

### Example Reports:
- **Patient Health Overview**: Displays metrics related to patient appointments, diagnoses, and treatment effectiveness.
- **Financial Performance**: Analyzes the hospital’s revenue streams, costs, and outstanding payments.

### Data Flow:
- Azure Synapse Analytics (SQL Views) → Power BI (Dashboards)

---

## 5. Final Pipeline Test

To validate the pipeline, new patient records are added to the local SQL database, and the pipeline is tested to ensure that:
- The new records are ingested into Azure Data Lake (Bronze).
- Data is transformed through Databricks (Silver, Gold).
- The new data is available in the Power BI report for analysis.

For example, adding 10 new patients to the `Patients` table in the SQL Server should result in updated patient count and related metrics in Power BI.

---

## Conclusion and Limitations

This project demonstrates the implementation of an end-to-end data engineering pipeline for healthcare data using Azure. The solution allows for secure ingestion, transformation, and reporting of healthcare data.

### Key Learnings:
- Azure Data Factory is effective for automating the ingestion process from on-premises databases.
- Azure Databricks enables scalable data transformation using PySpark, following the Medallion architecture.
- Power BI’s integration with Azure Synapse provides dynamic, real-time reporting capabilities.

### Limitations:
- **Data Size**: This example used a small dataset, which may not represent real-w


# End-to-End Azure Data Engineering Pipeline for Healthcare

## Project Overview
This project demonstrates an end-to-end Azure Data Engineering solution for a healthcare system. The pipeline moves data from an on-premises SQL Server to Snowflake, with all intermediate layers stored in Azure Data Lake Storage (ADLS) using the Medallion Architecture (Bronze, Silver, Gold).

- **Source**: On-premises SQL Server
- **Intermediate Layers**: Azure Data Lake Storage (ADLS)
- **Destination**: Snowflake
- **Transformations**: Done using Azure Databricks (PySpark)
- **Pipeline Automation**: Executed via Azure Data Factory (ADF) pipelines

## Step-by-Step ADF Pipeline Design

### 1. Setup Linked Services in ADF

#### 1.1. Linked Service for SQL Server (Source)
- **Linked Service Type**: Azure SQL Server
- **Authentication**: SQL Authentication or Windows Authentication
- **Connection**: Establish using Self-Hosted Integration Runtime (SHIR)

#### 1.2. Linked Service for ADLS (Intermediate Layers)
- **Linked Service Type**: Azure Data Lake Storage Gen2
- **Authentication**: Managed Identity or Service Principal
- **Storage URL**: Your Azure Data Lake account URL

#### 1.3. Linked Service for Snowflake (Destination)
- **Linked Service Type**: Snowflake
- **Snowflake Configuration**:
  - Account: Snowflake account name
  - Warehouse: Target Snowflake warehouse
  - Database: Target Snowflake database
  - Schema: Snowflake schema for tables

### 2. ADF Pipeline Design

#### 2.1. Pipeline 1: Ingest Data from SQL Server to ADLS (Bronze Layer)

- **Pipeline Name**: `IngestFromSQLServerToADLS_Bronze`
- **Activity**: Copy Data
  - **Source**:
    - Linked Service: On-premises SQL Server
    - Table(s): `Patients`, `Doctors`, `Appointments`, etc.
    - Enable Incremental Loads: Use `modified_date` or `created_date` if available.
  - **Sink**:
    - Linked Service: ADLS Gen2
    - Path: `adl://yourstorageaccount.dfs.core.windows.net/bronze/{table_name}/{date_partition}/`
    - Format: **Parquet** or **CSV**

- **Trigger**: Schedule the pipeline to run daily/hourly based on your requirements.

#### 2.2. Pipeline 2: Transform Data from Bronze to Silver using Databricks

- **Pipeline Name**: `TransformBronzeToSilver`
- **Activity**: Databricks Notebook
  - **Linked Service**: Databricks Linked Service
  - **Notebook**: Databricks notebook for data transformation from Bronze to Silver.
    - Input Path: Bronze layer in ADLS (`adl://yourstorageaccount.dfs.core.windows.net/bronze/...`)
    - Transformations:
      - Data cleaning (e.g., handle missing values, remove duplicates).
      - Apply business rules and standards (e.g., normalize medical record formats).
    - Output Path: Silver layer (`adl://yourstorageaccount.dfs.core.windows.net/silver/...`)
    - Format: **Delta**

- **Trigger**: This pipeline is triggered after the ingestion pipeline finishes.

#### 2.3. Pipeline 3: Transform Data from Silver to Gold using Databricks

- **Pipeline Name**: `TransformSilverToGold`
- **Activity**: Databricks Notebook
  - **Linked Service**: Databricks Linked Service
  - **Notebook**: Databricks notebook for data transformation from Silver to Gold.
    - Input Path: Silver layer in ADLS (`adl://yourstorageaccount.dfs.core.windows.net/silver/...`)
    - Transformations:
      - Aggregation and enrichment of data.
      - Example: Combine patient, appointment, and billing data to analyze healthcare performance.
    - Output Path: Gold layer (`adl://yourstorageaccount.dfs.core.windows.net/gold/...`)
    - Format: **Delta**

- **Trigger**: This pipeline is triggered after the Silver transformation pipeline completes.

#### 2.4. Pipeline 4: Load Data from ADLS (Gold) to Snowflake

- **Pipeline Name**: `LoadGoldToSnowflake`
- **Activity**: Copy Data
  - **Source**:
    - Linked Service: ADLS Gen2 (Gold Layer)
    - Path: `adl://yourstorageaccount.dfs.core.windows.net/gold/{table_name}/`
    - Format: **Delta** or **Parquet**
  - **Sink**:
    - Linked Service: Snowflake
    - Destination: Snowflake tables (e.g., `patients`, `doctors`, `appointments`, etc.)
    - Configure Upserts: Use Snowflake `MERGE` or `INSERT` as needed

- **Trigger**: Triggered after the data has been transformed into the Gold layer.

---

### 3. Scheduling and Automation

- **ADF Triggers**: Set triggers to automate pipelines in sequence:
  - Start with the ingestion pipeline (`IngestFromSQLServerToADLS_Bronze`).
  - Followed by transformation pipelines (`TransformBronzeToSilver`, `TransformSilverToGold`).
  - End with the loading pipeline (`LoadGoldToSnowflake`).
  
- **Monitoring**: Use ADF’s built-in monitoring to track success/failure of each pipeline.

---

## Pipeline Workflow Summary

1. **IngestFromSQLServerToADLS_Bronze**: 
   - Copies raw data from on-premises SQL Server to ADLS Bronze layer.
   
2. **TransformBronzeToSilver**:
   - Cleanses and standardizes data from Bronze to Silver layer via Databricks.
   
3. **TransformSilverToGold**:
   - Aggregates and transforms Silver data to the final Gold layer via Databricks.
   
4. **LoadGoldToSnowflake**:
   - Loads the final transformed Gold data from ADLS to Snowflake for analytics.

---

- **Medallion Architecture**: 
  - **Bronze Layer**: Raw data from SQL Server.
  - **Silver Layer**: Cleansed and standardized data.
  - **Gold Layer**: Aggregated and enriched data for analysis.

- **Cost Considerations**: 
  - Optimize the pipeline schedules and resource usage to minimize Azure and Snowflake costs.
  
- **Scalability**: 
  - This architecture can be scaled to handle large datasets by configuring ADF’s parallel copy and Databricks cluster settings.

#### Actual Pipeline

{
  "Pipelines": [
    {
      "PipelineName": "IngestFromSQLServerToADLS_Bronze",
      "Activities": [
        {
          "ActivityType": "Copy Data",
          "Source": {
            "Type": "SQL Server",
            "LinkedService": "SQLServer_OnPremises",
            "Tables": [
              "Patients",
              "Doctors",
              "Appointments",
              "MedicalRecords",
              "Billing"
            ],
            "IncrementalLoad": {
              "Enabled": true,
              "Column": "ModifiedDate"
            }
          },
          "Sink": {
            "Type": "Azure Data Lake Storage Gen2",
            "LinkedService": "ADLS_Bronze",
            "Path": "adl://yourstorageaccount.dfs.core.windows.net/bronze/{table_name}/{year}/{month}/{day}/",
            "Format": "Parquet"
          },
          "Trigger": "Scheduled",
          "Frequency": "Daily"
        }
      ]
    },
    {
      "PipelineName": "TransformBronzeToSilver",
      "Activities": [
        {
          "ActivityType": "Databricks Notebook",
          "Source": {
            "Type": "Azure Data Lake Storage Gen2",
            "LinkedService": "ADLS_Bronze",
            "Path": "adl://yourstorageaccount.dfs.core.windows.net/bronze/{table_name}/{year}/{month}/{day}/",
            "Format": "Parquet"
          },
          "Transformation": {
            "Type": "Databricks",
            "Notebook": "BronzeToSilverTransformation",
            "Operations": [
              "Remove Duplicates",
              "Handle Missing Data",
              "Standardize Formats"
            ]
          },
          "Sink": {
            "Type": "Azure Data Lake Storage Gen2",
            "LinkedService": "ADLS_Silver",
            "Path": "adl://yourstorageaccount.dfs.core.windows.net/silver/{table_name}/{year}/{month}/{day}/",
            "Format": "Delta"
          },
          "Trigger": "After Completion",
          "DependsOn": "IngestFromSQLServerToADLS_Bronze"
        }
      ]
    },
    {
      "PipelineName": "TransformSilverToGold",
      "Activities": [
        {
          "ActivityType": "Databricks Notebook",
          "Source": {
            "Type": "Azure Data Lake Storage Gen2",
            "LinkedService": "ADLS_Silver",
            "Path": "adl://yourstorageaccount.dfs.core.windows.net/silver/{table_name}/{year}/{month}/{day}/",
            "Format": "Delta"
          },
          "Transformation": {
            "Type": "Databricks",
            "Notebook": "SilverToGoldTransformation",
            "Operations": [
              "Aggregation",
              "Join Data (Patients + Appointments)",
              "Business Logic Implementation"
            ]
          },
          "Sink": {
            "Type": "Azure Data Lake Storage Gen2",
            "LinkedService": "ADLS_Gold",
            "Path": "adl://yourstorageaccount.dfs.core.windows.net/gold/{table_name}/{year}/{month}/{day}/",
            "Format": "Delta"
          },
          "Trigger": "After Completion",
          "DependsOn": "TransformBronzeToSilver"
        }
      ]
    },
    {
      "PipelineName": "LoadGoldToSnowflake",
      "Activities": [
        {
          "ActivityType": "Copy Data",
          "Source": {
            "Type": "Azure Data Lake Storage Gen2",
            "LinkedService": "ADLS_Gold",
            "Path": "adl://yourstorageaccount.dfs.core.windows.net/gold/{table_name}/{year}/{month}/{day}/",
            "Format": "Delta"
          },
          "Sink": {
            "Type": "Snowflake",
            "LinkedService": "Snowflake_Target",
            "Database": "HealthcareDB",
            "Schema": "public",
            "TableMapping": {
              "Patients": "patients",
              "Doctors": "doctors",
              "Appointments": "appointments",
              "MedicalRecords": "medical_records",
              "Billing": "billing"
            }
          },
          "Transformation": {
            "Type": "Upsert",
            "PrimaryKey": "ID",
            "MergeLogic": {
              "WhenMatched": "Update",
              "WhenNotMatched": "Insert"
            }
          },
          "Trigger": "After Completion",
          "DependsOn": "TransformSilverToGold"
        }
      ]
    }
  ]
}


