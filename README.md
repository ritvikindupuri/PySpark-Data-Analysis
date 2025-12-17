# High-Performance Big Data Analytics: PySpark & Query Optimization

## Project Overview

This project demonstrates the implementation of scalable data pipelines using Apache Spark (PySpark) to process and analyze large-scale weather telemetry from the WHIN (Wabash Heartland Innovation Network) dataset.

Moving beyond standard in-memory processing, this project leverages Spark's distributed computing architecture. It focuses heavily on performance tuning, specifically analyzing Lazy Evaluation, the Catalyst Optimizer, and the difference between Logical and Physical Execution Plans for complex self-join aggregations.

---

## Skills & Technologies

* **Big Data Frameworks:** Apache Spark, PySpark
* **Query Languages:** Spark SQL, DataFrame API
* **Performance Optimization:** Catalyst Optimizer, Logical vs. Physical Query Plans, Lazy Evaluation
* **Data Formats:** Parquet (Columnar Storage)
* **Concepts:** Distributed Computing, DAG (Directed Acyclic Graph), Immutability, Self-Joins

---

## Project Breakdown

### Component 1: Spark Architecture & Lazy Evaluation
**Goal:** Establish a distributed computing environment and demonstrate the efficiency of Spark's execution model.

* **Session Initialization:** Configured a local SparkSession with specific memory constraints (2g driver memory) to simulate resource-constrained environments.
* **Lazy Evaluation Analysis:** Demonstrated the performance difference between defining transformations (instantaneous) and executing actions (latency-bound).
    * Result: Defining a complex groupBy and filter operation took ~0.15 seconds, while the .show() action triggering the actual computation took ~1.83 seconds, proving that Spark builds a Directed Acyclic Graph (DAG) before executing any tasks.

### Component 2: Spark SQL & Catalyst Optimization
**Goal:** Integrate SQL logic within the Spark environment and benchmark performance against the DataFrame API.

* **SQL Integration:** Created temporary views (createOrReplaceTempView) to execute standard ANSI SQL queries on distributed Parquet data.
* **Performance Benchmarking:** Compared the runtime of the DataFrame API vs. raw SQL queries.
    * Insight: Validated that both methods yielded identical performance (approx. 0.8s - 1.8s range), confirming that Spark's Catalyst Optimizer compiles both approaches into the exact same underlying execution plan, allowing developers to choose their preferred syntax without performance penalties.

### Component 3: Execution Planning & Complex Aggregations
**Goal:** Optimize complex analytical queries by inspecting the underlying execution engine.

* **Physical vs. Logical Planning:** Utilized the .explain(mode="cost") method to dissect query execution.
    * Logical Plan: Analyzed the abstract "recipe" of the query (Projections, Filters, Aggregates).
    * Physical Plan: Audited the low-level execution details (HashAggregate, Exchange, SortMergeJoin) to understand data shuffling costs.
* **Complex Self-Joins:** Engineered a complex temporal query to identify rapid temperature fluctuations.
    * Technique: Performed a self-join on the station dataset with a 1-hour time offset (t2.observation_time - INTERVAL 1 HOUR) to calculate hourly temperature deltas across thousands of sensors.
    * Optimization: Filtered for high-volume stations (>1000 observations) to isolate statistically significant anomalies.

---

## Execution

1.  **Environment:** Requires a Spark-compatible environment (e.g., Databricks, Google Colab, or a local setup with pyspark installed).
2.  **Data:** Access to the WHIN weather dataset in Parquet format (weather.parquet).
3.  **Run:** Execute the Jupyter Notebook Project_13.ipynb sequentially.
    * Note: Ensure the Java Development Kit (JDK) is properly configured in your system path for Spark to initialize.

---

## Conclusion

This project highlights the shift from traditional single-node processing to distributed big data engineering. By peering "under the hood" of Spark—analyzing how the Catalyst Optimizer translates Python code into Physical Plans—it demonstrates a deep understanding of how to write efficient, scalable code that minimizes expensive operations like data shuffling (Exchanges) in a production environment.

---

## Resume Reference (ATS Optimized)

**Project Title:** Big Data Engineering & Spark Query Optimization

* Architected high-throughput data pipelines using PySpark to process diverse weather telemetry datasets, utilizing Parquet columnar storage for optimized I/O performance.
* Engineered complex temporal self-joins to identify environmental anomalies, utilizing Spark SQL explain plans (.explain()) to analyze Logical vs. Physical execution and minimize data shuffling overhead.
* Leveraged Spark’s Catalyst Optimizer and Lazy Evaluation model to reduce computational latency for large-scale aggregation tasks.
