- mysql的innodb如何定位锁问题，mysql如何减少主从复制延迟？

mysql的innodb如何定位锁问题:

在使用 show engine innodb status检查引擎状态时，发现了死锁问题

在5.5中，information_schema 库中增加了三个关于锁的表（MEMORY引擎）

innodb_trx ## 当前运行的所有事务

innodb_locks ## 当前出现的锁

innodb_lock_waits ## 锁等待的对应关系

mysql如何减少主从复制延迟:

如果延迟比较大，就先确认以下几个因素：

1.从库硬件比主库差，导致复制延迟

2.主从复制单线程，如果主库写并发太大，来不及传送到从库就会导致延迟。更高版本的mysql可以支持多线程复制，慢SQL语句过多，网络延迟，master负载主库读写压力大，导致复制延迟，架构的前端要加buffer及缓存层

slave负载。一般的做法是，使用多台slave来分摊读请求，再从这些slave中取一台专用的服务器只作为备份用，不进行其他任何操作.另外， 2个可以减少延迟的参数:

–slave-net-timeout=seconds 单位为秒 默认设置为 3600秒

#参数含义：当slave从主数据库读取log数据失败后，等待多久重新建立连接并获取数据

–master-connect-retry=seconds 单位为秒 默认设置为 60秒

#参数含义：当重新建立主从连接时，如果连接建立失败，间隔多久后重试

通常配置以上2个参数可以减少网络问题导致的主从数据同步延迟

MySQL数据库主从同步延迟解决方案

最简单的减少slave同步延时的方案就是在架构上做优化，尽量让主库的DDL快速执行

还有就是主库是写，对数据安全性较高，比如sync_binlog=1，innodb_flush_log_at_trx_commit

= 1 之类的设置，而slave则不需要这么高的数据安全，完全可以讲sync_binlog设置为0或者关闭binlog

innodb_flushlog也可以设置为0来提高sql的执行效率。另外就是使用比主库更好的硬件设备作为slave

- 如何重置mysql root密码？

一、 在已知MYSQL数据库的ROOT用户密码的情况下，修改密码的方法：

1、 在SHELL环境下，使用mysqladmin命令设置：

mysqladmin –u root –p password “新密码” 回车后要求输入旧密码

2、 在mysql>环境中,使用update命令，直接更新mysql库user表的数据：

Update mysql.user set password=password(‘新密码’) where user=’root’;

flush privileges;

注意：mysql语句要以分号”；”结束

3、 在mysql>环境中，使用grant命令，修改root用户的授权权限。

grant all on . to root@’localhost’ identified by ‘新密码’；

二、 如查忘记了mysql数据库的ROOT用户的密码，又如何做呢？方法如下：

1、 关闭当前运行的mysqld服务程序：service mysqld stop（要先将mysqld添加为系统服务）

2、 使用mysqld_safe脚本以安全模式（不加载授权表）启动mysqld 服务

/usr/local/mysql/bin/mysqld_safe --skip-grant-table &

3、 使用空密码的root用户登录数据库，重新设置ROOT用户的密码

＃mysql -u root

Mysql> Update mysql.user set password=password(‘新密码’) where user=’root’;

Mysql> flush privileges;