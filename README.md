# ETL Project: Kafka, Airflow, and Spark Integration

This project demonstrates an ETL (Extract, Transform, Load) pipeline using **Kafka**, **Airflow**, and **Spark**. It extracts data from a public API, processes it using Kafka and Spark, and loads it into a PostgreSQL database. The pipeline is orchestrated using Apache Airflow.

---

## **Directory Structure**
```
mohamedmostafam0-etl-project/
├── airflow.txt
├── docker-compose-airflow.yaml
├── docker-compose.yml
├── requirements.txt
├── airflow_resources/
│   ├── Dockerfile
│   └── dags/
│       ├── init.py
│       └── dag_kafka_spark.py
├── config/
│   └── scheduler/
├── dags/
│   └── dag_kafka_spark.py
├── kafka/
│   └── data/
│       ├── bootstrap.checkpoint
│       ├── cleaner-offset-checkpoint
│       ├── log-start-offset-checkpoint
│       ├── meta.properties
│       ├── recovery-point-offset-checkpoint
│       ├── replication-offset-checkpoint
│       ├── .kafka_cleanshutdown
│       ├── __cluster_metadata-0/
│       ├── rappel_conso-0/
├── scripts/
│   └── create_table.py
├── spark/
│   └── Dockerfile
└── src/
    ├── init.py
    ├── constants.py
    ├── kafka_client/
    │   ├── init.py
    │   ├── kafka_stream_data.py
    │   └── transformations.py
    └── spark_pgsql/
        └── spark_streaming.py
```

## **Components**

### **1. Kafka**
- **Role**: Acts as a message broker for streaming data.
- **Configuration**:
  - Kafka is configured using `docker-compose.yml`.
  - The `rappel_conso` topic is used to store processed data.
- **Data Flow**:
  - Data is produced by the `kafka_stream_data.py` script and consumed by the Spark streaming job.

### **2. Airflow**
- **Role**: Orchestrates the ETL pipeline.
- **DAGs**:
  - `dag_kafka_spark.py`: Defines the workflow for extracting data, streaming it to Kafka, and processing it with Spark.
- **Configuration**:
  - Airflow is configured using `docker-compose-airflow.yaml`.
  - The `LocalExecutor` is used for task execution.

### **3. Spark**
- **Role**: Processes streaming data from Kafka and writes it to PostgreSQL.
- **Configuration**:
  - Spark is containerized using the `spark/Dockerfile`.
  - The `spark_streaming.py` script reads data from Kafka, transforms it, and writes it to PostgreSQL.

### **4. PostgreSQL**
- **Role**: Stores the final processed data.
- **Configuration**:
  - PostgreSQL is configured using `docker-compose.yml`.
  - The `rappel_conso_table` table is created using the `create_table.py` script.

---

## **Setup Instructions**

### **1. Prerequisites**
- Docker and Docker Compose installed.
- Python 3.8+.

### **2. Clone the Repository**
```bash
git clone https://github.com/mohamedmostafam0/etl-project.git
cd etl-project
```

### **3. Set Up Environment Variables**
Create a `.env` file in the root directory with the following variables:
```plaintext
POSTGRES_USER=your_postgres_user
POSTGRES_PASSWORD=your_postgres_password
POSTGRES_DB=your_postgres_db
AIRFLOW_UID=1000
```

### **4. Build and Start the Services**
```bash
docker-compose -f docker-compose.yml up -d
docker-compose -f docker-compose-airflow.yaml up -d
```

### **5. Access the Services**
- **Airflow UI**: [http://localhost:8082](http://localhost:8082)
- **Kafka UI**: [http://localhost:8000](http://localhost:8000)
- **PostgreSQL**: `localhost:5432`
- **PgAdmin**: [http://localhost:5050](http://localhost:5050)

### **6. Trigger the DAG**
Open the Airflow UI and trigger the `kafka_spark_dag` DAG.

---

## **Workflow**

### **1. Data Extraction**
- The `kafka_stream_data.py` script fetches data from the public API and streams it to the `rappel_conso` Kafka topic.

### **2. Data Transformation**
- The `transformations.py` script normalizes and processes the data before sending it to Kafka.

### **3. Data Loading**
- The `spark_streaming.py` script reads data from Kafka, transforms it, and writes it to the `rappel_conso_table` in PostgreSQL.

---

## **Troubleshooting**

### **1. Kafka Connection Issues**
- Ensure Kafka is running and the `kafka:9092` endpoint is accessible.
- Check the Kafka logs for errors:
  ```bash
  docker logs <kafka-container-id>
  ```

### **2. Airflow Scheduler Errors**
- If the Airflow scheduler fails to connect to PostgreSQL, verify the database credentials in the `.env` file.
- Check the Airflow logs for errors:
  ```bash
  docker logs <airflow-scheduler-container-id>
  ```

### **3. Spark Streaming Issues**
- Ensure the Spark container can connect to Kafka and PostgreSQL.
- Check the Spark logs for errors:
  ```bash
  docker logs <spark-container-id>
  ```

---

## **License**
This project is licensed under the MIT License. See the `LICENSE` file for details.

---

## **Author**
**Mohamed Mostafa**

- **GitHub**: [mohamedmostafam0](https://github.com/mohamedmostafam0)

