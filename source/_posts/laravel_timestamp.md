---
title: Laravel——timestamp字段
date: 2018-04-27 23:00:34
categories:
- php
tags:
- laravel
#top: true
--


> Larval  使用migrate的时候 创建新表：

```
Schema::create('friend_30_limit_task', function (Blueprint $table) {
            $table->increments('id');

            $table->timestamp('limit_time')->comment('过期时间');

            $table->timestamps();
        });
```

**而我去更新数据的时候。发现limit_time字段 和update_at字段一样会随着数据的更新而更新。**

立即回头去翻看了数据表：

```
CREATE TABLE `friend_30_limit_task` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,

  `limit_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '过期时间',

  `created_at` timestamp NULL DEFAULT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `friend_30_limit_task_user_id_index` (`user_id`),
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci
```

**DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP** 自动给我加了个这么个鬼东西。

找出mysql文档，https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_explicit_defaults_for_timestamp


![image](https://user-images.githubusercontent.com/9856659/54903040-e10ace00-4f15-11e9-95fa-1028531b64f9.png)

其中第二条，当表中的第一个**TIMESTAMP** 字段，并且默认不为空时。会自动加上 上面两个属性。

Ok找到问题，开始修改，新建一个migrate更新文件

参照laravel文档修改：**$table->timestamp('limit_time')->nullable()；**


结果运行报错
```
[Doctrine\DBAL\DBALException]
  Unknown column type "timestamp" requested. Any Doctrine type that you use has to be registered with \Doctrine\DBAL\Types\Type::addType(). You can get a list of all the known types with \Doctrine\DB
  AL\Types\Type::getTypesMap(). If this error occurs during database introspection then you might have forgot to register all database types for a Doctrine Type. Use AbstractPlatform#registerDoctrine
  TypeMapping() or have your custom types implement Type#getMappedDatabaseTypes(). If the type name is empty you might have a problem with the cache or forgot some mapping information.
```
google。。。
https://github.com/laravel/framework/issues/16526，大致意思是 大佬们互相撕逼说这不是laravel框架的问题，这是doctrine/dbal的问题。：）
继续追，https://github.com/doctrine/dbal/issues/2558

![image](https://user-images.githubusercontent.com/9856659/54903130-24653c80-4f16-11e9-8117-dd30411073c4.png)

**TIMESTAMP 是mysql的特定类型**。并且我们不准备支持它。你可以改用DATETIME 类型。

**THE END:**
删除 表migrations 的迭代更新记录、删除。重新artisan migrate。-
