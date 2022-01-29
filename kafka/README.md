# simple studies - kafka

# environment composition:
- [x] 1 Kafka broker
- [x] 1 Zookeeper server
- [x] 1 Kafdrop interface (to observe topics and messages)


# about our configs (yml)
- the ports statement exposes the container’s port xxxx on the host’s port yyyy (example):
    ```
     ports:
     - "yyyy:xxxx" (thus, my local port yyyy will be able to access the xxxx container's port)
    ```

- kafka container:
  ```
    - ALLOW_PLAINTEXT_LISTENER=yes
    - KAFKA_LISTENER_SECURITY_PROTOCOL_MAP= CLIENT:PLAINTEXT,EXTERNAL:PLAINTEXT     
   ```
  
  *Defines key/value pairs for the security protocol to use, per listener name.*
```
    - KAFKA_ADVERTISED_LISTENERS= CLIENT://kafka-server:9092,EXTERNAL://localhost:9093
```
  *Describes how the host name that is advertised can be reached by clients. The value is published to ZooKeeper for clients to use. PLAINTEXT means Un-authenticated, non-encrypted channel. Other valid protocol values can be found [here](https://kafka.apache.org/11/javadoc/org/apache/kafka/common/security/auth/SecurityProtocol.html).*
```
    - KAFKA_ZOOKEEPER_CONNECT= zookeeper-server:2181
    - KAFKA_INTER_BROKER_LISTENER_NAME=CLIENT
```    
  *Defines which listener to use for inter-broker communication.* 
```
     - KAFKA_LISTENERS= CLIENT://:9092,EXTERNAL://:9093
```
  *In a multi-node (production) environment, you must set the KAFKA_ADVERTISED_LISTENERS property in your Dockerfile to the external host/IP address. Otherwise, by default, clients will attempt to connect to the internal host address.*

- kafdrop:
```
  - KAFKA_BROKERCONNECT: "PLAINTEXT://kafka-server:9092"
```
   *Has to be connected to all brokers individually to show its topics and values. If more than one broker is set, config pattern is "host:port,host:port"*

   Sources: 
   [Kafdrop](https://github.com/obsidiandynamics/kafdrop), [Confluent1](https://docs.confluent.io/platform/current/kafka/multi-node.html#), [Confluent2](https://docs.confluent.io/platform/current/installation/docker/config-reference.html)
# Commands
## Docker

from inside the project folder, type on terminal ` $ docker-compose up`, so the environments can be built. 
then, type `$ docker ps` to list all running process. if it all went well, you must see 3 lines, corresponding to the 3 services we've set on the yml file. as the following:


| CONTAINER ID | IMAGE                    | COMMAND                | CREATED            | STATUS            | PORTS                                                                   | NAMES                    |
|--------------|--------------------------|------------------------|--------------------|-------------------|-------------------------------------------------------------------------|--------------------------|
| 159af0dc18a3 | obsidiandynamics/kafdrop | "/kafdrop.sh"          | About a minute ago | Up About a minute | 0.0.0.0:9000->9000/tcp, :::9000->9000/tcp                               | kafka_kafdrop_1          |
| 8ca68d16a24e | bitnami/kafka:2          | "/opt/bitnami/script…" | About a minute ago | Up About a minute | 0.0.0.0:9092-9093->9092-9093/tcp, :::9092-9093->9092-9093/tcp           | kafka_kafka-server_1     |
| 123040c85f6c | bitnami/zookeeper:3      | "/opt/bitnami/script…" | About a minute ago | Up About a minute | 2888/tcp, 3888/tcp, 0.0.0.0:2181->2181/tcp, :::2181->2181/tcp, 8080/tcp | kafka_zookeeper-server_1 |

