# 简介
myservices微服务项目的项目结构。不包含各个微服务模块的代码。
微服务模块demo详见：
books: https://github.com/FengGeSe/books
comments: https://github.com/FengGeSe/comments

# 详情
本项目包含微服务所需的基础设置配置。
目前有:
---
- [x] Mysql           关系型数据库管理系统
- [x] Redis           key-value存储系统
- [x] Prometheus      开源的监控和报警系统
- [x] Zipkin          分布式链路追踪
- [ ] Kibana          开源的分析和可视化平台
- [ ] Elasticsearch   全文搜索和分析引擎
- [ ] Logstash        轻量级的日志搜集处理框架
- [ ] Kibana          开源的分析和可视化平台


docker-compose.yaml
```
version: '3'
services:
  #################################基础设施########################################
  mysql:
    image: mysql:5.7
    container_name: mysql
    ports:
      - 3306:3306/tcp
    environment:
      - MYSQL_ROOT_PASSWORD=123456
    volumes:
      - ./infrastructure/mysql/conf.d:/etc/mysql/conf.d
      - ./infrastructure/mysql/mysql.conf.d:/etc/mysql/mysql.conf.d
      - ./infrastructure/mysql/data:/var/lib/mysql
      - ./infrastructure/mysql/log:/var/log/mysql
  redis:
    image: redis
    container_name: redis
    ports:
      - 6379:6379/tcp
  prometheus:
    image: prom/prometheus
    container_name: prometheus
    ports:
      - 9090:9090/tcp
    volumes:
      - ./infrastructure/prometheus/data:/prometheus-data
    command: --config.file=/prometheus-data/prometheus.yml
  zipkin:
    image: openzipkin/zipkin
    container_name: zipkin
    ports:
      - 9411:9411/tcp

  #################################微服务########################################
  books:
    build: ./books
    image: books:1.0
    container_name: books
    depends_on:
      - mysql
      - redis
      - prometheus
      - zipkin
    ports:
      - 5001:5001
      - 5002:5002
      - 5003:5003
      - 5004:5004
      - 5005:5005

  comments:
    build: ./comments
    image: comments:1.0
    container_name: comments
    depends_on:
      - mysql
      - redis
      - prometheus
      - zipkin
    ports:
      - 5011:5001
      - 5012:5002
      - 5013:5003
      - 5014:5004
      - 5015:5005

```
# 文件目录
* README.md 
* docker-compose.yaml   // 定义了mysql容器启动参数
* infrastructure
    * mysql
        * conf.d   
            * disable_strict_mode.cnf   // 关掉sql语法strict模式
            * docker.cnf
            * mysql.cnf
            * utf8mb4.cnf   
        * data   
            * log    
                * error.log      // 文件权限 -rwxr-xr-x  否则写入不了日志
                * mysql.log      // 文件权限 -rwxr-xr-x  否则写入不了日志
        * mysql.conf.d   
            * mysqld.cnf

    * redis
    
    * prometheus
        * data
            * prometheus.yml    //  prometheus的配置文件
    
    * zipkin
         
# 使用
* 下载
```
git clone git@github.com:FengGeSe/myservices.git
```
* 启动
```
docker-compose up -d
```
* 连接
```
mysql -uroot -h127.0.0.1 -P3306 -p123456
```
