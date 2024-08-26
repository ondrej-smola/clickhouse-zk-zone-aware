

### Steps to test ClickHouse Zookeeper AZ-Aware Load Balancing

1. Deploy Operator
    ```shell
    helm repo add clickhouse-operator https://docs.altinity.com/clickhouse-operator
    helm install clickhouse-operator clickhouse-operator/altinity-clickhouse-operator
    ```
1. Deploy Keeper
    ```shell
    kubectl apply -f clickhouse-keeper.yaml
    ```
1. Deploy ClickHouse
    ```shell
    kubectl apply -f clickhouse-cluster.yaml
    ```
1. Every ClickHouse will connect to keeper in the same AZ
    ```shell
    select host, availability_zone from system.zookeeper_connection
    ```
1. Failover test
    1. Scale keeper sts to 2 replicas
    2. Watch CH pod in same AZ to connect to new keeper
    3. Scale keeper sts back to 3 replicas
    4. Watch CH pod in same AZ to connect back to keeper in same AZ 