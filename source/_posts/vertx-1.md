title: vert.x 第一撩
date: 2016-03-16 23:15:43
tags: vertx
---
现在的主流SSH框架繁琐不堪、臃肿无比，再加上水平扩展上使用的dubbo以及各种缓存、队列，
单机配置启动一套完整的模拟生产环境就能榨干一台普通主机或笔记本的资源。各种配置也是眼花缭乱。
所以有些小需求往往会抛弃java这一套而选择php，nodejs等来简单粗暴的实现。


近来在网上闲逛偶遇vertx，这个被比喻为jvm上的nodejs框架，正好击中了我这种既想用java，
又怕配置麻烦的痛点。他们的官网又对程序员特别具有挑逗性，让人实在忍不住下载下来把玩一番。
还有。。。
<center>
![img](http://7xr6gl.com1.z0.glb.clouddn.com/vertx-is-fun.png)
</center>

<!-- more -->


## 先跑起来再说

先瞅瞅环境：
- jdk 8
- vertx 3.2.1
- ubuntu 1404
- mongo 3.2
- docker 1.9.1
- IDEA 15.02


## 一步一步来：

1. 先把vertx3的官方examples弄下来
```
git clone https://github.com/litten/hexo-theme-yilia.git
```

2. 然后把里面的web-examples揪出来用ide单独打开

3. 把pom文件中一个引用注释掉
```
<!-- dependency>
  <groupId>io.vertx</groupId>
  <artifactId>examples-utils</artifactId>
  <version>${project.version}</version>
  <optional>true</optional>
</dependency -->
```
4. 打开类io.vertx.example.util.Runner
在最后面加上
```
  static class HelloWorldRunner {
    public static void main(String[] args) {
      Runner.runExample(io.vertx.example.web.mongo.Server.class);
    }
  }
```
5. 打开类io.vertx.example.web.mongo
修改mongo client的连接参数
```
final MongoClient mongo = MongoClient.createShared(vertx, new JsonObject().put("db_name", "demo").put("host","127.0.0.1"));
```
6. 在docker的宿主机(ubuntu)上创建mongo的数据文件夹
```
mkdir -p /data/mongo
```

7. docker的mongo container跑起来
```
docker run --name mongo -v /data/mongo:/data/db -p 27017:27017 -d mongo
```

8. 再进入io.vertx.example.util.Runner类里面，在刚才新加的静态类的main方法上右键Run就OK了

9. 访问 [8080](http://localhost:8080) ,展现出一个添加用户的小例子

<center>
![img](http://7xr6gl.com1.z0.glb.clouddn.com/vertx-mongo-demo-page.png)
</center>


<h4 style="text-align:right;color:purple">
黑喂狗
</h4>