### 1.备份命令

```sh
# 备份命令
mysqldump -uroot -p1ve08ySjW4Fv3kb tms > /opt/data/tms_0724.sql
# 传输
scp root@192.168.1.136:/opt/data/tms_0724.sql /opt/data/tms_0724.sql
# 执行备份文件
mysql -uroot -pGhs14JWF5S
use prod_haichen
source /opt/data/tms_0724.sql
```

### 2.定时任务备份

```sh
#定义变量名
export mysqldump_date=$(date +%Y%m%d_%H%M%S) && \
#备份命令
mysqldump -uroot -proot database_name > /opt/data/mysql/$mysqldump_date.sql && \
gzip /opt/data/mysql/$mysqldump_date.sql
find /opt/data/mysql/ -name "*.sql" -mtime +15 -exec rm -f {} \;

```

### 3.数据恢复

```sh
# 查询binlog
show binary logs;
# 查看当前正在写入的binlog
show master status;
# 生成新的binlog文件，mysql的后续操作都会写入到新的binlog文件当中，一般在恢复数据都时候都会先执行这个命令。
flush logs;
# 查看binlog日志
show binlog events in 'binlog.000003';
#查看binlog文件的创建日期
cd /var/lib/mysql
# 恢复数据
mysqlbinlog --start-datetime=2020-06-19 20:00:00 --stop-position=660 /var/lib/mysql/binlog.000003 | mysql -uroot -proot_password datbase_name
```

> 小知识点：初始化mysql容器时，添加参数`--binlog-rows-query-log-events=ON`。或者到容器当中修改`/etc/mysql/my.cnf`文件，添加参数`binlog_rows_query_log_events=ON`，然后重启mysql容器。这样可以把原始的SQL添加到`binlog`文件当中。

**恢复命令格式：**

```sh
mysqlbinlog [options] file | mysql -uroot -proot_password database_name
```

mysqlbinlog常用参数：

> --start-datetime 开始时间，格式 2020-06-19 18:00:00 
>
> --stop-datetime 结束时间，格式同上
>
> --start-positon 开始位置，（需要查看binlog文件） 
>
> --stop-position 结束位置，同上 ...



### 修改视图定义者

```sh
# 查询出定义者为 prod_cap_root
select concat("alter DEFINER=`root`@`%` SQL SECURITY DEFINER VIEW `",TABLE_SCHEMA,"`.",TABLE_NAME," as ",VIEW_DEFINITION,";") from information_schema.VIEWS where DEFINER = 'prod_cap_root@%';
# 执行 查询的sql
```

