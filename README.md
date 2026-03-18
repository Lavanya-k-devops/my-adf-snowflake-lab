Project Documentation: Automated GitHub to Snowflake Data Pipeline
Engineer: Madhu Sudhanan

Stack: GitHub, Azure Data Factory (ADF), Azure Storage, Snowflake

Date: March 2026

1. Project Overview
The objective of this project was to build a functional, automated ETL/ELT pipeline that extracts raw CSV data from a version-controlled GitHub repository and loads it into a Snowflake Data Warehouse for analytics. The project incorporates modern DevOps practices, including branching, manual triggering, and automated scheduling.

2. Infrastructure Setup (Source & Destination)
Phase A: GitHub (Source Control)
Repository Creation: Established a private repository to host source data and track ADF code changes.

File Preparation: Created a sample_data.csv file containing structured records (e.g., EMP_ID, NAME, DEPT).

Branching Strategy: * Used a main branch for production-ready code.

Created a feature-snowflake-load branch for development and testing to prevent direct corruption of the master logic.

Phase B: Snowflake (Data Warehouse)
Compute Resources: Created a Virtual Warehouse (e.g., COMPUTE_WH) to provide the processing power for data loading.

Database Hierarchy: * Created a Database: POC_DB

Created a Schema: POC_SCHEMA

Table Creation: Defined a structured table GITHUB_DATA with data types matching the CSV file to ensure data integrity during the load.

SQL Example: CREATE TABLE GITHUB_DATA (EMP_ID INT, NAME STRING, DEPT STRING);

3. Azure Environment Configuration
Phase C: Azure Resources
Resource Group: Organized all assets under a single logical group for cost tracking.

Azure Data Factory (ADF): Provisioned the V2 Data Factory instance to act as the orchestration engine.

Azure Storage Account: Configured a storage account to act as a staging area or to store metadata if required by the pipeline logic.

Phase D: ADF Connectivity (Linked Services)
GitHub Integration: Connected ADF to the GitHub repository using a Personal Access Token (PAT). This allows ADF to "read" the data files and "write" its own JSON code back to the repo.

Snowflake Linked Service: Established a secure connection to the Snowflake instance using the account URL, warehouse name, and credentials.

4. Pipeline Development (The ETL Logic)
Phase E: Dataset & Activity Configuration
Source Dataset: Created a DelimitedText dataset pointing to the GitHub repository.

Sink Dataset: Created a Snowflake dataset pointing to the GITHUB_DATA table.

Copy Activity: * Source: Configured to fetch the specific CSV file from GitHub.

Sink: Configured to "Auto-create table" or "Append" data into Snowflake.

Mapping: Verified that the CSV headers correctly aligned with the Snowflake table columns.

5. Deployment & Execution (The "Publish vs Trigger" Workflow)
Phase F: Testing & Validation
Debug Run: Used the "Debug" feature to test the pipeline logic in a sandbox mode without needing to publish. This confirmed that data actually moved from GitHub to Snowflake.

Publishing: * Once validated, the code was "Published" to the Live Factory.

This step generates ARM templates and updates the adf_publish branch, making the pipeline "Official."

Phase G: Triggering Mechanisms
Manual Trigger (Trigger Now): Executed a one-time run to verify that the published logic works in the production environment.

Scheduled Trigger: * Configured a recurring schedule (e.g., every 15 minutes or daily).

This ensures that as new data is added to GitHub, it is automatically ingested into Snowflake without manual intervention.

6. Key Learnings & Troubleshooting
Environment Sync: Understood that "Publishing" is for code updates, while "Triggering" is for data movement.

CI/CD Awareness: Learned the importance of merging branches in GitHub before publishing in ADF to maintain a clean code history.

Error Handling: Identified that if a pipeline is triggered but not yet published, the system will return a "Pipeline not found" error.

Next Steps for Phase 2:

Implementing Dynamic Parameters to handle multiple files in a single loop.

Setting up Email Notifications via Logic Apps for failure alerts.
