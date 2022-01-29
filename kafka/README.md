# kafka simple studies

# environment composition:
- [x] 1 Kafka broker (server)
- [x] 1 Zookeeper server
- [x] 1 Kafdrop interface (to observe topics and messages)


# About our configs
- the ports statement exposes the container’s port 8080 (standard http port) on the host’s port 80 (example: 
    ```
     ports:
     - "80:8080"
    ```

- kafka container:
  - KAFKA_CFG_ZOOKEEPER_CONNECT= zookeeper_service_name:exposed_port
  - ALLOW_PLAINTEXT_LISTENER=yes
  - KAFKA_CFG_LISTENER_SECURITY_PROTOCOL_MAP=CLIENT:PLAINTEXT,EXTERNAL:PLAINTEXT
  - KAFKA_CFG_LISTENERS=CLIENT://:9092,EXTERNAL://:9093
  - KAFKA_CFG_ADVERTISED_LISTENERS=CLIENT://kafka-server:9092,EXTERNAL://localhost:9093
  - KAFKA_INTER_BROKER_LISTENER_NAME=CLIENT

# Commands
## Docker

from inside the project folder, type on terminal ` $ docker-compose up`, so the environments can be built. 
then, type `$ docker ps` to list all running process. if it all went well, you must see 3 lines, corresponding to the 3 services we've set on the yml file. as the following:


| CONTAINER ID | IMAGE                    | COMMAND                | CREATED            | STATUS            | PORTS                                                                   | NAMES                    |
|--------------|--------------------------|------------------------|--------------------|-------------------|-------------------------------------------------------------------------|--------------------------|
| 159af0dc18a3 | obsidiandynamics/kafdrop | "/kafdrop.sh"          | About a minute ago | Up About a minute | 0.0.0.0:9000->9000/tcp, :::9000->9000/tcp                               | kafka_kafdrop_1          |
| 8ca68d16a24e | bitnami/kafka:2          | "/opt/bitnami/script…" | About a minute ago | Up About a minute | 0.0.0.0:9092-9093->9092-9093/tcp, :::9092-9093->9092-9093/tcp           | kafka_kafka-server_1     |
| 123040c85f6c | bitnami/zookeeper:3      | "/opt/bitnami/script…" | About a minute ago | Up About a minute | 2888/tcp, 3888/tcp, 0.0.0.0:2181->2181/tcp, :::2181->2181/tcp, 8080/tcp | kafka_zookeeper-server_1 |

