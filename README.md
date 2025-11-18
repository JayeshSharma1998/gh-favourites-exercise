flowchart TD
    %% Nodes
    TMDB[TMDB API] 
    Lambda[AWS Lambda Ingestion Function]
    S3Raw[Raw Data Lake (Amazon S3)]
    Glue[AWS Glue Data Catalog]
    Athena[Amazon Athena SQL Engine]
    DBT[DBT Transformations (ELT)]
    S3Warehouse[Curated Data Warehouse (Amazon S3)]
    QuickSight[Amazon QuickSight Dashboard]

    %% Flows
    TMDB -->|Fetch JSON| Lambda
    Lambda -->|Write Raw JSON| S3Raw
    S3Raw -->|Schema Crawler| Glue
    Glue --> Athena
    Athena -->|Query Raw Tables| DBT
    DBT -->|Materialized Models| S3Warehouse
    S3Warehouse -->|Query Warehouse| Athena
    Athena -->|Dataset| QuickSight
