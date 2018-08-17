Note: While anybody can use this (according to any potential upstream licenses), it's not _supported_ for public use.

----

Docker images to support implicit mysql replication support.

Features:
* based on official MariaDB images
* when you start the slave, it starts with replication started,
* no manual sync (mysqldump) is needed,
* slave fails to start if replication not healthy

Additional environment variables:
* REPLICATION_USER [default: replication]
* REPLICATION_PASSWORD [default: replication_pass]
* REPLICATION_HEALTH_GRACE_PERIOD [default: 3]
* REPLICATION_HEALTH_TIMEOUT [default: 10]
* MASTER_PORT [default: 3306]
* MASTER_HOST [default: master]

# Docker Run

## Start master

```
docker run -d -p 3506:3306 \
 -v /mydatabase/mysql/master:/var/lib/mysql \
  --name mysql_master \
  -e MYSQL_ROOT_PASSWORD=password \
  krutpong/mariadb10.3-replication
```

## Start slave with --link

```
  docker run -d -p 3507:3306 \
   -v /mydatabase/mysql/slave:/var/lib/mysql \
  --name mysql_slave \
  -e MYSQL_ROOT_PASSWORD=password \
  --link mysql_master:master \
  krutpong/mariadb10.3-replication
```

## Start slave with ip address

```
  docker run -d -p 3507:3306 \
   -v /mydatabase/mysql/slave:/var/lib/mysql \
  --name mysql_slave \
  -e MYSQL_ROOT_PASSWORD=password \
  -e MASTER_HOST=ip_master
  -e MASTER_PORT=3306
  krutpong/mariadb10.3-replication
```





## Start master [new user]

```
docker run -d -p 3506:3306 \
 -v /mydatabase/mysql/master:/var/lib/mysql \
  --name mysql_master \
  -e MYSQL_ROOT_PASSWORD=password \
  -e MYSQL_USER=example_user \
  -e MYSQL_PASSWORD=mysqlpwd \
  -e MYSQL_DATABASE=example \
  -e REPLICATION_USER=replication_user \
  -e REPLICATION_PASSWORD=myreplpassword \
  krutpong/mariadb10.3-replication
```

## Start slave with --link [new user]

```
  docker run -d -p 3507:3306 \
   -v /mydatabase/mysql/slave:/var/lib/mysql \
  --name mysql_slave \
  -e MYSQL_ROOT_PASSWORD=password \
  -e MYSQL_USER=example_user \
  -e MYSQL_PASSWORD=mysqlpwd \
  -e MYSQL_DATABASE=example \
  -e REPLICATION_USER=replication_user \
  -e REPLICATION_PASSWORD=myreplpassword \
  --link mysql_master:master \
  krutpong/mariadb10.3-replication
```

## Start slave with ip address [new user]

```
  docker run -d -p 3507:3306 \
   -v /mydatabase/mysql/slave:/var/lib/mysql \
  --name mysql_slave \
  -e MYSQL_ROOT_PASSWORD=password \
  -e MYSQL_USER=example_user \
  -e MYSQL_PASSWORD=mysqlpwd \
  -e MYSQL_DATABASE=example \
  -e REPLICATION_USER=replication_user \
  -e REPLICATION_PASSWORD=myreplpassword \
  -e MASTER_HOST=ip_master \
  -e MASTER_PORT=3306 \
  krutpong/mariadb10.3-replication
```


