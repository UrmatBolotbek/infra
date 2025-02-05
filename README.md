# Infra Service

Infra Service provides the foundational infrastructure for our application, enabling local development and integration testing. It uses Docker Compose to launch and manage essential services such as PostgreSQL, Redis, Minio, RabbitMQ, Zookeeper, Kafka, and AKHQ.

---

## Services Provided

- **PostgreSQL (master_postgres):**  
  - **Image:** `postgres:13.3`  
  - **Purpose:** Provides a relational database for persistent storage.  
  - **Configuration:**  
    - User: `user`  
    - Password: `password`  
    - Database: `postgres`  
  - **Ports:** Exposed on port `5432`.

- **Redis (master_redis):**  
  - **Image:** `redis/redis-stack:latest`  
  - **Purpose:** Serves as an in-memory data store for caching and pub/sub messaging.  
  - **Ports:** Exposed on port `6379`.

- **Minio:**  
  - **Image:** `minio/minio`  
  - **Purpose:** Provides object storage with a web-based console for managing files and buckets.  
  - **Configuration:**  
    - Exposed on ports `9000` and `9001` (for console access).  
    - Uses volume mapping (e.g., `~/minio/data:/data`) for persistent storage.  
    - Credentials:  
      - Root User: `user`  
      - Root Password: `password`  
    - Command: Runs with a console accessible on port `9001`.

- **Minio Bucket Creator (createbuckets):**  
  - **Image:** `minio/mc`  
  - **Purpose:** Automatically creates and configures a bucket (`corpbucket`) in Minio after a brief delay.  
  - **Behavior:**  
    - Waits for Minio to start.  
    - Configures the host and creates the bucket with public access.

- **RabbitMQ:**  
  - **Image:** `rabbitmq:3.9-management`  
  - **Purpose:** Provides a messaging broker using the AMQP protocol along with a management UI.  
  - **Configuration:**  
    - Default User: `guest`  
    - Default Password: `guest`  
  - **Ports:**  
    - AMQP: `5672`  
    - Management UI: `15672`

- **Zookeeper (master_zookeeper):**  
  - **Image:** `confluentinc/cp-zookeeper:latest`  
  - **Purpose:** Coordinates Kafka brokers in the cluster.  
  - **Configuration:**  
    - Client Port: `2181`  
    - Tick Time: `2000`  
  - **Ports:** Exposed on port `2181`.

- **Kafka (master_kafka):**  
  - **Image:** `confluentinc/cp-kafka:latest`  
  - **Purpose:** Provides a distributed event streaming platform for processing real-time data.  
  - **Configuration:**  
    - Broker ID: `1`  
    - Connects to Zookeeper at `master_zookeeper:2181`  
    - Advertised Listeners: `PLAINTEXT://localhost:9092`  
    - Auto-Creation of Topics Enabled  
    - Pre-configured Topics for posts, likes, comments, feed updates, and more.  
  - **Ports:** Exposed on port `9092`.  
  - **Dependencies:** Starts after Zookeeper.

- **AKHQ:**  
  - **Image:** `tchiotludo/akhq`  
  - **Purpose:** Provides a web UI for monitoring and managing Kafka clusters and topics.  
  - **Configuration:**  
    - Configured via environment variable with Kafka connection details (e.g., `bootstrap.servers: "master_kafka:9092"`).  
  - **Ports:** Exposed on port `8088`.  
  - **Dependencies:** Starts after Kafka.

---

## Conclusion

Infra Service is essential for local development and testing, providing a full suite of infrastructure components via Docker Compose. This setup simplifies environment configuration, ensures consistency, and supports seamless integration of all microservices.

---
