
# Azure Data Engineering Project

## Project Overview

This project establishes a comprehensive end-to-end data pipeline, spanning from an on-premise SQL Server to Azure Data Lake Gen2. It is designed to showcase a variety of Azure services and their integration for a seamless data processing and analytics workflow.

## Key Components

- **Azure Data Factory**: Utilized for efficient data ingestion from the on-premise SQL Server to Azure Data Lake Gen2.
- **Databricks**: Employed for robust and layered data processing.
- **Azure Synapse Analytics**: Used for advanced analytics on the processed data.
- **Power BI**: Integrated for dynamic reporting and visualization of insights.
- **Security**: Reinforced through the implementation of Azure Active Directory and Key Vault.

## Documentation

The project includes detailed documentation in the form of Notes (Word files). These documents provide a step-by-step guide on the project's execution, ensuring a comprehensive understanding of each phase.

## Detailed Project Flow

### 1. Data Ingestion

Data from the on-premise SQL Database was ingested into Azure Data Lake Gen 2 using Azure Data Factory (ADF).

```sql
-- Example SQL query to extract data from on-premise database
SELECT *
FROM Sales
WHERE OrderDate >= '2024-01-01'
```

This query is then used in an ADF pipeline to copy data to Azure Data Lake Gen2.

```json
{
    "name": "CopySalesDataPipeline",
    "properties": {
        "activities": [
            {
                "name": "CopySalesData",
                "type": "Copy",
                "inputs": [
                    {
                        "referenceName": "OnPremSqlDataset",
                        "type": "DatasetReference"
                    }
                ],
                "outputs": [
                    {
                        "referenceName": "AzureDataLakeGen2Dataset",
                        "type": "DatasetReference"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "SELECT * FROM Sales WHERE OrderDate >= '2024-01-01'"
                    },
                    "sink": {
                        "type": "ParquetSink",
                        "storeSettings": {
                            "type": "AzureBlobFSWriteSettings"
                        }
                    }
                }
            }
        ]
    }
}
```

### 2. Data Processing

The ingested data underwent transformation through Databricks, ensuring it was formatted and optimized for analysis.

```python
# Databricks PySpark code for data transformation
from pyspark.sql import SparkSession
from pyspark.sql.functions import *

# Create SparkSession
spark = SparkSession.builder.appName("SalesDataTransformation").getOrCreate()

# Read data from Data Lake
df = spark.read.parquet("abfss://container@storageaccount.dfs.core.windows.net/sales_data.parquet")

# Perform transformations
transformed_df = df.withColumn("TotalAmount", col("Quantity") * col("UnitPrice")) \
                   .withColumn("OrderYear", year(col("OrderDate"))) \
                   .withColumn("OrderMonth", month(col("OrderDate")))

# Write transformed data back to Data Lake
transformed_df.write.mode("overwrite").parquet("abfss://container@storageaccount.dfs.core.windows.net/transformed_sales_data.parquet")
```

### 3. Data Analysis and Reporting

The transformed data was loaded into a cloud database created within Azure Synapse Analytics. This database was then connected to Power BI for the creation of insightful visualizations and reports.

```sql
-- Synapse Analytics SQL script to create a view for reporting
CREATE VIEW SalesSummary AS
SELECT 
    OrderYear,
    OrderMonth,
    SUM(TotalAmount) AS TotalSales,
    COUNT(DISTINCT CustomerID) AS UniqueCustomers
FROM 
    dbo.TransformedSalesData
GROUP BY 
    OrderYear, OrderMonth;
```

This view can then be used in Power BI to create visualizations such as:

- Monthly sales trend chart
- Customer acquisition by month
- Top-selling products heatmap

## Security Measures

To ensure data security and compliance:

- Azure Active Directory (AAD) was implemented for authentication across all services.
- Sensitive information such as connection strings and access keys were stored in Azure Key Vault.
- Data encryption at rest and in transit was enabled for all storage and database services.

## Conclusion

This project demonstrates a robust, scalable, and secure data engineering pipeline using Azure services. It showcases the integration of various components to create a comprehensive solution for data ingestion, processing, analysis, and visualization.
