# Watch Data Homepage
The goal of the project is to build a data flow from my smart watch to data lake.
This repository acts as the entrypoint into the project and describes each part.
The project is still being developed, and not all described steps are already done.

## Flow Philosophy
Firstly, the watch is connected to a phone through Gadgetbridge app that allows scheduled exports as SQLite and additional .fit and .gpx files.
Exported data is automatically sent to the landing bucket on the home server. This is where Apache Airflow picks it up on a schedule.
As Spark can't directly read tables from an SQLite database stored on S3, custom code is written and containerized to extract all tables into incremental parquet files.
Then, PySpark code instructs spark to move new data (using streaming with checkpoints) to Apache Iceberg tables in the bronze layer, following the medalion architecture.
Finally, dbt cleans all tables and pushes data through staging, silver and gold layers.

## Public Repositories
- [SQLite extractor](https://github.com/jamwojt/sqlite_extractor): Small Python container for getting parquet files from SQLite stored on S3
- [watch data dbt](https://github.com/jamwojt/watch_data_dbt): SQL code for transforming the data

