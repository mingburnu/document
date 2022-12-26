## Master Host
> vim /etc/mysql/mysql.conf.d/mysqld.cnf

    [mysqld]
    bind-address = 0.0.0.0
    server-id = 1
    log-bin=mysql-bin

> service mysql restart

> mysql -u root -p

> CREATE USER 'slave'@'192.168.0.246' IDENTIFIED BY 'password';   GRANT
> REPLICATION SLAVE ON *.* TO 'slave'@'192.168.0.246';   FLUSH
> PRIVILEGES;

> SHOW MASTER STATUS \G

> show slave hosts;

## Slave Host
> vim /etc/mysql/mysql.conf.d/mysqld.cnf

    [mysqld]    
    server-id=2    
    read-only=on

> service mysql restart

> mysql -u root -p

> CHANGE MASTER TO MASTER_HOST ='192.168.0.245', MASTER_USER ='slave', MASTER_PASSWORD ='password';

> START SLAVE;

> SHOW SLAVE STATUS \G

### REFERENCE
[MySQL Master Slave Replication 主從式架構設定教學](https://medium.com/dean-lin/%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%B6%E4%BD%A0%E5%AF%A6%E4%BD%9C-mysql-master-slave-replication-16d0a0fa1d04)
