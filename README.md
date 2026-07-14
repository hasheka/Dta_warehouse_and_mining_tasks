# CDC Pipeline: PostgreSQL → Kafka using Debezium

## Objective
Implement a Change Data Capture (CDC) pipeline that streams real-time
database changes (INSERT, UPDATE, DELETE) from PostgreSQL to Kafka
using Debezium, without writing custom application code.

## Tech Stack
- PostgreSQL 15 (logical replication)
- Apache Kafka
- Zookeeper
- Debezium Connect 2.5
- Docker Compose

## How to Run
1. `docker compose up -d`
2. Verify containers: `docker ps`
3. Connect to Postgres: `docker exec -it postgres psql -U postgres -d company`
4. Create the `employee` table and insert sample data
5. Register the Debezium connector:
   `curl -X POST http://localhost:8083/connectors -H "Content-Type: application/json" -d @register-postgres.json`
6. Consume events from Kafka:
   `docker exec -it kafka kafka-console-consumer --bootstrap-server localhost:9092 --topic company_server.public.employee --from-beginning`

## Result
INSERT, UPDATE, and DELETE operations on the `employee` table were
captured in real time by Debezium and streamed as JSON events to the
Kafka topic `company_server.public.employee`. Each event includes an
operation code (`c` = create, `u` = update, `d` = delete, `r` = snapshot).

## Sample Output
See `output.png` / `output2.png` for terminal output showing the
connector status and captured Kafka events.