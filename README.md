# Steps to test ClickHouse Zookeeper AZ-Aware Load Balancing


* Requires ClickHouse 24.7+

```shell
docker compose up -d

docker compose exec clickhouse-02 clickhouse-client --query "select host, availability_zone from system.zookeeper_connection"
# should print clickhouse-keeper-03 az3
docker compose stop clickhouse-keeper-03
docker compose exec clickhouse-02 clickhouse-client --query "select host, availability_zone from system.zookeeper_connection"
# should print clickhouse-keeper-02 or 01
docker compose start clickhouse-keeper-03
sleep 5
docker compose exec clickhouse-02 clickhouse-client --query "select host, availability_zone from system.zookeeper_connection"
# should print clickhouse-keeper-03 az3 again
```
