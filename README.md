# UK Carbon Intensity ETL Pipeline

An ETL pipeline that pulls regional carbon-intensity data from the public UK Carbon Intensity API and loads it into PostgreSQL for analysis.

## What it does
- **Daily ETL (`xtd_carbon_etl.py`):** extracts the last 24 hours of data, flattens the nested JSON with pandas, aggregates it per region and loads it into PostgreSQL with SQLAlchemy.
- **Historical backfill (`historical/`):** downloads past data asynchronously with aiohttp, with a concurrency limit, retries and rate-limit (HTTP 429) handling. Raw JSON is saved to a bronze layer, then PySpark flattens and pivots it into silver and gold layers.
- **Data model (`carbon_xtdlabs.sql`):** a star schema with a region dimension and a fact table partitioned by year.

## Tools
Python, requests, aiohttp, pandas, PySpark, SQLAlchemy, PostgreSQL

## How to run
1. Create a `config.yaml` with your database details (never commit passwords).
2. Run `carbon_xtdlabs.sql` to create the tables.
3. Run `python xtd_carbon_etl.py`.
