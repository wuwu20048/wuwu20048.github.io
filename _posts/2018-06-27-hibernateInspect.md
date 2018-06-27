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

无状态session指不记录操作状态，操作结束就结束了。
有状态session记录操作状态，当一系列操作完成后才结束。所以它需要的资源多。
 
StatelessSession没有持久化上下文，也不提供多少高层的生命周期语义。特别是，无状态session不实现第一级cache,也不和第二级缓存，或者查询缓存交互。它不实现事务化写，也不实现脏数据检查。用stateless session进行的操作甚至不级联到关联实例。stateless session忽略集合类(Collections)。通过stateless session进行的操作不触发Hibernate的事件模型和拦截器。无状态session对数据的混淆现象免疫，因为它没有第一级缓存。无状态session是低层的抽象，和低层JDBC相当接近。