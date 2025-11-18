```mermaid
flowchart TD
    A --> B

    %% STYLE
    classDef aws fill=#232F3E,stroke=#fff,stroke-width=1px,color=#fff
    classDef db fill=#1D8102,stroke=#fff,color=#fff
    classDef compute fill=#DD6B10,stroke=#fff,color=#fff
    classDef storage fill=#4C9AFF,stroke=#fff,color=#fff

    %% NODES

    API["🎬 TMDB API"]

    Lambda["<b>⚡ AWS Lambda</b><br/>Serverless Ingestion Function"]
    class Lambda compute

    S3Raw["<b>🗂️ Amazon S3</b><br/>Raw Data Lake"]
    class S3Raw storage

    Glue["<b>🧭 AWS Glue</b><br/>Data Catalog"]
    class Glue aws

    Athena["<b>🔍 Amazon Athena</b><br/>Serverless SQL Engine"]
    class Athena aws

    dbt["<b>🧱 dbt</b><br/>Transformations (ELT)<br/>dbt-athena"]
    class dbt db

    S3Warehouse["<b>🏛️ Amazon S3</b><br/>Curated Warehouse (dbt models)"]
    class S3Warehouse storage

    QuickSight["<b>📊 Amazon QuickSight</b><br/>Dashboard / Visualization"]
    class QuickSight aws

    %% FLOWS
    API -->|Fetch JSON| Lambda
    Lambda -->|Write Raw JSON| S3Raw
    S3Raw -->|Schema Crawler| Glue
    Glue -->|Table Metadata| Athena
    Athena -->|Query Raw Tables| dbt
    dbt -->|Materialized Models| S3Warehouse
    S3Warehouse -->|Query Warehouse| Athena
    Athena -->|Dataset| QuickSight
