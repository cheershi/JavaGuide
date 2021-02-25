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

