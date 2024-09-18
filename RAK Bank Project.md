
# RAK Bank Azure Data Migration Project

## Introduction

RAK Bank, one of the leading financial institutions in the UAE, undertook a major initiative to modernize its data infrastructure by migrating its on-premise data systems to Microsoft Azure. The objective of this project was to enhance scalability, improve data processing speed, and reduce the overhead of maintaining legacy systems. This migration aligned with RAK Bank’s strategic focus on digital transformation and harnessing cloud capabilities to optimize operations, enable advanced analytics, and ensure data security.

## Project Scope

The migration project encompassed the following key areas:
- **Data Sources**: Migration of structured and unstructured data from multiple on-premise systems including customer databases, transaction logs, financial reports, and other core banking systems.
- **Azure Services Utilized**:
  - Azure Data Lake Storage (ADLS) for secure and scalable data storage.
  - Azure Synapse Analytics for advanced analytics and data warehousing.
  - Azure Data Factory (ADF) for orchestrating ETL/ELT pipelines.
  - Azure SQL Database and Cosmos DB for hosting transactional and non-relational data.
  - Azure Active Directory (AAD) for access control and security.
- **ETL Process**: Custom ETL pipelines were developed using Azure Data Factory to ensure seamless data migration with transformations and quality checks in place.
- **Data Security**: End-to-end encryption, role-based access, and multi-factor authentication were implemented to comply with UAE banking regulations and ensure data integrity during and after migration.

## Challenges and Solutions

- **Challenge 1: Data Compatibility**
  - **Solution**: Data from legacy systems was transformed into Azure-compatible formats using Azure Data Factory’s mapping data flows and Databricks for complex data transformation logic.
  
- **Challenge 2: Data Volume**
  - **Solution**: Azure Data Lake was leveraged to store and process large volumes of data, ensuring cost-effective and scalable storage solutions.
  
- **Challenge 3: Minimal Downtime**
  - **Solution**: The migration was executed in phases using a hybrid cloud approach. Critical systems were prioritized, and fallback strategies were implemented to minimize disruption to banking operations.

## Key Features

- **High Scalability**: By migrating to Azure, RAK Bank achieved high scalability to handle increased customer demand and data growth.
- **Advanced Analytics**: Azure Synapse Analytics provided advanced data analytics capabilities, enabling the bank to generate insights from both structured and unstructured data.
- **Improved Security**: With Azure’s built-in security features such as encryption and AAD, data security and compliance were greatly enhanced.
  
## Outcome

The successful completion of the Azure data migration project resulted in:
- Reduced operational costs due to decreased reliance on on-premise infrastructure.
- Faster data processing times, leading to improved customer services.
- The ability to harness real-time analytics for decision-making processes.
- Enhanced data security measures to protect sensitive customer information.



# RAK Bank Azure Data Migration Project - Schema Diagram

## High-Level Schema

The RAK Bank Azure Data Migration project contains the following key entities:

1. **Customers**
2. **Accounts**
3. **Transactions**
4. **Loans**
5. **Cards**
6. **Branches**
7. **Employees**
8. **Audit**

These entities are interconnected through various relationships such as foreign keys (FKs) to represent the logical flow of data within the bank's system.

---

### Customers Table

| Column Name          | Data Type      | Description                                    |
|----------------------|----------------|------------------------------------------------|
| `customer_id`         | INT (PK)       | Unique identifier for the customer             |
| `first_name`          | VARCHAR(100)   | First name of the customer                     |
| `last_name`           | VARCHAR(100)   | Last name of the customer                      |
| `email`               | VARCHAR(255)   | Email address of the customer                  |
| `phone_number`        | VARCHAR(20)    | Phone number of the customer                   |
| `date_of_birth`       | DATE           | Customer's date of birth                       |
| `address`             | VARCHAR(255)   | Address of the customer                        |
| `city`                | VARCHAR(100)   | City of the customer                           |
| `country`             | VARCHAR(100)   | Country of the customer                        |
| `national_id`         | VARCHAR(50)    | National ID number                             |
| `customer_since`      | DATE           | The date the customer joined                   |
| `customer_status`     | VARCHAR(50)    | Active/Inactive status                         |

---

### Accounts Table

| Column Name          | Data Type      | Description                                    |
|----------------------|----------------|------------------------------------------------|
| `account_id`          | INT (PK)       | Unique identifier for the account              |
| `customer_id`         | INT (FK)       | Reference to the customer                      |
| `account_type`        | VARCHAR(50)    | Type of the account (e.g., Savings, Current)   |
| `balance`             | DECIMAL(15,2)  | Current balance of the account                 |
| `currency`            | VARCHAR(10)    | Currency of the account (e.g., AED, USD)       |
| `branch_id`           | INT (FK)       | Branch where the account is held               |
| `account_status`      | VARCHAR(50)    | Active/Inactive status                         |
| `date_opened`         | DATE           | Date the account was opened                    |

---

### Transactions Table

| Column Name           | Data Type      | Description                                    |
|-----------------------|----------------|------------------------------------------------|
| `transaction_id`       | INT (PK)       | Unique identifier for the transaction          |
| `account_id`           | INT (FK)       | Reference to the account                       |
| `transaction_type`     | VARCHAR(50)    | Type of transaction (e.g., Credit, Debit)      |
| `amount`               | DECIMAL(15,2)  | Transaction amount                             |
| `transaction_date`     | DATE           | Date of the transaction                        |
| `description`          | VARCHAR(255)   | Transaction description                        |
| `balance_after_txn`    | DECIMAL(15,2)  | Account balance after the transaction          |
| `branch_id`            | INT (FK)       | Branch where the transaction occurred          |

---

### Loans Table

| Column Name           | Data Type      | Description                                    |
|-----------------------|----------------|------------------------------------------------|
| `loan_id`              | INT (PK)       | Unique identifier for the loan                 |
| `customer_id`          | INT (FK)       | Reference to the customer                      |
| `loan_type`            | VARCHAR(50)    | Type of loan (e.g., Personal, Home, Auto)      |
| `loan_amount`          | DECIMAL(15,2)  | Total loan amount                              |
| `interest_rate`        | DECIMAL(5,2)   | Interest rate on the loan                      |
| `loan_status`          | VARCHAR(50)    | Loan status (e.g., Approved, Pending, Closed)  |
| `loan_start_date`      | DATE           | Date the loan was issued                       |
| `loan_end_date`        | DATE           | Date the loan is to be repaid                  |

---

### Cards Table

| Column Name           | Data Type      | Description                                    |
|-----------------------|----------------|------------------------------------------------|
| `card_id`              | INT (PK)       | Unique identifier for the card                 |
| `customer_id`          | INT (FK)       | Reference to the customer                      |
| `card_type`            | VARCHAR(50)    | Type of card (e.g., Credit, Debit)             |
| `card_number`          | VARCHAR(16)    | Card number (masked for security)              |
| `card_status`          | VARCHAR(50)    | Card status (e.g., Active, Blocked)            |
| `expiry_date`          | DATE           | Expiry date of the card                        |
| `issue_date`           | DATE           | Date the card was issued                       |
| `credit_limit`         | DECIMAL(15,2)  | Credit limit (if applicable)                   |

---

### Branches Table

| Column Name           | Data Type      | Description                                    |
|-----------------------|----------------|------------------------------------------------|
| `branch_id`            | INT (PK)       | Unique identifier for the branch               |
| `branch_name`          | VARCHAR(100)   | Name of the branch                             |
| `branch_code`          | VARCHAR(10)    | Branch code for transactions                   |
| `city`                 | VARCHAR(100)   | City where the branch is located               |
| `country`              | VARCHAR(100)   | Country where the branch is located            |
| `phone_number`         | VARCHAR(20)    | Contact number of the branch                   |

---

### Employees Table

| Column Name           | Data Type      | Description                                    |
|-----------------------|----------------|------------------------------------------------|
| `employee_id`          | INT (PK)       | Unique identifier for the employee             |
| `first_name`           | VARCHAR(100)   | Employee's first name                          |
| `last_name`            | VARCHAR(100)   | Employee's last name                           |
| `position`             | VARCHAR(50)    | Job title of the employee                      |
| `branch_id`            | INT (FK)       | Branch where the employee works                |
| `email`                | VARCHAR(255)   | Contact email of the employee                  |
| `phone_number`         | VARCHAR(20)    | Employee's contact number                      |
| `hire_date`            | DATE           | Date the employee was hired                    |
| `salary`               | DECIMAL(10,2)  | Employee's salary                              |

---

### Audit Table

| Column Name           | Data Type      | Description                                    |
|-----------------------|----------------|------------------------------------------------|
| `audit_id`             | INT (PK)       | Unique identifier for the audit                |
| `table_name`           | VARCHAR(100)   | Name of the table being audited                |
| `operation_type`       | VARCHAR(50)    | Type of operation (Insert, Update, Delete)     |
| `performed_by`         | VARCHAR(100)   | Name or ID of the user who performed the operation |
| `operation_time`       | TIMESTAMP      | Time the operation was performed               |
| `details`              | JSON           | Detailed log of changes                        |

---

## Extended Tables for 200 Columns

To complete the schema with 200 columns, the following additional tables can be included:

### Customer Contacts Table

| Column Name           | Data Type      | Description                                    |
|-----------------------|----------------|------------------------------------------------|
| `contact_id`           | INT (PK)       | Unique identifier for the contact              |
| `customer_id`          | INT (FK)       | Reference to the customer                      |
| `emergency_contact_name` | VARCHAR(100) | Name of the emergency contact                  |
| `emergency_contact_number` | VARCHAR(20) | Emergency contact number                      |

---

### Loan Payments Table

| Column Name           | Data Type      | Description                                    |
|-----------------------|----------------|------------------------------------------------|
| `payment_id`           | INT (PK)       | Unique identifier for the payment              |
| `loan_id`              | INT (FK)       | Reference to the loan                          |
| `payment_amount`       | DECIMAL(15,2)  | Amount of the loan payment                     |
| `payment_date`         | DATE           | Date of the payment                            |
| `payment_status`       | VARCHAR(50)    | Status of the payment (e.g., Completed, Pending) |

---

### Transaction Fees Table

| Column Name           | Data Type      | Description                                    |
|-----------------------|----------------|------------------------------------------------|
| `fee_id`               | INT (PK)       | Unique identifier for the fee                  |
| `transaction_id`       | INT (FK)       | Reference to the transaction                   |
| `fee_amount`           | DECIMAL(15,2)  | Fee amount for the transaction                 |
| `fee_type`             | VARCHAR(50)    | Type of fee (e.g., Service Fee, Penalty)       |
| `description`          | VARCHAR(255)   | Description of the fee                         |

---



