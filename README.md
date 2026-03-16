
Databricks Metadata Table Resolver Automation
Overview
This project is a Databricks-based automation utility that resolves source table names provided in an Excel file into their fully qualified Databricks table paths using metadata from system.information_schema.
The automation significantly reduces manual effort involved in searching and validating table locations across multiple Databricks catalogs. What previously required days of manual lookup is now completed in seconds, improving delivery speed, accuracy, and team productivity.
________________________________________
Problem Statement
Teams often receive input files containing logical or source system table names without complete Databricks paths. Manually:
•	Searching across multiple catalogs
•	Validating schemas
•	Handling special naming conventions
is time consuming and error prone.
This tool automates the entire process.
________________________________________
Solution
The automation:
1.	Accepts an Excel file with source table names
2.	Queries Databricks system metadata for tables and views
3.	Matches source names against available metadata
4.	Applies custom parsing rules for special naming conventions
5.	Generates an output Excel file with:
•	Fully qualified table names
•	Visual indicators for missing tables
________________________________________
Key Features
•	Supports multiple Databricks catalogs
•	Resolves both tables and views
•	Handles comma separated multiple table inputs
•	Custom parsing logic for prefixed table names
•	Highlights unresolved entries in red
•	Parameterized using Databricks widgets
•	Excel based input and output (business friendly)
________________________________________
Architecture
Excel Input
   ↓
Pandas DataFrame
   ↓
Databricks system.information_schema
   ↓
Metadata Matching Engine
   ↓
Excel Output (with formatting)
________________________________________
Input Format
Excel File Example
Source_Table
SRC_SCHEMA_TABLE_A
SRC_SCHEMA_TABLE_B, SRC_SCHEMA_TABLE_C
________________________________________
Output Format
Source_Table	Resolved_Databricks_Table
SRC_SCHEMA_TABLE_A	dev_data_db.core_schema.table_a
SRC_SCHEMA_TABLE_B	NOT FOUND
•	Rows with unresolved tables are highlighted in red
________________________________________
Technologies Used
•	Python
•	Apache Spark (Databricks)
•	Pandas
•	OpenPyXL
•	Databricks Widgets
•	Databricks system.information_schema
________________________________________
Configuration
All environment specific values are parameterized.
python
dbutils.widgets.text("layer", "")
dbutils.widgets.text("env", "")
Catalogs are dynamically constructed based on environment:
python
db_list = [
    f"{env}_core_db",
    f"{env}_integration_db",
    f"{env}_analytics_db"
]
________________________________________
Matching Logic
The resolver follows a layered approach:
1.	Direct Match
•	Attempts to match source name to catalog.schema.table
2.	Prefix Based Parsing
•	Parses structured source names to derive schema/table
3.	Catalog Scan
•	Searches across allowed catalogs
4.	Fallback
•	Marks unresolved entries as NOT FOUND
________________________________________
Visual Indicators
•	Cells containing NOT FOUND are automatically formatted in red
•	Helps users quickly identify missing or invalid mappings
________________________________________
Benefits & Impact
•	Reduced manual lookup effort from days to seconds
•	Improved accuracy and consistency
•	Eliminated dependency on tribal knowledge
•	Scalable across environments and teams
•	Business friendly Excel interface
________________________________________
Security & Data Privacy
To ensure data confidentiality:
•	All client specific catalogs, schemas, and table names are abstracted
•	No sensitive data is stored or logged
•	Metadata access is read only
•	The repository contains no production identifiers
________________________________________
How to Run
1.	Upload the input Excel file
2.	Set Databricks widget values:
•	env
•	layer
3.	Run the notebook/script
4.	Download the generated output Excel file
________________________________________
Future Enhancements
•	Add logging and audit metrics
•	Support for CSV input/output
•	Fuzzy matching for near matches
•	Web UI wrapper
•	CI/CD integration
________________________________________
Author
Harsh Shiwale
Data Engineering / Automation
Cognizant


