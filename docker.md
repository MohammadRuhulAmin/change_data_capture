# Docker 

## Remove all the iamges and containers
  ```shell
    docker rmi $(docker images -a -q)
    docker container prune
    docker volume prune
  ```