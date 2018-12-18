# 介绍
使用官方mysql:5.7镜像，将配置，数据，日志挂载到宿主机。

docker-compose.yaml
```
version: '2'
services:
  mysql:
    image: mysql:5.7
    container_name: mysql
    ports:
      - 3306:3306/tcp
    environment:
      - MYSQL_ROOT_PASSWORD=123456
    volumes:
      - ./mysql/conf.d:/etc/mysql/conf.d
      - ./mysql/mysql.conf.d:/etc/mysql/mysql.conf.d
      - ./mysql/data:/var/lib/mysql
      - ./mysql/log:/var/log/mysql
```
# 文件目录
* README.md 
* docker-compose.yaml   // 定义了mysql容器启动参数
* mysql
    * conf.d   // 挂载到容器中 /etc/mysql/conf.d
        * disable_strict_mode.cnf   // 关掉sql语法strict模式
        * docker.cnf
        * mysql.cnf
        * utf8mb4.cnf     // utf8mb4开启
    * data    // 挂载到容器中  /var/lib/mysql
        * log     // 挂载到容器中  /var/log/mysql
            * error.log      // 文件权限 -rwxr-xr-x  否则写如不了日志
            * mysql.log      // 文件权限 -rwxr-xr-x  否则写如不了日志
    * mysql.conf.d    // 挂载到容器中  /etc/mysql/mysql.conf.d
            * mysqld.cnf
# 使用
* 下载
```
git clone git@github.com:FengGeSe/docker-compose-mysql.git  &&  cd docker-compose-mysql  && rm -rf .git
```
* 启动
```
docker-compose up -d
```
* 查看
```
docker ps | grep mysql
```
* 连接
```
mysql -uroot -h127.0.0.1 -P3306 -p123456
```
