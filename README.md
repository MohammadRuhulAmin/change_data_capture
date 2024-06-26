## Change Data Capture
### `Apache Kafka` `Apache Zookeeper` `Debezium`
#### Capturing MySql data
![image](https://github.com/MohammadRuhulAmin/change_data_capture/assets/67856574/1493c933-d9da-49c0-9efd-e7f24599e227)

- Step1: Start Zookeeper Server
  ```shell
    docker run -it --rm --name zookeeper -p 2181:2181 -p 2888:2888 -p 3888:3888 quay.io/debezium/zookeeper:2.5
  ```

