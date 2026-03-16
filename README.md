<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    
</head>
<body>

<h1>Databricks Metadata Table Resolver Automation</h1>

<h2>Overview</h2>
<p>
This project is a Databricks-based automation utility that resolves source table names
provided in an Excel file into their fully qualified Databricks table paths using metadata
from <code>system.information_schema</code>.
</p>
<p>
The automation significantly reduces manual effort involved in searching and validating
table locations across multiple Databricks catalogs. What previously required <strong>days of
manual lookup</strong> is now completed in <strong>seconds</strong>, improving delivery speed, accuracy,
and team productivity.
</p>

<h2>Problem Statement</h2>
<p>
Teams often receive input files containing logical or source system table names without
complete Databricks paths. Manually performing the following steps is time-consuming
and error-prone:
</p>
<ul>
    <li>Searching across multiple catalogs</li>
    <li>Validating schemas</li>
    <li>Handling special naming conventions</li>
</ul>
<p>
This tool automates the entire process.
</p>

<h2>Solution</h2>
<p>The automation:</p>
<ul>
    <li>Accepts an Excel file with source table names</li>
    <li>Queries Databricks system metadata for tables and views</li>
    <li>Matches source names against available metadata</li>
    <li>Applies custom parsing rules for special naming conventions</li>
    <li>Generates an output Excel file with:
        <ul>
            <li>Fully qualified table names</li>
            <li>Visual indicators for missing tables</li>
        </ul>
    </li>
</ul>

<h2>Key Features</h2>
<ul>
    <li>Supports multiple Databricks catalogs</li>
    <li>Resolves both tables and views</li>
    <li>Handles comma-separated multiple table inputs</li>
    <li>Custom parsing logic for prefixed table names</li>
    <li>Highlights unresolved entries in red</li>
    <li>Parameterized using Databricks widgets</li>
    <li>Excel-based input and output (business-friendly)</li>
</ul>

<h2>Architecture</h2>
<pre>
Excel Input
    ↓
Pandas DataFrame
    ↓
Databricks system.information_schema
    ↓
Metadata Matching Engine
    ↓
Excel Output (with formatting)
</pre>

<h2>Input Format</h2>
<p><strong>Excel File Example</strong></p>
<table border="1" cellpadding="5" cellspacing="0">
    <tr>
        <th>Source_Table</th>
    </tr>
    <tr>
        <td>SRC_SCHEMA_TABLE_A</td>
    </tr>
    <tr>
        <td>SRC_SCHEMA_TABLE_B, SRC_SCHEMA_TABLE_C</td>
    </tr>
</table>

<h2>Output Format</h2>
<table border="1" cellpadding="5" cellspacing="0">
    <tr>
        <th>Source_Table</th>
        <th>Resolved_Databricks_Table</th>
    </tr>
    <tr>
        <td>SRC_SCHEMA_TABLE_A</td>
        <td>dev_data_db.core_schema.table_a</td>
    </tr>
    <tr>
        <td>SRC_SCHEMA_TABLE_B</td>
        <td><strong>NOT FOUND</strong></td>
    </tr>
</table>
<p>
Rows with unresolved tables are highlighted in <strong>red</strong>.
</p>

<h2>Technologies Used</h2>
<ul>
    <li>Python</li>
    <li>Apache Spark (Databricks)</li>
    <li>Pandas</li>
    <li>OpenPyXL</li>
    <li>Databricks Widgets</li>
    <li>Databricks system.information_schema</li>
</ul>

<h2>Configuration</h2>
<p>All environment-specific values are parameterized.</p>

<pre>
dbutils.widgets.text("layer", "")
dbutils.widgets.text("env", "")
</pre>

<p>Catalogs are dynamically constructed based on environment:</p>

<pre>
db_list = [
    f"{env}_core_db",
    f"{env}_integration_db",
    f"{env}_analytics_db"
]
</pre>

<h2>Matching Logic</h2>
<ul>
    <li><strong>Direct Match:</strong> Attempts to match source name to <code>catalog.schema.table</code></li>
    <li><strong>Prefix-Based Parsing:</strong> Parses structured source names to derive schema and table</li>
    <li><strong>Catalog Scan:</strong> Searches across allowed catalogs</li>
    <li><strong>Fallback:</strong> Marks unresolved entries as <code>NOT FOUND</code></li>
</ul>

<h2>Visual Indicators</h2>
<ul>
    <li>Cells containing <code>NOT FOUND</code> are automatically formatted in red</li>
    <li>Helps users quickly identify missing or invalid mappings</li>
</ul>

<h2>Benefits &amp; Impact</h2>
<ul>
    <li>Reduced manual lookup effort from days to seconds</li>
    <li>Improved accuracy and consistency</li>
    <li>Eliminated dependency on tribal knowledge</li>
    <li>Scalable across environments and teams</li>
    <li>Business-friendly Excel interface</li>
</ul>

<h2>Security &amp; Data Privacy</h2>
<ul>
    <li>All client-specific catalogs, schemas, and table names are abstracted</li>
    <li>No sensitive data is stored or logged</li>
    <li>Metadata access is read-only</li>
    <li>The repository contains no production identifiers</li>
</ul>

<h2>How to Run</h2>
<ol>
    <li>Upload the input Excel file</li>
    <li>Set Databricks widget values:
        <ul>
            <li><code>env</code></li>
            <li><code>layer</code></li>
        </ul>
    </li>
    <li>Run the notebook or script</li>
    <li>Download the generated output Excel file</li>
</ol>

<h2>Future Enhancements</h2>
<ul>
    <li>Add logging and audit metrics</li>
    <li>Support for CSV input/output</li>
    <li>Fuzzy matching for near matches</li>
    <li>Web UI wrapper</li>
    <li>CI/CD integration</li>
</ul>

<h2>Author</h2>
<p>
<strong>Harsh Shiwale</strong><br>
Data Engineering / Automation<br>
Cognizant
</p>

</body>
</html>
