
# Inventory Management System: Data Engineering Pipeline for Local to Cloud Migration in Azure

## Overview
This pipeline facilitates the migration of an inventory management system from a local database to a cloud-based solution in Azure. The goal is to ensure efficient, scalable, and real-time inventory tracking, data processing, and analytics.

## Architecture Components
1. **Local Database (SQLite)**
2. **Azure Data Lake Storage (ADLS)**
3. **Azure Data Factory (ADF)**
4. **Azure Synapse Analytics**
5. **Azure SQL Database or Cosmos DB**
6. **Azure Monitor** for logging and alerts

---
# Tables and Columns for Inventory Management System

## 1. **Products**
   - **Columns:**
     - ProductID (Primary Key)
     - ProductName
     - Category
     - QuantityAvailable
     - UnitPrice
     - SupplierID (Foreign Key)
     - RestockLevel
     - CreatedAt
     - UpdatedAt

## 2. **Suppliers**
   - **Columns:**
     - SupplierID (Primary Key)
     - SupplierName
     - ContactName
     - PhoneNumber
     - Email
     - Address
     - City
     - Country
     - CreatedAt
     - UpdatedAt

## 3. **PurchaseOrders**
   - **Columns:**
     - PurchaseOrderID (Primary Key)
     - SupplierID (Foreign Key)
     - OrderDate
     - DeliveryDate
     - TotalAmount
     - Status
     - CreatedAt
     - UpdatedAt

## 4. **SalesTransactions**
   - **Columns:**
     - TransactionID (Primary Key)
     - CustomerID (Foreign Key)
     - ProductID (Foreign Key)
     - TransactionDate
     - QuantitySold
     - TotalPrice
     - PaymentMethod
     - CreatedAt
     - UpdatedAt

## 5. **Customers**
   - **Columns:**
     - CustomerID (Primary Key)
     - CustomerName
     - Email
     - PhoneNumber
     - Address
     - City
     - Country
     - LoyaltyPoints
     - CreatedAt
     - UpdatedAt

## 6. **StockAdjustments**
   - **Columns:**
     - AdjustmentID (Primary Key)
     - ProductID (Foreign Key)
     - AdjustmentType (Increase/Decrease)
     - QuantityAdjusted
     - Reason
     - AdjustedBy
     - AdjustmentDate
     - CreatedAt
     - UpdatedAt

## 7. **InventoryLogs**
   - **Columns:**
     - LogID (Primary Key)
     - ProductID (Foreign Key)
     - QuantityBefore
     - QuantityAfter
     - ActionType (e.g., Sale, Restock, Adjustment)
     - ActionDate
     - PerformedBy
     - Notes
     - CreatedAt
     - UpdatedAt

## 8. **ECommerceOrders**
   - **Columns:**
     - OrderID (Primary Key)
     - Platform (e.g., Amazon, Shopify)
     - CustomerID (Foreign Key)
     - ProductID (Foreign Key)
     - OrderDate
     - QuantityOrdered
     - TotalPrice
     - OrderStatus
     - CreatedAt
     - UpdatedAt

## 9. **IoTDeviceLogs**
   - **Columns:**
     - LogID (Primary Key)
     - DeviceID (Foreign Key)
     - ProductID (Foreign Key)
     - EventType (e.g., Stock In, Stock Out)
     - Quantity
     - Timestamp
     - Location
     - CreatedAt
     - UpdatedAt

## 10. **CompetitorData**
   - **Columns:**
     - CompetitorID (Primary Key)
     - CompetitorName
     - ProductID (Foreign Key)
     - CompetitorPrice
     - StockAvailability
     - LastUpdated
     - CreatedAt
     - UpdatedAt




# Data Engineering Pipeline for Migrating 20GB Multi-Source Data to Azure

## **Pipeline Overview**
This pipeline describes the process for extracting, transforming, and loading (ETL) 20GB of data from Oracle SQL Server and other sources into Azure for scalable storage, processing, and analytics. It leverages Azure services for cloud-native integration and real-time capabilities.

---

## **Pipeline Steps**

### **1. Data Ingestion**
#### Sources:
- **Oracle SQL Server**
  - Transactional Data (e.g., sales, inventory, orders)
- **CSV/Excel Files**
  - Historical data, supplier records, and archived sales data.
- **APIs**
  - Real-time sales and inventory updates from POS systems or e-commerce platforms.
- **IoT Devices**
  - Sensor data like stock movements and warehouse environmental data.
- **Web Scraped Data**
  - Competitor pricing and stock data.

#### Tools & Techniques:
- Use **Azure Data Factory (ADF)** to extract data:
  - Use **Oracle SQL Connector** for structured database extraction.
  - Use **File System Connector** for CSV/Excel ingestion.
  - Use **REST API Connector** for API data.
  - Use **IoT Hub** for IoT device data streaming.

---

### **2. Data Staging in Azure Data Lake Storage (ADLS)**
- **Raw Zone:**
  - Store unprocessed data in a hierarchical structure by source and date.
  - Ensure source-specific folders: `oracle/`, `csv/`, `api/`, `iot/`.
- **File Format:**
  - Use **Parquet** or **Avro** for efficient storage and query optimization.
- **Compression:**
  - Apply Gzip or Snappy compression to reduce storage costs.

---

### **3. Data Transformation and Cleaning**
#### Tool: Azure Data Factory (ADF) with Mapping Data Flows
- **Tasks:**
  - Remove duplicates and invalid records.
  - Standardize column names, data types, and formats.
  - Handle missing values (e.g., impute, drop).
  - Join datasets (e.g., combining transaction data with product and customer data).
  - Filter and partition data for efficient downstream processing.

#### Outputs:
- **Transformed Zone in ADLS:**
  - Store cleaned and transformed data in a new zone for downstream processing.

---

### **4. Data Loading**
#### Destination Options:
1. **Azure SQL Database:**
   - For structured relational data like transactional records.
   - Use for operational analytics and integration with business applications.
   - **Tool:** Azure Data Factory with SQL Sink Connector.

2. **Azure Synapse Analytics:**
   - For large-scale analytics and data warehouse solutions.
   - Load data using **PolyBase** for high-performance ingestion.
   - Partition data by time (e.g., monthly, daily) for query optimization.

3. **Azure Cosmos DB:**
   - For semi-structured data like API responses or IoT device logs.
   - Use for real-time lookups and globally distributed applications.

---

### **5. Data Processing & Analytics**
#### Tools:
- **Azure Synapse Analytics:**
  - Perform batch processing and large-scale analytics.
  - Integrate with Power BI for visualization.
- **Azure Databricks:**
  - Use for advanced machine learning and data science workflows.
  - Handle unstructured data like web-scraped competitor data.

---

### **6. Real-Time Pipeline for Incremental Data**
- **Tools:**
  - Use **Event Hubs** to ingest real-time IoT or API data.
  - Process data with **Azure Stream Analytics**.
  - Store real-time processed data in **Cosmos DB** or **ADLS**.

---

### **7. Monitoring and Logging**
#### Tools:
- **Azure Monitor:**
  - Set up alerts for pipeline failures or performance issues.
  - Log execution metadata and statistics for all pipeline runs.
- **Azure Log Analytics:**
  - Store and query logs for diagnostics and troubleshooting.

---

### **8. Data Backup and Archival**
- **Azure Blob Storage:**
  - Archive raw and processed data for compliance and future reference.
- **Retention Policies:**
  - Define lifecycle policies to automatically move older data to cold storage.

---

### **Pipeline Diagram (Example Hierarchical Flow)**

```plaintext
[Data Sources] --> [Azure Data Factory] --> [Azure Data Lake (Raw Zone)]
                   --> [Transform & Clean in ADF] --> [Azure Data Lake (Transformed Zone)]
                   --> [Load to Azure SQL Database / Synapse / Cosmos DB]
                   --> [Real-Time Data to Cosmos DB via Event Hubs]
                   --> [Monitor & Log Pipeline in Azure Monitor]
                   --> [Backup & Archive in Azure Blob Storage]


## Pipeline Steps

### 1. Data Ingestion Pipeline
   - **Goal**: Collect data from local databases and systems for storage in Azure.
   - **Process**:
     1. **Export Data**: Use scripts or tools to extract data from the local SQLite database.
     2. **Data Transfer**: Utilize **Azure Data Factory** (ADF) to automate and schedule data transfers to Azure Data Lake Storage (ADLS).
     3. **Data Storage**: Store raw data in ADLS for future processing.

### 2. Data Processing Pipeline
   - **Goal**: Process and clean data for further use in the cloud.
   - **Process**:
     1. **Data Preparation**: Use **ADF** or **Azure Databricks** to clean, transform, and format data in a suitable structure for analytics.
     2. **Data Transformation**: Apply business rules to ensure data consistency and quality across inventory items, vendors, and sales records.
     3. **Storage**: Store the processed data in a structured format in **Azure SQL Database** or **Azure Synapse Analytics** for reporting and analysis.

### 3. Real-time Streaming Pipeline (Optional)
   - **Goal**: Process real-time data (e.g., sales transactions) for up-to-date inventory levels.
   - **Process**:
     1. **Event Hubs / IoT Hub**: Capture real-time sales or stock updates.
     2. **Stream Analytics**: Use **Azure Stream Analytics** to process real-time data and send updates to the database.

### 4. Data Analytics and Reporting
   - **Goal**: Enable analytics and visualization for decision-making.
   - **Process**:
     1. **Data Aggregation**: Use **Azure Synapse** to run aggregation queries on inventory data.
     2. **Power BI Integration**: Connect **Power BI** with Azure SQL or Synapse to build dashboards for inventory trends, stock levels, and demand forecasting.

### 5. Monitoring and Alerting
   - **Goal**: Ensure pipeline health and data accuracy.
   - **Process**:
     1. **Azure Monitor**: Set up logs and metrics for pipeline activities.
     2. **Alerts**: Configure alerts for issues like data transfer failures or unusual inventory levels.

---

## Tools Summary

- **Azure Data Lake Storage (ADLS)**: Scalable data storage for raw data.
- **Azure Data Factory (ADF)**: Orchestrate data migration and processing tasks.
- **Azure Synapse Analytics**: For complex queries and analytics at scale.
- **Azure SQL Database**: Store structured data for easy access.
- **Azure Stream Analytics (optional)**: Real-time data processing.
- **Power BI**: Visualize data and create reports.
- **Azure Monitor**: Track and alert on pipeline and system health.

---

## Benefits
- **Scalability**: Ability to handle increasing data volumes.
- **Real-time Insights**: Accurate and up-to-date information on inventory.
- **Resilience**: Robust alerting and monitoring ensure system health.
- **Flexibility**: Azure services provide adaptable solutions for future needs.



##### 




