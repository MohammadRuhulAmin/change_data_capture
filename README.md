## Change Data Capture
### `Apache Kafka` `Apache Zookeeper` `Debezium`
#### Capturing MySql data
![image](https://github.com/MohammadRuhulAmin/change_data_capture/assets/67856574/1493c933-d9da-49c0-9efd-e7f24599e227)

- Step 1: Start Zookeeper Server (Starting the server for stand alone mode)
  ```shell
    docker run -it --rm --name zookeeper -p 2181:2181 -p 2888:2888 -p 3888:3888 quay.io/debezium/zookeeper:2.5
  ```
- Step 2: Start Kafka broker
  ```shell
   docker run -it --rm --name kafka -p 9092:9092 --link zookeeper:zookeeper quay.io/debezium/kafka:2.5
  ```
- Step 3: Starting a MySQL database
- 
  ```shell
  # Not necessarily needed for all time
  sudo lsof -i :3306
  sudo kill -9 <PID>
  ```
  ```shell
  docker run -it --rm --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=debezium -e MYSQL_USER=mysqluser -e MYSQL_PASSWORD=mysqlpw          quay.io/debezium/example-mysql:2.5
  ```
- Step 4: Starting Mysql Command line client
  ```shell
  docker run -it --rm --name mysqlterm --link mysql mysql:8.2 sh -c 'exec mysql -h"$MYSQL_PORT_3306_TCP_ADDR" -P"$MYSQL_PORT_3306_TCP_PORT"   -uroot -p"$MYSQL_ENV_MYSQL_ROOT_PASSWORD"'
  ```
  `select database inventory which is a default database` 
  ```sql
    mysql> use inventory;
    mysql> SELECT * FROM customers; #just for checking
  ```
- Step 5: Start Kafka Connect
  ```shell
   docker run -it --rm --name connect -p 8083:8083 -e GROUP_ID=1 -e CONFIG_STORAGE_TOPIC=my_connect_configs -e OFFSET_STORAGE_TOPIC=my_connect_offsets -e STATUS_STORAGE_TOPIC=my_connect_statuses --link kafka:kafka --link mysql:mysql quay.io/debezium/connect:2.5
  
  ```
- Step 6: Deploying the mysql connector
  ```shell
  curl -i -X POST -H "Accept:application/json" -H "Content-Type:application/json" localhost:8083/connectors/ -d '{ "name": "inventory-connector", "config": { "connector.class": "io.debezium.connector.mysql.MySqlConnector", "tasks.max": "1", "database.hostname": "mysql", "database.port": "3306", "database.user": "debezium", "database.password": "dbz", "database.server.id": "184054", "topic.prefix": "dbserver1", "database.include.list": "inventory", "schema.history.internal.kafka.bootstrap.servers": "kafka:9092", "schema.history.internal.kafka.topic": "schemahistory.inventory" } }'
  ```
- Step 7 : Vewing the captured event
  ```shell
    docker run -it --rm --name watcher --link zookeeper:zookeeper --link kafka:kafka quay.io/debezium/kafka:2.5 watch-topic -a -k   dbserver1.inventory.customers -a


  ```

  To see the captured event, lets execute the following query and observe the system one by one:
  ```sql
    update customers set last_name = "xyyd" where id = 1001;
    insert into customers(first_name,last_name,email)values("ruhul","amin","ruhul.cs.dev@gmail.com");
    delete from customers where email = "ruhul.cs.dev@gmail.com";
    
  ```



### Reference:
```html
https://debezium.io/documentation/reference/stable/tutorial.html
```


  

