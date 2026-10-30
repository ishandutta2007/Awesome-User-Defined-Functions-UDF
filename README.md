# Awesome-User-Defined-Functions-UDF

## Top User-Defined Functions (UDF) Ecosystem



**Curated List of SaaS Products & Open-Source GitHub Projects**  

*Focused on Extensible SQL Engines, Custom Function Runtimes & Self-Hosted Extension Frameworks*  

**Last updated: October 2026**



This repository tracks notable **SaaS platforms** and **open-source projects** for **User-Defined Functions (UDF)**. These tools enable organizations to extend database and query engine capabilities with custom logic—written in SQL, Python, JavaScript, Java, or C++—allowing teams to encapsulate business rules, integrate external services, and perform specialized computations directly within their data pipelines.



**Examples** include Snowflake UDFs, Databricks Spark UDFs, Google Cloud BigQuery UDFs, AWS Redshift UDFs, ClickHouse UDFs, SingleStore UDFs, Teradata UDFs, DuckDB Extensions, Postgres Extensions, and Trino UDFs (the category leaders).



**Open-source emphasis**: The UDF landscape is **intrinsically tied to the host engine rather than a standalone product category**. There is no "open-source UDF platform" that replaces Snowflake or BigQuery—instead, open-source engines like **DuckDB**, **PostgreSQL**, **ClickHouse**, and **Trino** each provide their own extension mechanisms with varying flexibility. **DuckDB** offers a rich official extension ecosystem (spatial, httpfs, iceberg, postgres, vss) that functions as a lightweight, embeddable analytics engine with extensibility . **ClickHouse** supports executable UDFs via XML/YAML configuration with Python or shell scripts reading from STDIN . **PostgreSQL** extensions (via `pg_stat_statements`, `pgcrypto`, `postgis`, etc.) remain the gold standard for mature, community-maintained extensibility . The critical distinction: **SaaS UDFs are managed and sandboxed** (Snowflake, BigQuery), while **open-source UDFs require you to own the runtime, security, and deployment**. For teams seeking a fully managed UDF experience with Python/Java/JS support, commercial warehouses lead; for teams wanting to embed custom logic in a self-hosted engine, DuckDB and PostgreSQL offer the most practical paths.



Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.



## Table of Contents

- [SaaS/Hosted Platforms](#saas-products)

- [Open-Source GitHub Projects](#open-source-github-projects)

- [How to Contribute](#how-to-contribute)

- [Disclaimer](#disclaimer)



## SaaS/Hosted Platforms



- **[Snowflake User-Defined Functions (UDFs)](https://docs.snowflake.com/)**  

  Supports scalar UDFs in SQL, JavaScript, Python, Java, and Scala; VOLATILE scalar UDFs are now supported with dynamic table incremental refresh (GA July 2026) in INCREMENTAL, AUTO, and ADAPTIVE modes, limited to the SELECT clause . Also supports Python UDTFs (user-defined table functions) with partition-aware processing via `__init__` and `end_partition` methods . Fully managed with secure sandboxing.



- **[Databricks Spark UDFs](https://docs.databricks.com/)**  

  Python UDFs and Pandas UDFs supported in Databricks Runtime 13.3 LTS+ for all access modes; Unity Catalog registration available via SQL syntax in Runtime 14.1+ . Supports Variant type return values (scalar, struct, array, map) in Runtime 14.1+. **Critical caveat**: Spark SQL does not guarantee subexpression evaluation order—UDFs relying on short-circuit null checks are dangerous; embed null handling inside the UDF itself .



- **[Google Cloud BigQuery UDFs](https://cloud.google.com/bigquery/docs/user-defined-functions)**  

  Supports SQL, JavaScript, and Python UDFs, plus UDAFs (aggregate), table functions, remote functions, and stored procedures . Python UDFs can access external Google Cloud services via Cloud resource connections and install PyPI libraries. JavaScript UDFs consume more slot resources than SQL—prefer SQL when possible . Community-contributed UDFs available in `bigquery-public-data.persistent_udfs` and the `bigquery-utils` GitHub repo .



- **[AWS Redshift UDFs](https://docs.aws.amazon.com/redshift/)**  

  **Critical migration notice**: Amazon Redshift stopped supporting creation of new scalar Python UDFs after **October 30, 2025**; existing Python UDFs continue to function but are deprecated . **Lambda UDFs are the recommended replacement**—they can connect to external APIs, use isolated Lambda compute resources (no impact on Redshift query performance), support multiple Python runtimes, and simplify permission management .



- **[ClickHouse UDFs](https://clickhouse.com/docs/)**  

  Executable UDFs configured via XML/YAML files specifying a command (Python, shell script, or executable) that reads arguments from STDIN and writes results to STDOUT . Supports JSONEachRow format for multiple return values, constant parameters in the command (with `execute_direct` to prevent shell injection), and Python scripts with named arguments. **Self-managed**—you own the script deployment and security.



- **[SingleStore UDFs](https://docs.singlestore.com/)**  

  Python UDFs via SingleStore Notebooks (Private Preview, March 2026) . Define functions with `@udf` annotation (e.g., sentiment analysis via NLTK), start a UDF server with `apps.run_udf_app()`, and call from SQL. **Notable**: live logs for debugging, and the UDF server runs as a separate app—compute-heavy UDFs do not impact database performance. Requires Private Preview access.



- **[Teradata UDFs](https://docs.teradata.com/)**  

  SQL UDFs via `CREATE FUNCTION`; C/C++/Java UDFs via the External Routine Installation Tool (`tdextroutine`) for VantageCloud Lake . Requires a manifest.json file listing source files and properties. **DBC user cannot create UDFs**; requires explicit GRANT privileges. Migration tool (`XRMigrationTool`) converts legacy BTEQ scripts to the new deployment format .



- **[DuckDB Extensions](https://duckdb.org/docs/extensions/overview)**  

  Official extensions function as the UDF mechanism: `spatial` (geospatial), `httpfs` (HTTP/S3), `postgres` (read/write Postgres), `iceberg` (Apache Iceberg), `vss` (vector similarity search), `json`, `icu`, `parquet` (built-in), and more . Most are autoloadable—DuckDB fetches them on first use. **The most practical open-source UDF-equivalent** for embedded analytics.



- **[PostgreSQL Extensions](https://www.postgresql.org/docs/current/extension.html)**  

  The most mature extension ecosystem: `pg_stat_statements` (query stats), `pgcrypto` (cryptographic functions), `postgis` (geospatial), `hstore` (key-value), `ltree` (hierarchical trees), `pg_graphql` (GraphQL), `pg_jsonschema` (JSON schema validation), `pg_hint_plan` (optimizer hints), and hundreds more . Extensions are installed via `CREATE EXTENSION` and often require OS-level packages.



- **[Trino UDFs](https://trino.io/docs/current/develop/functions.html)**  

  Custom functions packaged as plugins (ZIP with `Plugin` implementation and `@ScalarFunction` classes); deployed by placing the ZIP in the plugin directory and restarting Trino . **Requires restart**—no hot-loading. Some commercial Trino distributions (e.g., 数栈 Trino 385) add a function management UI for uploads, but open-source Trino lacks this .



## Open-Source GitHub Projects



### Embeddable Analytics Engines with Extension Support



- **[DuckDB](https://github.com/duckdb/duckdb)**  

  The most practical open-source engine for UDF-like extensibility (MIT). Official extensions cover spatial, HTTP/S3, Postgres, Iceberg, vector search, and more—most are autoloadable . DuckDB also supports native UDFs via its C++ API and Python `create_function`. **Best for embedded analytics** where you need custom logic without a server—runs in-process, single binary, zero configuration.



- **[ClickHouse](https://github.com/ClickHouse/ClickHouse)**  

  Open-source columnar OLAP engine (Apache 2.0) with executable UDFs via XML/YAML configuration . Scripts can be Python, shell, or any executable reading from STDIN. **Trade-off**: every UDF invocation spawns a process—high-frequency UDFs become I/O-bound. Best for occasional custom logic (parsing, external lookups) rather than per-row transformations.



- **[PostgreSQL](https://github.com/postgres/postgres)**  

  The foundational open-source relational database (PostgreSQL License) with the richest extension ecosystem in existence . Extensions range from cryptographic functions (`pgcrypto`) to geospatial (`postgis`), hierarchical data (`ltree`), and GraphQL (`pg_graphql`). **The standard for mature, production-proven extensibility**—but extensions often require OS-level compilation and superuser installation.



### Plugin Frameworks for Query Engines



- **[Trino](https://github.com/trinodb/trino)**  

  Open-source distributed SQL query engine (Apache 2.0) with a plugin-based UDF system . Package custom functions as a ZIP with a `Plugin` implementation and `@ScalarFunction` classes; deploy to the plugin directory and restart. **No hot-loading**—UDF deployment requires a restart. Best for teams already running Trino who need custom scalar functions in Java.



- **[Apache Calcite](https://github.com/apache/calcite)**  

  Open-source dynamic data management framework with a pluggable function registry; used by many engines (Flink, Hive, Drill) as their SQL parser/optimizer. **Infrastructure layer**—if you are building a query engine, Calcite provides the UDF registration hooks; if you are using an engine, you likely inherit Calcite's function capabilities.



### Additional Strong Open-Source Options



- **pgvector** — PostgreSQL extension for vector similarity search (PostgreSQL License); the most practical open-source vector search integration, implemented as a native extension rather than a separate database .

- **PostGIS** — The gold-standard geospatial extension for PostgreSQL; provides hundreds of spatial functions as native database functions.

- **DuckDB Spatial** — DuckDB's official geospatial extension; provides ST_* functions and spatial indexing within the embeddable engine .



**Frameworks for building custom systems**: For **embedded analytics with UDF-like extensibility**, DuckDB provides the most practical open-source path with autoloadable extensions . For **server-side OLAP with script-based UDFs**, ClickHouse supports Python/shell executable UDFs but with process-spawn overhead . For **mature, production-proven extensions**, PostgreSQL's ecosystem is unmatched . For **Java-based query engine plugins**, Trino provides a structured UDF plugin system . No open-source engine matches the managed, sandboxed, multi-language UDF experience of Snowflake or BigQuery—organizations requiring that depth should evaluate commercial warehouses.



## How to Contribute



1. Fork the repo.

2. Add/edit entries in `README.md` (follow existing format).

3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.

4. Submit PR with a short explanation.



Star the repo if you find it useful!



## Disclaimer



- This is a **community-curated** list — not exhaustive and not an endorsement.

- UDFs execute arbitrary code within database engines; **security implications are significant**—malicious or buggy UDFs can crash the engine, leak data, or compromise the host. ClickHouse explicitly warns about shell argument expansion vulnerabilities and recommends `execute_direct` . Redshift deprecated Python UDFs partly due to isolation and performance concerns .

- **UDF support is engine-specific and evolving**—Redshift is migrating away from Python UDFs toward Lambda UDFs ; Snowflake continues expanding UDF capabilities (VOLATILE UDFs with incremental refresh, GA July 2026) ; SingleStore's Python UDFs remain in Private Preview . Verify current status before architecting around a specific UDF feature.

- **No open-source UDF platform exists as a standalone product**—UDFs are a feature of the host engine. Organizations seeking managed UDFs with multi-language support and sandboxing should evaluate Snowflake, BigQuery, or Databricks; those wanting embedded extensibility should evaluate DuckDB or PostgreSQL extensions.



---



**Made for data engineers, analytics platform architects, and teams extending query engines with custom logic.**  

Let's make UDF ecosystems more open, but with clear eyes about the engine-specific constraints and security responsibilities that define each path.
