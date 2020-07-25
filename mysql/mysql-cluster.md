# refer to https://www.cnblogs.com/FG123/p/12702092.html

sudo docker-compose up

# exec node1
进入node1，在/etc/mysql/mysql.conf.d/mysqld.cnf 中添加：
```
# cluster conf by czp
server-id=100
log-bin=mysql-bin
binlog-do-db=czp
```
进入 mysql 创建 repl 用户：
CREATE USER 'repl'@'%' IDENTIFIED BY 'password';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%';
/etc/init.d/mysql restart # 配置生效

# exec node2
进入node2，在/etc/mysql/mysql.conf.d/mysqld.cnf中添加：
```
server-id=101
log-bin=mysql-slave-bin
```
/etc/init.d/mysql restart # 配置生效
CHANGE MASTER TO
MASTER_HOST='172.21.0.3', -- 查看master的docker内网ip命令 : docker inspect  <container_name>
MASTER_PORT=3306,
MASTER_USER='repl',
MASTER_PASSWORD='password',
MASTER_LOG_FILE='mysql-bin.000003',
MASTER_LOG_POS=154;

show master status;
start slave;
show slave status;
show slave status\G

# exec node3
进入node3，在/etc/mysql/mysql.conf.d/mysqld.cnf 中添加：
```
server-id=102
log-bin=mysql-slave-bin
```
/etc/init.d/mysql restart # 配置生效

CHANGE MASTER TO
MASTER_HOST='172.21.0.3', -- 查看master的docker内网ip命令 : docker inspect  <container_name>
MASTER_PORT=3306,
MASTER_USER='repl',
MASTER_PASSWORD='password',
MASTER_LOG_FILE='mysql-bin.000003',
MASTER_LOG_POS=154;

show master status;
start slave;
show slave status;


保证 Slave_SQL_Running: Yes


---
```Dockerfile
version: "3" #版本信息
services: #服务列表
  mysql_1:
    image: mysql:5.7
    ports:
      - 3306:3306
    volumes:
      - "/root/mysql_server/node1/data:/var/lib/mysql"
      - "/root/mysql_server/node1/conf.d:/etc/mysql/conf.d"
    environment:
      - MYSQL_ROOT_PASSWORD=123456
    restart: always
  mysql_2:
    image: mysql:5.7
    ports:
      - 3307:3306
    volumes:
      - "/root/mysql_server/node2/data:/var/lib/mysql"
      - "/root/mysql_server/node2/conf.d:/etc/mysql/conf.d"
    environment:
      - MYSQL_ROOT_PASSWORD=123456
    restart: always
  mysql_3:
    image: mysql:5.7
    ports:
      - 3308:3306
    volumes:
      - "/root/mysql_server/node3/data:/var/lib/mysql"
      - "/root/mysql_server/node3/conf.d:/etc/mysql/conf.d"
    environment:
      - MYSQL_ROOT_PASSWORD=123456
    restart: always
```
