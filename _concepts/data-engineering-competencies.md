---
title: "Data engineering competencies"
group: data
last_modified_at: 2026-11-21
excerpt: "What a good data engineer should know at each stage of the data engineering lifecycle, and the technology that does the job on Databricks, Microsoft, AWS, Google Cloud, Snowflake and open source."
tags:
  - data-engineering
  - certifications
---
{% comment %}
AI DRAFT (Claude, 2026-11-21), for review. A reference page, so longer than
the usual concept target.

Every product link was opened on 2026-11-21 and the name is the one the
vendor's own page used that day. Vendors rename often (Google Cloud and
Databricks renamed several products in 2026), so re-check names when you
update the page. Links use kramdown reference syntax; definitions are at
the bottom of the file. Delete this comment when you're done.
{% endcomment %}

## In one sentence

A data engineer moves data from where it is created to where it is used,
reliably, securely and at a sensible cost.

## How to read this page

The page follows the data engineering lifecycle from *Fundamentals of Data
Engineering* by Joe Reis and Matt Housley: five stages, and six
undercurrents that run under every stage.

**Generation → Storage → Ingestion → Transformation → Serving**

*Undercurrents: security · data management · DataOps · data architecture ·
orchestration · software engineering*

Each stage has three parts:

- **Know:** the vendor-neutral competencies, drawn from the official
  outlines of eight data engineering certifications (listed at the end).
- **Probe:** a question to test the competency in an interview.
- **Technology:** what does the job on each platform, linked to the
  vendor's documentation, plus the open-source equivalent.

The concepts matter more than the product names. Someone who understands
incremental loading learns Auto Loader or Snowpipe in a day; someone who
only knows the product name does not.

## 1. Generation

Data is created in source systems the data engineer usually doesn't own.
The work here is understanding those systems and connecting to them.

**Know**
- Types of source systems: operational databases (OLTP), SaaS
  applications, APIs, event streams, files, logs, IoT devices
- How a source changes data: inserts, updates, deletes, and how to see
  them (update timestamps, or change data capture from the database log)
- Source limits: API rate limits, throttling, the load your queries put on
  a production database
- Source schemas change without warning; agree on them with the owning
  team (data contracts)
- Choosing between a managed connector, a community or custom connector,
  querying the source in place, and having the source push data to you

**Probe:** *"The sales team wants data from the CRM. What do you ask the
CRM team before you write any code?"*

**Technology**

| Platform | Operational stores | Connecting to sources |
|---|---|---|
| Databricks | [Lakebase][d-lakebase] (managed Postgres) | [Lakeflow Connect][d-lfc] managed connectors ([concepts][d-lfc-concepts]) for SaaS and databases; [community connectors][d-community] and [custom connectors][d-custom] (Beta), built on the [PySpark custom data source API][d-pyds] and run as declarative pipelines; [Zerobus Ingest][d-zerobus] for sources that push records directly into tables; [Lakehouse Federation][d-federation] to query sources in place |
| Microsoft | [SQL database in Fabric][m-sqldb], [Azure SQL Database][m-azsql], [Azure Cosmos DB][m-cosmos] | [Mirroring][m-mirroring] of operational databases into OneLake, [OneLake shortcuts][m-shortcuts], [Data Factory][m-datafactory] connectors |
| AWS | [RDS][a-rds], [Aurora][a-aurora], [DynamoDB][a-ddb] with [DynamoDB Streams][a-ddbstreams] | [AppFlow][a-appflow] for SaaS, [DMS][a-dms] for databases, [zero-ETL integrations][a-zeroetl] into Redshift |
| Google Cloud | [Cloud SQL][g-cloudsql], [AlloyDB][g-alloydb], [Spanner][g-spanner], [Bigtable][g-bigtable] | [Datastream][g-datastream] for database CDC, [BigQuery Data Transfer Service][g-dts] for SaaS and Google services |
| Snowflake | [Hybrid tables][s-hybrid], [Snowflake Postgres][s-postgres] | [Openflow][s-openflow] (managed connectors built on Apache NiFi) |
| Open source | PostgreSQL, MySQL | [Debezium][o-debezium] for CDC, [Airbyte][o-airbyte] and [dlt][o-dlt] for connectors |

## 2. Storage

Where data rests, at every stage. Storage choices decide the cost and speed
of everything that comes after.

**Know**
- Object storage as the foundation of the lakehouse
- Columnar file formats (Parquet) and open table formats (Delta Lake,
  Apache Iceberg): what they add, such as ACID transactions, time travel,
  schema enforcement, deletion vectors and change feeds
- Data layout: partitioning vs clustering (Z-order, liquid clustering),
  data skipping with file statistics, why small files hurt and how
  compaction fixes them
- Table maintenance: compaction (OPTIMIZE), removing old files (VACUUM),
  retention settings and their effect on time travel
- Managed vs external tables; governed storage for non-tabular files
- Lifecycle: retention, storage tiers, deletion, backups
- Choosing a store by access pattern: warehouse, lakehouse, key-value,
  relational, vector index

**Probe:** *"When would you partition a table, when would you cluster it,
and when neither?"*

**Technology**

| Platform | Technology to know |
|---|---|
| Databricks | **Formats:** [Delta Lake][d-delta], [Apache Iceberg][d-iceberg], [Iceberg reads on Delta tables (UniForm)][d-uniform]. **Tables and files:** [Unity Catalog managed tables][d-managed], [volumes][d-volumes]. **Layout:** [liquid clustering][d-clustering], [partitioning guidance][d-partitions], [data skipping and Z-order][d-skipping], [file size control (optimized writes, auto compaction)][d-filesize]. **Maintenance:** [OPTIMIZE][d-optimize], [VACUUM][d-vacuum], [predictive optimization][d-po], [deletion vectors][d-dv]. **History and changes:** [time travel and RESTORE][d-history], [change data feed][d-cdf] |
| Microsoft | [OneLake][m-onelake], [Lakehouse][m-lakehouse] (Delta tables and files), [Fabric Data Warehouse][m-warehouse], [Eventhouse][m-eventhouse] (KQL databases), [V-Order][m-vorder] write optimization; [Azure Data Lake Storage][m-adls] on Azure |
| AWS | [S3][a-s3] with [S3 Lifecycle][a-lifecycle], [S3 Tables][a-s3tables] (managed Iceberg), [Redshift][a-redshift], [DynamoDB][a-ddb], [OpenSearch Service][a-opensearch] |
| Google Cloud | [Cloud Storage][g-gcs] with [Object Lifecycle Management][g-lifecycle], [BigQuery][g-bq] ([partitioned][g-partition] and [clustered][g-cluster] tables), [BigLake][g-biglake], [Apache Iceberg managed tables][g-iceberg], [Bigtable][g-bigtable], [Spanner][g-spanner] |
| Snowflake | [Micro-partitions and clustering][s-micro], [clustering keys][s-clusterkeys], [Iceberg tables][s-iceberg], [Time Travel][s-timetravel], [Fail-safe][s-failsafe], [cloning][s-clone] |
| Open source | [Parquet][o-parquet], [Delta Lake][o-delta], [Apache Iceberg][o-iceberg] |

## 3. Ingestion

Getting data out of sources and into storage, once or continuously.

**Know**
- Full vs incremental loads; batch vs streaming, and when streaming is
  worth its cost
- Change data capture: reading inserts, updates and deletes from a
  database log
- File ingestion: detecting new files, schema inference, schema evolution,
  rescuing bad records
- Replayability: loading the same data twice must not create duplicates
- **Streaming:** event time vs processing time, windows, watermarks and
  late data, stateful processing, checkpoints and exactly-once recovery,
  message buses, fan-in and fan-out

**Probe:** *"Copy a 2 TB PostgreSQL table into the lakehouse and keep it
current every 15 minutes. Walk me through it."*

**Probe (streaming):** *"Your windowed aggregation's state keeps growing
and it double-counts after a restart. Fix both."*

**Technology**

| Platform | Files | Streaming and CDC |
|---|---|---|
| Databricks | [Auto Loader][d-autoloader], [COPY INTO][d-copyinto] | [Structured Streaming][d-sss] (Kafka, Kinesis, Pub/Sub sources), [Lakeflow Connect][d-lfc] database CDC, [Zerobus Ingest][d-zerobus] |
| Microsoft | [Copy job][m-copyjob] (full, incremental, CDC), [Data Factory pipelines][m-datafactory], [Dataflow Gen2][m-dataflow]; [Azure Data Factory][m-adf] | [Eventstreams][m-eventstreams], [mirroring][m-mirroring]; [Azure Event Hubs][m-eventhubs], [Azure Stream Analytics][m-asa] |
| AWS | [Glue][a-glue] jobs and crawlers, [Transfer Family][a-transfer] (SFTP) | [Kinesis Data Streams][a-kds], [Amazon Data Firehose][a-firehose], [MSK][a-msk] (Kafka), [DMS][a-dms] |
| Google Cloud | [Storage Transfer Service][g-sts], [BigQuery Data Transfer Service][g-dts], [Cloud Data Fusion][g-datafusion] | [Pub/Sub][g-pubsub], [Managed Service for Apache Kafka][g-kafka], [Dataflow][g-dataflow], [Datastream][g-datastream], [Database Migration Service][g-dms] |
| Snowflake | [Stages and bulk loading][s-loading], [COPY INTO][s-copy], [Snowpipe][s-snowpipe] | [Snowpipe Streaming][s-streaming], [Kafka connector][s-kafka], [Openflow][s-openflow] |
| Open source | [Spark][o-spark] | [Kafka][o-kafka], [Spark Structured Streaming][o-sss], [Flink][o-flink], [Beam][o-beam], [Debezium][o-debezium] |

## 4. Transformation

Turning raw data into something useful: cleaned, combined and modeled.

**Know**
- **SQL:** joins, window functions, aggregation, deduplication, pivoting,
  semi-structured data (JSON, nested arrays, VARIANT)
- **Distributed processing:** partitions, shuffles, data skew, disk spill,
  broadcast joins, adaptive query execution, file and partition pruning,
  reading the Spark UI or a query profile
- **Data modeling:** dimensional models (facts, dimensions, grain),
  normalization trade-offs, slowly changing dimensions type 1 and 2,
  medallion layers (bronze, silver, gold)
- **Incremental transformation:** MERGE (upsert), applying CDC
  downstream, materialized views vs streaming tables
- Declarative pipelines vs hand-written jobs
- Handling nulls, duplicates and late data during transformation

**Probe:** *"One task in a stage runs 20 times longer than the others. What
is happening, and what do you try first?"*

**Probe (modeling):** *"Model customer addresses so last year's sales
report still shows the address at the time of sale."*

**Technology**

| Platform | Technology to know |
|---|---|
| Databricks | **Pipelines:** [Spark Declarative Pipelines on Lakeflow][d-ldp], [AUTO CDC][d-autocdc] (replaces APPLY CHANGES; SCD type 1 and 2), [materialized views][d-mv], [streaming tables][d-st], [Lakeflow Designer][d-designer] (visual data prep). **SQL and data:** [MERGE][d-merge], [VARIANT][d-variant]. **Engine performance:** [Photon][d-photon], [adaptive query execution][d-aqe], [dynamic file pruning][d-dfp], [predictive I/O][d-pio], [disk cache][d-diskcache] (formerly Delta cache) |
| Microsoft | [Notebooks][m-notebooks] on [Spark compute][m-spark], [materialized lake views][m-mlv], T-SQL in the [Warehouse][m-warehouse], KQL in the [Eventhouse][m-eventhouse], [Dataflow Gen2][m-dataflow]; [Azure Databricks][m-adb] on Azure |
| AWS | [Glue][a-glue] Spark jobs, [Glue DataBrew][a-databrew] (visual), [EMR][a-emr], [Redshift materialized views][a-rsmv], [Athena][a-athena], [Lambda][a-lambda] for small jobs |
| Google Cloud | [BigQuery][g-bq] SQL, [Dataform][g-dataform], [Dataflow][g-dataflow], [Managed Service for Apache Spark][g-spark] (formerly Dataproc) and its [serverless option][g-sparkserverless], [BigQuery continuous queries][g-continuous] |
| Snowflake | [Dynamic tables][s-dt], [streams][s-streams] and [tasks][s-tasks], [Snowpark][s-snowpark] (Python, Java, Scala), [MERGE][s-merge], [dbt Projects on Snowflake][s-dbt] |
| Open source | [Spark][o-spark], [dbt][o-dbt] (models, incremental strategies, snapshots for SCD type 2), [Trino][o-trino], [DuckDB][o-duckdb] |

## 5. Serving

Delivering data to the people and systems that use it.

**Know**
- **Analytics:** gold tables, views and materialized views for BI;
  semantic layers and governed metric definitions; query performance for
  many concurrent users
- **Machine learning and AI:** preparing features; calling LLMs inside
  pipelines for enrichment and classification; preparing unstructured data
  for embeddings and retrieval-augmented generation (RAG); vector indexes
- **Sharing:** publishing data to other teams or organizations without
  copying it; clean rooms for analysis without exposing raw data
- **Applications and reverse ETL:** serving data back to operational
  tools and apps

**Probe:** *"Classify 10 million support tickets by topic inside the
pipeline. What do you watch out for?"*

**Technology**

| Platform | Analytics and BI | AI and ML | Sharing and apps |
|---|---|---|---|
| Databricks | [SQL warehouses][d-sqlwh], [metric views][d-metricviews], [AI/BI dashboards][d-dashboards], [Genie][d-genie] | [AI Functions][d-aifunc] (ai_query and task functions), [AI Search][d-aisearch] (formerly Vector Search) | [OpenSharing][d-opensharing] (formerly Delta Sharing), [Clean Rooms][d-cleanrooms], [Databricks Apps][d-apps], [Lakebase][d-lakebase] |
| Microsoft | [Direct Lake][m-directlake] semantic models in Power BI, [SQL analytics endpoint][m-sqlendpoint] | [Fabric AI Functions][m-aifunc], [Fabric data agent][m-dataagent], [Foundry IQ (Azure AI Search)][m-aisearch] | [OneLake shortcuts][m-shortcuts] |
| AWS | [Redshift][a-redshift], [Redshift Spectrum][a-spectrum], [Athena][a-athena], [AWS Quick][a-quick] (formerly QuickSight) | [Bedrock][a-bedrock], [Bedrock Knowledge Bases][a-kb], [OpenSearch Service][a-opensearch] for vectors | [Redshift data sharing][a-datashare], [AWS Clean Rooms][a-cleanrooms] |
| Google Cloud | [BigQuery][g-bq], [materialized views][g-mv], [BI Engine][g-biengine], [Looker][g-looker] | [BigQuery ML][g-bqml], [generative AI functions][g-genai], [vector search][g-vector], [Gemini Enterprise Agent Platform][g-agentplatform] (formerly Vertex AI) | [BigQuery sharing][g-sharing] (formerly Analytics Hub) |
| Snowflake | [Semantic views][s-semantic] | [Cortex AI Functions][s-cortex], [Cortex Search][s-search] | [Secure Data Sharing][s-sharing], [Marketplace][s-marketplace], [Data Clean Rooms][s-cleanrooms], [Streamlit in Snowflake][s-streamlit] |
| Open source | [Trino][o-trino], [DuckDB][o-duckdb] | — | [Delta Lake][o-delta] (Delta Sharing protocol) |

## Undercurrents

These run under every stage. Interviews often learn the most here, because
this is where judgement shows.

### Security and data management

**Know**
- Identity: users, groups, service principals, managed identities
- Least privilege; role-based and attribute-based access control (ABAC)
- Row filters and column masks
- Encryption and key management; secrets kept out of code
- Finding sensitive data (PII); anonymization and pseudonymization
- Catalogs, metadata, tags and discoverability; lineage; audit logs
- **Data quality:** profiling, validation rules and expectations (warn,
  drop, quarantine, fail), constraints, tests on models and sources,
  freshness checks, anomaly detection
- Retention and right-to-erasure deletion (GDPR); data sovereignty

**Probe:** *"Analysts may see all rows, except credit card numbers, which
only finance sees in full. Implement it."*

**Probe (quality):** *"A supplier sometimes sends files with a renamed
column. How does your pipeline notice, and what happens next?"*

| Platform | Access and protection | Catalog, lineage and quality |
|---|---|---|
| Databricks | [Unity Catalog][d-uc], [ABAC][d-abac], [row filters and column masks][d-masks], [data classification][d-classification], [secrets][d-secrets] | [Lineage][d-lineage], [tags][d-tags], [pipeline expectations][d-expectations], [data quality monitoring][d-dqm] (anomaly detection, and data profiling, formerly Lakehouse Monitoring) |
| Microsoft | [Microsoft Entra][m-entra], [OneLake security][m-onelakesec], [row-level security][m-rls], [dynamic data masking][m-ddm], [sensitivity labels][m-labels], [Azure Key Vault][m-keyvault] | [Lineage][m-lineage], [Microsoft Purview][m-purview] ([with Fabric][m-purviewfabric]) |
| AWS | [IAM][a-iam], [Lake Formation][a-lf], [KMS][a-kms], [Secrets Manager][a-secrets], [Macie][a-macie] (PII discovery) | [Glue Data Catalog][a-catalog], [SageMaker Catalog][a-smcatalog], [Glue Data Quality][a-gdq], [CloudTrail][a-cloudtrail] |
| Google Cloud | [IAM][g-iam], [Cloud KMS][g-kms], [Secret Manager][g-secret], [Sensitive Data Protection][g-sdp], [column data masking][g-masking], [row-level security][g-rls] | [Knowledge Catalog][g-catalog] (formerly Dataplex Universal Catalog), [data lineage][g-lineage], [auto data quality][g-dq] |
| Snowflake | [Dynamic data masking][s-masking], [row access policies][s-rap], [sensitive data classification][s-classify] | [Horizon Catalog][s-horizon], [object tagging][s-tags], [access history][s-access], [lineage][s-lineage], [data quality checks][s-dq] |
| Open source | [Unity Catalog OSS][o-uc], [Apache Polaris][o-polaris] | [OpenLineage][o-openlineage], [Great Expectations][o-gx], [Soda][o-soda], [dbt][o-dbt] tests |

### Orchestration

**Know**
- DAGs of tasks and dependencies
- Scheduled vs event-driven vs file-arrival triggers
- Retries, timeouts, conditional and looping tasks, parameters
- Repairing failed runs; alerting on failure
- **Idempotency and backfills:** a job that runs twice gives the same
  result; reprocessing history safely

**Probe:** *"Files land at unpredictable times. How do you trigger
processing without paying for idle compute?"*

| Platform | Technology to know |
|---|---|
| Databricks | [Lakeflow Jobs][d-jobs] (triggers, If/Else, For Each, repair runs) |
| Microsoft | [Data Factory pipelines][m-datafactory], [Apache Airflow job][m-airflow], [Activator][m-activator]; [Azure Data Factory][m-adf] |
| AWS | [Step Functions][a-sfn], [MWAA][a-mwaa] (Airflow), [EventBridge][a-eventbridge], [Glue workflows][a-gworkflows] |
| Google Cloud | [Managed Airflow][g-airflow] (formerly Cloud Composer), [Workflows][g-workflows] |
| Snowflake | [Tasks][s-tasks] and task graphs, [dynamic tables][s-dt] |
| Open source | [Airflow][o-airflow], [Dagster][o-dagster] |

### DataOps: monitoring, performance and cost

**Know**
- Logging, metrics and alerts on pipelines and data
- Troubleshooting failed and slow jobs: run history against a baseline,
  out-of-memory errors, library conflicts, query profiles
- Cost attribution: which workload costs what; budgets and limits
- Incidents: telling consumers when data is wrong; postmortems

**Probe:** *"A nightly job that took 20 minutes now takes 2 hours. How do
you investigate?"*

**Probe (cost):** *"Last month's bill doubled. Find out which workloads
caused it."*

| Platform | Technology to know |
|---|---|
| Databricks | [System tables][d-systables] (billing, compute, jobs, audit), [pipeline event log][d-eventlog], [SQL alerts][d-alerts], [query profile][d-qprofile], [Spark UI guide][d-sparkui] |
| Microsoft | [Monitor hub][m-monitor], [Capacity Metrics app][m-capacity]; [Azure Monitor][m-azmonitor] |
| AWS | [CloudWatch][a-cloudwatch], [CloudTrail][a-cloudtrail] |
| Google Cloud | [Cloud Monitoring][g-monitoring], [Cloud Logging][g-logging], [BigQuery workload management][g-workload] (reservations) |
| Snowflake | [Query history and query profile][s-qh], [resource monitors][s-rm], [alerts][s-alerts] |
| Open source | [OpenLineage][o-openlineage], Airflow and Dagster run history |

### Data architecture

**Know**
- Turning requirements into a design; batch vs streaming
- Choosing compute: serverless, job, interactive, warehouse; autoscaling;
  provisioned vs on-demand pricing
- Reliability, fault tolerance, disaster recovery, multi-region
- Migrations, and portability across vendors through open formats

**Probe:** *"Design the data platform for a company with 50 source systems
and a team of four engineers."*

| Platform | Compute to know |
|---|---|
| Databricks | [Serverless compute][d-serverless], [SQL warehouses][d-sqlwh], [Photon][d-photon] |
| Microsoft | [Fabric overview][m-overview] (capacity-based, one platform for all workloads), [Spark compute][m-spark] |
| AWS | [Glue][a-glue] (serverless Spark), [EMR][a-emr], [Redshift][a-redshift] (serverless or provisioned), [Lambda][a-lambda]; [in-scope services for the DEA-C01 exam][a-inscope] |
| Google Cloud | [BigQuery workload management][g-workload] (on-demand vs capacity), [Managed Service for Apache Spark][g-spark], [Dataflow][g-dataflow] |
| Snowflake | [Virtual warehouses][s-wh], [query acceleration service][s-qas], [search optimization service][s-sos] |
| Open source | [Spark][o-spark], [Trino][o-trino], [DuckDB][o-duckdb], open table formats for portability |

### Software engineering

**Know**
- Git: branches, pull requests, merge conflicts
- Project structure, packaging, dependency management
- Unit, integration and end-to-end tests for pipelines
- Infrastructure as code; promoting the same code across dev, test and
  prod

**Probe:** *"How do you test a PySpark transformation before it reaches
production?"*

| Platform | Technology to know |
|---|---|
| Databricks | [Declarative Automation Bundles][d-bundles] (formerly Databricks Asset Bundles), [Git folders][d-git] (formerly Repos), [Databricks CLI][d-cli], [Databricks SDK for Python][d-sdk] |
| Microsoft | [Fabric Git integration][m-git], [deployment pipelines][m-deploy]; [Bicep][m-bicep] on Azure |
| AWS | [CloudFormation][a-cfn], [AWS CDK][a-cdk], [SageMaker Unified Studio][a-smus] projects |
| Google Cloud | [Terraform on Google Cloud][g-terraform], [Dataform][g-dataform] (version-controlled SQL) |
| Snowflake | [Snowflake CLI][s-cli], [Git integration][s-git] |
| Open source | [Terraform][o-terraform], [dbt][o-dbt] |

## What certifications miss

The certifications cover ingestion, orchestration, security and operations
well. They barely test the following, so these are worth extra attention in
an interview:

- **Generation:** talking to source owners and agreeing on data contracts
- **Data modeling depth:** 5% of the Databricks Professional exam; only
  dbt and DP-750 treat it as a main topic
- **Idempotency and backfills:** hidden in "replayability" (AWS) and
  "managing restarts" (Google Cloud), never named on their own
- **Incidents:** troubleshooting is everywhere; on-call and postmortems are
  not

## The certifications behind the competencies

| Certification | Exam outline (weights) |
|---|---|
| [Databricks Data Engineer Associate][c-dbxa] (May 2026) | Platform 6% · Ingestion 21% · Transformation and modeling 22% · Lakeflow Jobs 16% · CI/CD 10% · Troubleshooting and optimization 10% · Governance and security 15% |
| [Databricks Data Engineer Professional][c-dbxp] (Oct 2026) | Python and SQL development 23% · Ingestion 12% · Data manipulation 12% · Monitoring 10% · Cost and performance 15% · Security and compliance 8% · Governance 5% · Debugging and deploying 10% · Data modeling 5% |
| [Microsoft DP-700, Fabric Data Engineer Associate][c-dp700] | Implement and manage 30–35% · Ingest and transform 30–35% · Monitor and optimize 30–35% |
| [Microsoft DP-750, Azure Databricks Data Engineer Associate][c-dp750] (Mar 2026) | Environment setup 15–20% · Secure and govern Unity Catalog 15–20% · Prepare and process data 30–35% · Deploy and maintain pipelines 30–35% |
| [AWS Data Engineer Associate (DEA-C01)][c-aws] | Ingestion and transformation 34% · Data store management 26% · Operations and support 22% · Security and governance 18% |
| [Google Cloud Professional Data Engineer][c-gcp] (v4.2) | Design ~22% · Ingest and process ~25% · Store ~20% · Prepare and use for analysis ~15% · Maintain and automate ~18% |
| [Snowflake SnowPro Advanced: Data Engineer (DEA-C02)][c-snow] | Data movement · Performance optimization · Storage and data protection · Data governance · Data transformation (weights not published) |
| [dbt Analytics Engineering][c-dbt] (dbt 1.11) | Developing models · Governance · Debugging · Pipelines · Tests · External dependencies · dbt state (weights not published) |

## Further reading

- Joe Reis and Matt Housley, *Fundamentals of Data Engineering*, O'Reilly,
  2022. The lifecycle and undercurrents this page follows.
- Martin Kleppmann, [*Designing Data-Intensive Applications*][b-ddia],
  O'Reilly. Storage, replication, streams: the theory under every platform.
- Ralph Kimball and Margy Ross, [*The Data Warehouse Toolkit*, 3rd
  edition][b-kimball], Wiley, 2013. Dimensional modeling.
- Tyler Akidau, Slava Chernyak and Reuven Lax, *Streaming Systems*,
  O'Reilly, 2018. Event time, watermarks and windows.
- [DAMA-DMBOK][b-dmbok], the data management body of knowledge. Governance,
  metadata, master data and data quality.
- [SFIA: Data engineering (DENG)][b-sfia]. The skill defined at levels 2 to
  6, useful for judging seniority rather than knowledge.
- Databricks, [Comprehensive guide to optimize Databricks, Spark and Delta
  Lake workloads][d-optguide]. Practical layout and tuning advice; some
  details predate liquid clustering.

## See also

Data contract · Data product · Lineage, once those pages exist.

## Where I disagree, or don't know yet

[Add after the interview test.]

{% comment %} ---------- Link definitions (not rendered) ---------- {% endcomment %}

[d-lakebase]: https://docs.databricks.com/aws/en/oltp/
[d-lfc]: https://docs.databricks.com/aws/en/ingestion/overview
[d-lfc-concepts]: https://docs.databricks.com/aws/en/ingestion/lakeflow-connect/
[d-community]: https://docs.databricks.com/aws/en/ingestion/community-connectors
[d-custom]: https://docs.databricks.com/aws/en/ingestion/custom-connectors
[d-pyds]: https://docs.databricks.com/aws/en/pyspark/datasources
[d-zerobus]: https://docs.databricks.com/aws/en/ingestion/zerobus-overview
[d-federation]: https://docs.databricks.com/aws/en/query-federation/
[d-autoloader]: https://docs.databricks.com/aws/en/ingestion/cloud-object-storage/auto-loader/
[d-copyinto]: https://docs.databricks.com/aws/en/ingestion/cloud-object-storage/copy-into/
[d-sss]: https://docs.databricks.com/aws/en/structured-streaming/
[d-delta]: https://docs.databricks.com/aws/en/delta/
[d-iceberg]: https://docs.databricks.com/aws/en/iceberg/
[d-uniform]: https://docs.databricks.com/aws/en/delta/uniform
[d-managed]: https://docs.databricks.com/aws/en/tables/managed
[d-volumes]: https://docs.databricks.com/aws/en/volumes/
[d-clustering]: https://docs.databricks.com/aws/en/delta/clustering
[d-partitions]: https://docs.databricks.com/aws/en/tables/partitions
[d-skipping]: https://docs.databricks.com/aws/en/delta/data-skipping
[d-filesize]: https://docs.databricks.com/aws/en/delta/tune-file-size
[d-optimize]: https://docs.databricks.com/aws/en/delta/optimize
[d-vacuum]: https://docs.databricks.com/aws/en/delta/vacuum
[d-po]: https://docs.databricks.com/aws/en/optimizations/predictive-optimization
[d-dv]: https://docs.databricks.com/aws/en/delta/deletion-vectors
[d-history]: https://docs.databricks.com/aws/en/delta/history
[d-cdf]: https://docs.databricks.com/aws/en/delta/delta-change-data-feed
[d-diskcache]: https://docs.databricks.com/aws/en/optimizations/disk-cache
[d-photon]: https://docs.databricks.com/aws/en/compute/photon
[d-pio]: https://docs.databricks.com/aws/en/optimizations/predictive-io
[d-aqe]: https://docs.databricks.com/aws/en/optimizations/aqe
[d-dfp]: https://docs.databricks.com/aws/en/optimizations/dynamic-file-pruning
[d-merge]: https://docs.databricks.com/aws/en/delta/merge
[d-variant]: https://docs.databricks.com/aws/en/semi-structured/variant
[d-ldp]: https://docs.databricks.com/aws/en/ldp/
[d-autocdc]: https://docs.databricks.com/aws/en/ldp/cdc
[d-expectations]: https://docs.databricks.com/aws/en/ldp/expectations
[d-eventlog]: https://docs.databricks.com/aws/en/ldp/monitor-event-logs
[d-mv]: https://docs.databricks.com/aws/en/views/materialized
[d-st]: https://docs.databricks.com/aws/en/tables/streaming
[d-metricviews]: https://docs.databricks.com/aws/en/metric-views/
[d-designer]: https://docs.databricks.com/aws/en/designer/
[d-sqlwh]: https://docs.databricks.com/aws/en/compute/sql-warehouse/
[d-serverless]: https://docs.databricks.com/aws/en/compute/serverless/
[d-aifunc]: https://docs.databricks.com/aws/en/large-language-models/ai-functions
[d-aisearch]: https://docs.databricks.com/aws/en/vector-search/vector-search
[d-dashboards]: https://docs.databricks.com/aws/en/dashboards/
[d-genie]: https://docs.databricks.com/aws/en/genie/
[d-opensharing]: https://docs.databricks.com/aws/en/delta-sharing/
[d-cleanrooms]: https://docs.databricks.com/aws/en/clean-rooms/
[d-apps]: https://docs.databricks.com/aws/en/dev-tools/databricks-apps/
[d-uc]: https://docs.databricks.com/aws/en/data-governance/unity-catalog/
[d-abac]: https://docs.databricks.com/aws/en/data-governance/unity-catalog/abac/
[d-masks]: https://docs.databricks.com/aws/en/data-governance/unity-catalog/filters-and-masks/
[d-lineage]: https://docs.databricks.com/aws/en/data-governance/unity-catalog/data-lineage
[d-tags]: https://docs.databricks.com/aws/en/database-objects/tags
[d-classification]: https://docs.databricks.com/aws/en/data-governance/unity-catalog/data-classification
[d-secrets]: https://docs.databricks.com/aws/en/security/secrets/
[d-dqm]: https://docs.databricks.com/aws/en/data-quality-monitoring/
[d-jobs]: https://docs.databricks.com/aws/en/jobs/
[d-bundles]: https://docs.databricks.com/aws/en/dev-tools/bundles/
[d-git]: https://docs.databricks.com/aws/en/repos/
[d-cli]: https://docs.databricks.com/aws/en/dev-tools/cli/
[d-sdk]: https://docs.databricks.com/aws/en/dev-tools/sdk-python
[d-systables]: https://docs.databricks.com/aws/en/admin/system-tables/
[d-alerts]: https://docs.databricks.com/aws/en/sql/user/alerts/
[d-qprofile]: https://docs.databricks.com/aws/en/sql/user/queries/query-profile
[d-sparkui]: https://docs.databricks.com/aws/en/optimizations/spark-ui-guide/
[d-optguide]: https://www.databricks.com/discover/pages/optimize-data-workloads-guide

[m-overview]: https://learn.microsoft.com/en-us/fabric/fundamentals/microsoft-fabric-overview
[m-onelake]: https://learn.microsoft.com/en-us/fabric/onelake/onelake-overview
[m-shortcuts]: https://learn.microsoft.com/en-us/fabric/onelake/onelake-shortcuts
[m-lakehouse]: https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-overview
[m-warehouse]: https://learn.microsoft.com/en-us/fabric/data-warehouse/data-warehousing
[m-eventhouse]: https://learn.microsoft.com/en-us/fabric/real-time-intelligence/eventhouse
[m-eventstreams]: https://learn.microsoft.com/en-us/fabric/real-time-intelligence/event-streams/overview
[m-mirroring]: https://learn.microsoft.com/en-us/fabric/mirroring/overview
[m-sqldb]: https://learn.microsoft.com/en-us/fabric/database/sql/overview
[m-datafactory]: https://learn.microsoft.com/en-us/fabric/data-factory/data-factory-overview
[m-copyjob]: https://learn.microsoft.com/en-us/fabric/data-factory/what-is-copy-job
[m-dataflow]: https://learn.microsoft.com/en-us/fabric/data-factory/dataflows-gen2-overview
[m-airflow]: https://learn.microsoft.com/en-us/fabric/data-factory/apache-airflow-jobs-concepts
[m-notebooks]: https://learn.microsoft.com/en-us/fabric/data-engineering/how-to-use-notebook
[m-spark]: https://learn.microsoft.com/en-us/fabric/data-engineering/spark-compute
[m-vorder]: https://learn.microsoft.com/en-us/fabric/data-engineering/delta-optimization-and-v-order
[m-mlv]: https://learn.microsoft.com/en-us/fabric/data-engineering/materialized-lake-views/overview-materialized-lake-view
[m-directlake]: https://learn.microsoft.com/en-us/fabric/fundamentals/direct-lake-overview
[m-sqlendpoint]: https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-sql-analytics-endpoint
[m-dataagent]: https://learn.microsoft.com/en-us/fabric/data-science/concept-data-agent
[m-activator]: https://learn.microsoft.com/en-us/fabric/real-time-intelligence/data-activator/activator-introduction
[m-git]: https://learn.microsoft.com/en-us/fabric/cicd/git-integration/intro-to-git-integration
[m-deploy]: https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/intro-to-deployment-pipelines
[m-monitor]: https://learn.microsoft.com/en-us/fabric/admin/monitoring-hub
[m-onelakesec]: https://learn.microsoft.com/en-us/fabric/onelake/security/get-started-security
[m-purviewfabric]: https://learn.microsoft.com/en-us/fabric/governance/microsoft-purview-fabric
[m-labels]: https://learn.microsoft.com/en-us/fabric/fundamentals/apply-sensitivity-labels
[m-rls]: https://learn.microsoft.com/en-us/fabric/data-warehouse/row-level-security
[m-ddm]: https://learn.microsoft.com/en-us/fabric/data-warehouse/dynamic-data-masking
[m-lineage]: https://learn.microsoft.com/en-us/fabric/governance/lineage
[m-capacity]: https://learn.microsoft.com/en-us/fabric/enterprise/metrics-app
[m-aifunc]: https://learn.microsoft.com/en-us/fabric/data-science/ai-functions/overview
[m-adls]: https://learn.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction
[m-eventhubs]: https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-about
[m-adf]: https://learn.microsoft.com/en-us/azure/data-factory/introduction
[m-adb]: https://learn.microsoft.com/en-us/azure/databricks/introduction/
[m-entra]: https://learn.microsoft.com/en-us/entra/fundamentals/whatis
[m-keyvault]: https://learn.microsoft.com/en-us/azure/key-vault/general/overview
[m-azmonitor]: https://learn.microsoft.com/en-us/azure/azure-monitor/overview
[m-purview]: https://learn.microsoft.com/en-us/purview/purview
[m-azsql]: https://learn.microsoft.com/en-us/azure/azure-sql/database/sql-database-paas-overview
[m-cosmos]: https://learn.microsoft.com/en-us/azure/cosmos-db/introduction
[m-bicep]: https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/overview
[m-asa]: https://learn.microsoft.com/en-us/azure/stream-analytics/stream-analytics-introduction
[m-aisearch]: https://learn.microsoft.com/en-us/azure/search/search-what-is-azure-search

[a-inscope]: https://docs.aws.amazon.com/aws-certification/latest/data-engineer-associate-01/dea-01-in-scope-services.html
[a-s3]: https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html
[a-lifecycle]: https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html
[a-s3tables]: https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-tables.html
[a-glue]: https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html
[a-catalog]: https://docs.aws.amazon.com/glue/latest/dg/catalog-and-crawler.html
[a-gdq]: https://docs.aws.amazon.com/glue/latest/dg/glue-data-quality.html
[a-gworkflows]: https://docs.aws.amazon.com/glue/latest/dg/workflows_overview.html
[a-databrew]: https://docs.aws.amazon.com/databrew/latest/dg/what-is.html
[a-emr]: https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-what-is-emr.html
[a-kds]: https://docs.aws.amazon.com/streams/latest/dev/introduction.html
[a-firehose]: https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html
[a-msk]: https://docs.aws.amazon.com/msk/latest/developerguide/what-is-msk.html
[a-dms]: https://docs.aws.amazon.com/dms/latest/userguide/Welcome.html
[a-appflow]: https://docs.aws.amazon.com/appflow/latest/userguide/what-is-appflow.html
[a-transfer]: https://docs.aws.amazon.com/transfer/latest/userguide/what-is-aws-transfer-family.html
[a-redshift]: https://docs.aws.amazon.com/redshift/latest/mgmt/welcome.html
[a-spectrum]: https://docs.aws.amazon.com/redshift/latest/dg/c-using-spectrum.html
[a-datashare]: https://docs.aws.amazon.com/redshift/latest/dg/datashare-overview.html
[a-rsmv]: https://docs.aws.amazon.com/redshift/latest/dg/materialized-view-overview.html
[a-zeroetl]: https://docs.aws.amazon.com/redshift/latest/mgmt/zero-etl-using.html
[a-athena]: https://docs.aws.amazon.com/athena/latest/ug/what-is.html
[a-lf]: https://docs.aws.amazon.com/lake-formation/latest/dg/what-is-lake-formation.html
[a-ddb]: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html
[a-ddbstreams]: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html
[a-rds]: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html
[a-aurora]: https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html
[a-opensearch]: https://docs.aws.amazon.com/opensearch-service/latest/developerguide/what-is.html
[a-lambda]: https://docs.aws.amazon.com/lambda/latest/dg/welcome.html
[a-sfn]: https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html
[a-mwaa]: https://docs.aws.amazon.com/mwaa/latest/userguide/what-is-mwaa.html
[a-eventbridge]: https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html
[a-quick]: https://docs.aws.amazon.com/quicksight/latest/user/welcome.html
[a-bedrock]: https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html
[a-kb]: https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html
[a-smus]: https://docs.aws.amazon.com/sagemaker-unified-studio/latest/userguide/what-is-sagemaker-unified-studio.html
[a-smcatalog]: https://docs.aws.amazon.com/sagemaker-unified-studio/latest/userguide/working-with-business-catalog.html
[a-iam]: https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html
[a-kms]: https://docs.aws.amazon.com/kms/latest/developerguide/overview.html
[a-secrets]: https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html
[a-macie]: https://docs.aws.amazon.com/macie/latest/user/what-is-macie.html
[a-cfn]: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html
[a-cdk]: https://docs.aws.amazon.com/cdk/v2/guide/home.html
[a-cloudwatch]: https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html
[a-cloudtrail]: https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html
[a-cleanrooms]: https://docs.aws.amazon.com/clean-rooms/latest/userguide/what-is.html

[g-gcs]: https://docs.cloud.google.com/storage/docs/introduction
[g-lifecycle]: https://docs.cloud.google.com/storage/docs/lifecycle
[g-bq]: https://docs.cloud.google.com/bigquery/docs/introduction
[g-partition]: https://docs.cloud.google.com/bigquery/docs/partitioned-tables
[g-cluster]: https://docs.cloud.google.com/bigquery/docs/clustered-tables
[g-biglake]: https://docs.cloud.google.com/bigquery/docs/biglake-intro
[g-iceberg]: https://docs.cloud.google.com/bigquery/docs/iceberg-tables
[g-bigtable]: https://docs.cloud.google.com/bigtable/docs/overview
[g-spanner]: https://cloud.google.com/spanner/docs/overview
[g-alloydb]: https://docs.cloud.google.com/alloydb/docs/overview
[g-cloudsql]: https://docs.cloud.google.com/sql/docs/introduction
[g-pubsub]: https://docs.cloud.google.com/pubsub/docs/overview
[g-kafka]: https://docs.cloud.google.com/managed-service-for-apache-kafka/docs/overview
[g-datastream]: https://docs.cloud.google.com/datastream/docs/overview
[g-dataflow]: https://docs.cloud.google.com/dataflow/docs/overview
[g-spark]: https://docs.cloud.google.com/dataproc/docs/concepts/overview
[g-sparkserverless]: https://docs.cloud.google.com/dataproc-serverless/docs/overview
[g-dts]: https://docs.cloud.google.com/bigquery/docs/dts-introduction
[g-datafusion]: https://docs.cloud.google.com/data-fusion/docs/concepts/overview
[g-dms]: https://docs.cloud.google.com/database-migration/docs/overview
[g-sts]: https://docs.cloud.google.com/storage-transfer/docs/overview
[g-dataform]: https://docs.cloud.google.com/dataform/docs/overview
[g-airflow]: https://docs.cloud.google.com/composer/docs/composer-3/composer-overview
[g-workflows]: https://docs.cloud.google.com/workflows/docs/overview
[g-catalog]: https://docs.cloud.google.com/dataplex/docs/introduction
[g-dq]: https://docs.cloud.google.com/dataplex/docs/auto-data-quality-overview
[g-lineage]: https://docs.cloud.google.com/dataplex/docs/about-data-lineage
[g-continuous]: https://docs.cloud.google.com/bigquery/docs/continuous-queries-introduction
[g-mv]: https://docs.cloud.google.com/bigquery/docs/materialized-views-intro
[g-biengine]: https://docs.cloud.google.com/bigquery/docs/bi-engine-intro
[g-looker]: https://docs.cloud.google.com/looker/docs/intro
[g-bqml]: https://docs.cloud.google.com/bigquery/docs/bqml-introduction
[g-genai]: https://docs.cloud.google.com/bigquery/docs/generative-ai-overview
[g-vector]: https://docs.cloud.google.com/bigquery/docs/vector-search-intro
[g-agentplatform]: https://docs.cloud.google.com/vertex-ai/docs/start/introduction-unified-platform
[g-sharing]: https://docs.cloud.google.com/bigquery/docs/analytics-hub-introduction
[g-iam]: https://docs.cloud.google.com/iam/docs/overview
[g-kms]: https://docs.cloud.google.com/kms/docs/key-management-service
[g-sdp]: https://docs.cloud.google.com/sensitive-data-protection/docs/sensitive-data-protection-overview
[g-masking]: https://docs.cloud.google.com/bigquery/docs/column-data-masking-intro
[g-rls]: https://docs.cloud.google.com/bigquery/docs/row-level-security-intro
[g-monitoring]: https://docs.cloud.google.com/monitoring/docs/monitoring-overview
[g-logging]: https://docs.cloud.google.com/logging/docs/overview
[g-workload]: https://docs.cloud.google.com/bigquery/docs/reservations-intro
[g-secret]: https://docs.cloud.google.com/secret-manager/docs/overview
[g-terraform]: https://docs.cloud.google.com/docs/terraform

[s-hybrid]: https://docs.snowflake.com/en/user-guide/tables-hybrid
[s-postgres]: https://docs.snowflake.com/en/user-guide/snowflake-postgres/about
[s-openflow]: https://docs.snowflake.com/en/user-guide/data-integration/openflow/about
[s-loading]: https://docs.snowflake.com/en/user-guide/data-load-overview
[s-copy]: https://docs.snowflake.com/en/sql-reference/sql/copy-into-table
[s-snowpipe]: https://docs.snowflake.com/en/user-guide/data-load-snowpipe-intro
[s-streaming]: https://docs.snowflake.com/en/user-guide/snowpipe-streaming/data-load-snowpipe-streaming-overview
[s-kafka]: https://docs.snowflake.com/en/user-guide/kafka-connector-overview
[s-micro]: https://docs.snowflake.com/en/user-guide/tables-clustering-micropartitions
[s-clusterkeys]: https://docs.snowflake.com/en/user-guide/tables-clustering-keys
[s-iceberg]: https://docs.snowflake.com/en/user-guide/tables-iceberg
[s-timetravel]: https://docs.snowflake.com/en/user-guide/data-time-travel
[s-failsafe]: https://docs.snowflake.com/en/user-guide/data-failsafe
[s-clone]: https://docs.snowflake.com/en/user-guide/object-clone
[s-sos]: https://docs.snowflake.com/en/user-guide/search-optimization-service
[s-qas]: https://docs.snowflake.com/en/user-guide/query-acceleration-service
[s-dt]: https://docs.snowflake.com/en/user-guide/dynamic-tables-about
[s-streams]: https://docs.snowflake.com/en/user-guide/streams-intro
[s-tasks]: https://docs.snowflake.com/en/user-guide/tasks-intro
[s-snowpark]: https://docs.snowflake.com/en/developer-guide/snowpark/index
[s-merge]: https://docs.snowflake.com/en/sql-reference/sql/merge
[s-dbt]: https://docs.snowflake.com/en/user-guide/data-engineering/dbt-projects-on-snowflake
[s-sharing]: https://docs.snowflake.com/en/user-guide/data-sharing-intro
[s-marketplace]: https://docs.snowflake.com/en/collaboration/collaboration-marketplace-about
[s-cortex]: https://docs.snowflake.com/en/user-guide/snowflake-cortex/aisql
[s-search]: https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-search/cortex-search-overview
[s-semantic]: https://docs.snowflake.com/en/user-guide/views-semantic/overview
[s-streamlit]: https://docs.snowflake.com/en/developer-guide/streamlit/about-streamlit
[s-horizon]: https://docs.snowflake.com/en/user-guide/snowflake-horizon
[s-masking]: https://docs.snowflake.com/en/user-guide/security-column-ddm-intro
[s-rap]: https://docs.snowflake.com/en/user-guide/security-row-intro
[s-tags]: https://docs.snowflake.com/en/user-guide/object-tagging
[s-access]: https://docs.snowflake.com/en/user-guide/access-history
[s-dq]: https://docs.snowflake.com/en/user-guide/data-quality-intro
[s-lineage]: https://docs.snowflake.com/en/user-guide/ui-snowsight-lineage
[s-cli]: https://docs.snowflake.com/en/developer-guide/snowflake-cli/index
[s-git]: https://docs.snowflake.com/en/developer-guide/git/git-overview
[s-rm]: https://docs.snowflake.com/en/user-guide/resource-monitors
[s-qh]: https://docs.snowflake.com/en/user-guide/ui-snowsight-activity
[s-wh]: https://docs.snowflake.com/en/user-guide/warehouses-overview
[s-alerts]: https://docs.snowflake.com/en/user-guide/alerts
[s-cleanrooms]: https://docs.snowflake.com/en/user-guide/cleanrooms/introduction
[s-classify]: https://docs.snowflake.com/en/user-guide/classify-intro

[o-spark]: https://spark.apache.org/docs/latest/
[o-sss]: https://spark.apache.org/docs/latest/streaming/index.html
[o-kafka]: https://kafka.apache.org/
[o-flink]: https://flink.apache.org/
[o-beam]: https://beam.apache.org/documentation/
[o-debezium]: https://debezium.io/documentation/
[o-airbyte]: https://docs.airbyte.com/
[o-dlt]: https://dlthub.com/docs/intro
[o-delta]: https://docs.delta.io/
[o-iceberg]: https://iceberg.apache.org/docs/latest/
[o-parquet]: https://parquet.apache.org/docs/
[o-airflow]: https://airflow.apache.org/docs/
[o-dagster]: https://dagster.io/docs
[o-dbt]: https://docs.getdbt.com/
[o-gx]: https://docs.greatexpectations.io/
[o-soda]: https://docs.soda.io/
[o-openlineage]: https://openlineage.io/docs/
[o-uc]: https://www.unitycatalog.io/
[o-polaris]: https://polaris.apache.org/
[o-trino]: https://trino.io/docs/current/
[o-duckdb]: https://duckdb.org/docs/
[o-terraform]: https://developer.hashicorp.com/terraform/docs

[c-dbxa]: https://www.databricks.com/sites/default/files/2026-10/Databricks-Certified-Data-Engineer-Associate-Exam-Guide-August-2026.pdf
[c-dbxp]: https://www.databricks.com/sites/default/files/2026-10/Databricks-Certified-Data-Engineer-Professional-Exam-Guide-9-Oct-2026.pdf
[c-dp700]: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/dp-700
[c-dp750]: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/dp-750
[c-aws]: https://docs.aws.amazon.com/aws-certification/latest/data-engineer-associate-01/data-engineer-associate-01.html
[c-gcp]: https://services.google.com/fh/files/misc/professional_data_engineer_exam_guide_english.pdf
[c-snow]: https://learn.snowflake.com/en/certifications/snowpro-advanced-dataengineer-C02
[c-dbt]: https://www.getdbt.com/certifications/analytics-engineer-certification-exam

[b-ddia]: https://dataintensive.net/
[b-kimball]: https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/books/data-warehouse-dw-toolkit/
[b-dmbok]: https://dama.org/learning-resources/dama-data-management-body-of-knowledge-dmbok/
[b-sfia]: https://sfia-online.org/en/sfia-9/skills/data-engineering
