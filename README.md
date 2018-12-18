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
* 启动
```
docker-compose up -d
```

# 验证
* 连接mysql
```
mysql -h127.0.0.1 -P 3306 -uroot -p123456
```
* 连接redis
```
redis-cli -h 127.0.0.1 -p 6379
```
* 打开prometheus监控
```
$ open http://127.0.0.1:9090
```
* zipkin调用链路跟踪
```
$ open http://127.0.0.1:9411
```


