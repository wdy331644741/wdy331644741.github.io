---
title: webpack 修改dev-server 端口
date: 2018-04-27 23:00:34
categories:
- 前端
tags:
- webpack
#top: true
---

# 相关错误

> 运行webpack 报Invalid Host header！

### Package.json 属性说明

- name - 包名。
- version - 包的版本号。
- description - 包的描述。
- homepage - 包的官网 url 。
- author - 包的作者姓名。
- contributors - 包的其他贡献者姓名。
- dependencies - 依赖包列表。如果依赖包没有安装，npm 会自动将依赖包安装在 node_module 目录下。
- repository - 包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上。
- main - main 字段指定了程序的主入口文件，require('moduleName') 就会加载这个文件。这个字段的默认值是模块根目录下面的 index.js。
- keywords - 关键字


# webpack-dev-server
> webpack-dev-server是一个小型的Node.js Express服务器,它使用webpack-dev-middleware来服务于webpack的包,除此自外，它还有一个通过Sock.js来连接到服务器的微型运行时



demo:
[git clone git@github.com:ruanyf/webpack-demos.git](git@github.com:ruanyf/webpack-demos.git)


webpack.config.js 配置文件参数详细介绍:[ webpack 官方文档](https://doc.webpack-china.org/configuration/)

webpack-dev-server 
运行 Invalid Host header

解决办法

经过检测 端口8080 只允许监听本地的8080端口
> root@wdy ~ # netstat -aon

> Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       Timer
tcp        0      0 0.0.0.0:139             0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0      0 0.0.0.0:6379            0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0      0 127.0.0.1:11211         0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0      0 127.0.0.1:8080          0.0.0.0:*               LISTEN

改用
```# webpack-dev-server --host 0.0.0.0 ```

仍提示Invalid Host header

```# vi webpack.config.js```

```
//追加配置
devServer: {
    host: '0.0.0.0',
    disableHostCheck: true
  }
```
success！
