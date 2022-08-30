## Deploy wordpress + mysql docker image
1. Pull images from docker hub  
```
docker pull wordpress:latest
docker pull mysql:latest
```

2. Start containers  
### [option 1] Create docker volume and network. Use in both containers
```
docker volume create db_data
docker network create mysqlnet

docker run --name mysql1 \
-e MYSQL_ROOT_PASSWORD=somewordpress \
-e MYSQL_DATABASE=wordpress \
-e MYSQL_USER=wordpress \
-e MYSQL_PASSWORD=wordpress \
-v db_data:/var/lib/mysql \
--net=mysqlnet \
-d mysql

docker run --name test-wordpress \
-e WORDPRESS_DB_HOST=test-mysql:3306 \
-e WORDPRESS_DB_USER=wordpress \
-e WORDPRESS_DB_PASSWORD=wordpress \
-e WORDPRESS_DB_NAME=wordpress \
--net=mysqlnet 
-p 8081:80 
-d wordpress
```

### [option 2] use --link
```
docker run --name mysqlwp \
-e MYSQL_ROOT_PASSWORD=wordpressdocker \
-e MYSQL_DATABASE=wordpress \
-e MYSQL_USER=wordpress \
-e MYSQL_PASSWORD=wordpresspwd \
-v /home/docker/mysql:/var/lib/mysql \
-d mysql

docker run --name wordpress  \
--link mysqlwp:mysql 
-p 80:80 \
-e WORDPRESS_DB_NAME=wordpress \
-e WORDPRESS_DB_USER=wordpress \
-e WORDPRESS_DB_PASSWORD=wordpresspwd \
-d wordpress
```

### Reference
https://stackoverflow.com/a/55223785/9265852
