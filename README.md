## Change Data Capture
### `Apache Kafka` `Apache Zookeeper` `Debezium`
#### Capturing MySql data
![image](https://github.com/MohammadRuhulAmin/change_data_capture/assets/67856574/1493c933-d9da-49c0-9efd-e7f24599e227)

- Step1: Start Zookeeper Server (Starting the server for stand alone mode)
  ```shell
    docker run -it --rm --name zookeeper -p 2181:2181 -p 2888:2888 -p 3888:3888 quay.io/debezium/zookeeper:2.5
  ```
- Step2: Start Kafka broker
  ```shell
   docker run -it --rm --name kafka -p 9092:9092 --link zookeeper:zookeeper quay.io/debezium/kafka:2.5
  ```
- Step3: Starting a MySQL database
  kill :
  ```shell
  # Not necessarily needed for all time
  sudo lsof -i :3306
  sudo kill -9 <PID>
  ```
  ```shell
  docker run -it --rm --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=debezium -e MYSQL_USER=mysqluser -e MYSQL_PASSWORD=mysqlpw          quay.io/debezium/example-mysql:2.5
  ```





### Reference:
```html
https://debezium.io/documentation/reference/stable/tutorial.html
```


  

