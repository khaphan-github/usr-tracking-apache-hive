# Apache Hive Docker Cluster

This Docker Compose setup provides a complete Apache Hive environment with the following components:

## Services

- **Hadoop HDFS**: Namenode and Datanode for distributed storage
- **Hadoop YARN**: ResourceManager and NodeManager for resource management
- **PostgreSQL**: Metastore database for Hive metadata
- **Hive Metastore**: Manages metadata for Hive tables
- **HiveServer2**: Provides JDBC/ODBC interface for Hive
- **Hive WebHCat**: REST API for Hive operations

## Ports

- **9870**: Hadoop Namenode Web UI
- **8088**: Hadoop ResourceManager Web UI
- **5432**: PostgreSQL database
- **9083**: Hive Metastore
- **10000**: HiveServer2 (JDBC/ODBC)
- **10002**: HiveServer2 Web UI
- **50111**: Hive WebHCat REST API

## Usage

1. **Start the cluster:**
   ```bash
   docker-compose up -d
   ```

2. **Check service status:**
   ```bash
   docker-compose ps
   ```

3. **View logs:**
   ```bash
   docker-compose logs -f hiveserver2
   ```

4. **Connect to Hive using Beeline:**
   ```bash
   docker-compose exec hiveserver2 beeline -u jdbc:hive2://localhost:10000
   ```

5. **Stop the cluster:**
   ```bash
   docker-compose down
   ```

6. **Stop and remove all data:**
   ```bash
   docker-compose down -v
   ```

## Web UIs

- **Hadoop Namenode**: http://localhost:9870
- **Hadoop ResourceManager**: http://localhost:8088
- **HiveServer2**: http://localhost:10002

## Example Hive Commands

Once connected via Beeline:

```sql
-- Create a database
CREATE DATABASE test_db;

-- Use the database
USE test_db;

-- Create a table
CREATE TABLE employees (
    id INT,
    name STRING,
    department STRING
) STORED AS TEXTFILE;

-- Insert data
INSERT INTO employees VALUES (1, 'John Doe', 'Engineering');
INSERT INTO employees VALUES (2, 'Jane Smith', 'Marketing');

-- Query data
SELECT * FROM employees;
```

## Troubleshooting

1. **Services not starting**: Check if all required ports are available
2. **Connection issues**: Wait for all services to be healthy before connecting
3. **Memory issues**: Adjust memory settings in hadoop.env if needed
4. **Persistence**: Data is stored in Docker volumes and will persist between restarts

## Configuration

- Hadoop configuration is in `hadoop.env`
- Hive configuration is handled through environment variables in docker-compose.yaml
- PostgreSQL is used as the metastore database with credentials: hive/hive
