```mermaid
flowchart TD
    A --> B
    TMDB[TMDB API]
    Lambda[AWS Lambda Ingestion Function]
    RawLake[Raw Data Lake (S3)]
    Glue[AWS Glue Data Catalog]
    Athena[Amazon Athena SQL Engine]
    DBT[DBT Transformations]
    Warehouse[Curated Data Warehouse (S3)]
    QuickSight[Amazon QuickSight Dashboard]

    TMDB -->|Fetch JSON| Lambda
    Lambda -->|Write Raw JSON| RawLake
    RawLake -->|Schema Crawler| Glue
    Glue -->|Table Metadata| Athena
    Athena -->|Query Raw Tables| DBT
    DBT -->|Materialized Models| Warehouse
    Warehouse -->|Query Warehouse| Athena
    Athena -->|Dataset| QuickSight
