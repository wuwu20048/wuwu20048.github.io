---
author: 小风果果
date: 2018-06-27 14:16:32+00:00
layout: post
title: Hibernate 拦截器使用
subtitle: Hibernate 拦截器使用
catalog: true
tags:
- Hibernate
---

## 需求描述

拦截系统的所有sql请求，并做相应处理

## 方案

此处可获取要执行的sql,并修改返回。

```java
public class SqlInterceptor implements StatementInspector{

    @Override
    public String inspect(String sql) {
        return null;
    }
}
```

hibernate配置项修改,增加一个配置属性，value值即你自定义的拦截器包路径或者 ref 引用

```xml
<prop key="hibernate.session_factory.statement_inspector">ctd.persistence.SqlInterceptor</prop>
```

## 使用限制

通过openSession的方式可以拦截。而使用openStatelessSession 的方式则就无能为力了。