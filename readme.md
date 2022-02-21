# typecho-v1.17.10.30 + Viod-v.3.5.1 主题的blog部署 

- 源码在`code`中已经拷贝了主题/插件到其中。
    - 注意在主题最高级设置中可以参考 `theam.sample.json` 修改相关的配置。


## docker 启动 typecho 须知

```bash

docker run -itd --name=typecho \
     --restart=always \
     -p 127.0.0.1:8060:80 \
     -v $PWD/code:/var/www/app \
     -v /etc/localtime:/etc/localtime:ro \
   registry.cn-beijing.aliyuncs.com/rapid7/apache-php:php73

```

## docker 试用`mysql`启动须知

```bash

docker run -itd --name=mysql -p 127.0.0.1:3306:3306 --restart=always \
-v /srv/docker/data/mysqldata:/var/lib/mysql \
-e MYSQL_USER=admin007 \
-e MYSQL_PASSWORD=myadmin@816 \
-e MYSQL_DATABASE=xxscan \
-e MYSQL_ROOT_PASSWORD=test@1q2w2e4R \
-e character-set-server=utf8 \
-e collation-server=utf8_general_ci \
registry.cn-hangzhou.aliyuncs.com/xxzhang/mysql:5.7


docker exec -it gunicorn python3 manage.py collectstatic --noinput 

docker exec -t mysql mysql -uroot -ptest@1q2w2e4R -e 'drop database typecho;'
docker exec -t mysql mysql -uroot -ptest@1q2w2e4R -e 'CREATE DATABASE `typecho` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;'
# 导出数据库
docker exec -i mysql mysqldump -uroot -ptest@1q2w2e4R typecho > typechov1.sql
# 导入数据库
docker exec -t mysql mysql -uroot -ptest@1q2w2e4R -e 'CREATE DATABASE `typechov1` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;'
docker exec -i mysql mysql -uroot -ptest@1q2w2e4R typechov1 < typechov1.sql
```

## nginx省略
> 后续补充上`waf`


