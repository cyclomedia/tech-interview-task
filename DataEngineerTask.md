# Data Engineer Task

### Collaborative Point Cloud Pipeline

Build a small data pipeline: stream a COPC point cloud from remote storage, process the spatial data to extract meaningful attributes or metrics, and save the structured output in an efficient storage format.

### AI tools welcome

Use Cursor, Claude, Copilot, or any agent you like. We will ask how you used them — not whether you did. Include a short Approach section in your README.

### Background

Cyclomedia works with street-level imagery and point clouds. Our data pipelines need to ingest, index, and enrich heavy 3D spatial data efficiently at scale. This task is a scaled-down version of that problem — you will not receive any starter repo from us.

### Requirements

### 1. Pipeline Orchestration & Architecture

Build the pipeline using a modern orchestration framework (e.g., Dagster, Prefect, Apache Airflow, Mage, or a structured native Python DAG runner). Break the workflow into modular, retriable tasks with error logging.

### 2. Memory-Efficient Streaming & Processing

Interact with the remote 2 GB dataset efficiently via streaming, chunking, or HTTP Range Requests. Maintain a low, bounded memory footprint without downloading the entire file as a single monolithic block.

Load this COPC file — SoFi Stadium (public dataset, ~2 GB — expect a slower initial load):

[https://s3.amazonaws.com/hobu-lidar/sofi.copc.laz](https://s3.amazonaws.com/hobu-lidar/sofi.copc.laz "https://s3.amazonaws.com/hobu-lidar/sofi.copc.laz")

Preview in browser: viewer.copc.io

Tip: You are free to process and enrich the data as you see fit. Some suggested approaches include:

- **Ground Extraction:** Isolate ground points from structures and calculate the relative height-above-ground for remaining points.
- **Spatial Clustering / Segmentation:** Run a spatial clustering algorithm (e.g., block-based DBSCAN) to isolate structural boundaries or filter noise.
- **Downsampling & Statistical Profiling:** Apply voxel-grid downsampling and calculate local spatial features (e.g., point density, elevation standard deviation) per voxel.

Verify your streaming logic works without exhausting system memory before wiring up the storage layer.

### 3. Structured Storage Layer

Save the processed, enriched data into a storage layout of your choice (e.g., a spatial database like PostGIS/DuckDB, or cloud-native columnar formats like Apache Parquet/Delta Lake). Your architecture should prioritize efficient bulk-loading or batch-writing patterns rather than slow row-by-row operations.

### 4. Documentation

README with: prerequisites, how to run everything, how to verify the storage output, and an Approach section (how you scoped the work, what you asked AI to do, and what you verified manually).

### Submission

Create your own **public** repository on GitHub (or another version control hub), commit your solution there, and send us the link. We want to see your commit history and how the work evolved — not just a final zip or snapshot.

No tight time box — most candidates finish in a few hours with agents. If blocked, note what you tried in your README; we can still have a useful review.

### How we will test

- Follow your README to run it
- Inspect the DAG to assess task isolation, modularity, and error-handling decisions
- Execute the pipeline against the live S3 URL to observe data streaming and verify bounded memory consumption
- Verify that the processed points were successfully saved, enriched with your chosen metric, and indexed cleanly
- 45–60 min walkthrough: your decisions, AI usage, what you'd do next

### Stretch goals (optional)

- **Data Servability Bonus:** Build a lightweight consumer interface (e.g., a simple FastAPI endpoint or a quick Python/Streamlit CLI viewer) to demonstrate how downstream applications or analysts can easily serve, query, or visualize your processed data.
- Containerized or one-command setup via docker-compose
- Parallelize chunk processing using multiprocessing, Dask, or Ray
- Implement data quality/outlier noise filtering steps

### References

- COPC spec: copc.io
- PDAL (Point Data Abstraction Library): pdal.io
- Laspy documentation: laspy.readthedocs.io
- DuckDB Spatial extension: duckdb.org/docs/extensions/spatial