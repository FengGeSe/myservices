# 简介
myservices微服务项目的项目结构。不包含各个微服务模块的代码。

微服务模块demo详见：

books: https://github.com/FengGeSe/books

comments: https://github.com/FengGeSe/comments


# 详情
本项目包含微服务所需的基础设置配置。
目前有：
- [x] Mysql           关系型数据库管理系统
- [x] Redis           key-value存储系统
- [x] Prometheus      开源的监控和报警系统
- [x] Zipkin          分布式链路追踪
- [ ] Kibana          开源的分析和可视化平台
- [ ] Elasticsearch   全文搜索和分析引擎
- [ ] Logstash        轻量级的日志搜集处理框架
- [ ] Kibana          开源的分析和可视化平台



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
* 创建mysql日志文件
ps: 必须赋予相应的权限，mysql才能写入日志到这里。
```
cd infrastructure/mysql/log/ && touch mysql.log error.log &&  chmod 755 mysql.log error.log
```
* 启动
```
docker-compose up -d
```

# 验证
* 连接mysql
```
$ mysql -h127.0.0.1 -P 3306 -uroot -p123456
```
* 连接redis
```
$ redis-cli -h 127.0.0.1 -p 6379
```
* 打开prometheus监控
```
$ open http://127.0.0.1:9090
```
* zipkin调用链路跟踪
```
$ open http://127.0.0.1:9411
```

# 管理
开发环境下使用docker-compose管理。
常用命令如下：
* 拉取所有镜像
```
$ docker-compose pull
```
* 构建所有镜像
```
$ docker-compose build
```
* 启动所有容器
```
$ docker-compose up -d
```
* 销毁所有容器
```
$ docker-compose down
```
* 查看某个容器的日志
```
$ docker logs -f comments
```
* 停止某个容器
```
$ docker-compose stop comments
```
* 移除某个容器(先stop后rm)
```
$ docker-compose rm comments
```
* 启动某个容器
```
$ docker-compose up -d comments
```




# 添加微服务
添加一个微服务模块到你的项目。有2种方式。

### 第一种: 从源码build。 
这个适用于你是负责这个模块的开发者，有这个模块的源码权限。

比如要添加一个books的微服务模块.
在myservices目录下
```
$ git clone git@github.com:FengGeSe/books.git
```
添加如下配置到docker-compose.yml文件:
```
   books:
    image: gxlz/golang:1.10.3-apline3.7
    container_name: books
    ports:
      - 5001:5001
      - 5002:5002
      - 5003:5003
      - 5004:5004
      - 5005:5005
    volumes:
      - ./books:/go/src/myservices/books
    depends_on:
      - mysql
      - redis
      - prometheus
      - zipkin
    entrypoint:
      - ash
      - -c
      - |
        sleep 15 && \
        cd /go/src/myservices/books && \
        realize start
```
将myservices/books目录下的源码挂载到容器内部。
并用realize命令监控源码的改动，从而重启服务。
这个在开发的时候十分方便。

ps: 配置请根据具体情况调整。

### 第二种: 从镜像仓库拉取。 
这个适用于别人维护的模块，你需要调用了人家的服务。

比如要添加一个comments的微服务模块.
在myservices目录下
```
$ git clone git@github.com:FengGeSe/comments.git
```
添加如下配置到docker-compose.yaml:
```
  comments:
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
ps: 配置请根据具体情况调整。

