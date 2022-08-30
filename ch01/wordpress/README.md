## Problem
The dockerfile form book does not work. Probably some problem associated with the --link option of `docker run`

### Steps that work
1. Pull images from docker hub  
```
docker pull wordpress:latest
docker pull mysql:latest
```

2. Create volume and network  
```
docker volume create db_data
docker network create mysqlnet
```

3. Start containers   
```
docker run --name test-mysql -e MYSQL_ROOT_PASSWORD=somewordpress -e MYSQL_DATABASE=wordpress -e MYSQL_USER=wordpress -e MYSQL_PASSWORD=wordpress -v db_data:/var/lib/mysql --net=mysqlnet -d mysql
docker run --name test-wordpress -e WORDPRESS_DB_HOST=test-mysql:3306 -e WORDPRESS_DB_USER=wordpress -e WORDPRESS_DB_PASSWORD=wordpress -e WORDPRESS_DB_NAME=wordpress --net=mysqlnet -p 8081:80 -d wordpress
```


### Reference
https://stackoverflow.com/a/55223785/9265852
