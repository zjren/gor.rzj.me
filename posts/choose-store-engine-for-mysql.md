---
date: 2013-03-20
layout: post
title: Mysql常见存储引擎的特点和Mysql选择存储引擎的基本原则
categories:
- Mysql
tags:
- Mysql
---

Mysql中常见的三种引擎特点

MyISAM引擎

> * 用途: 快读
> * 锁: 全表锁定
> * 持久性: 基于表恢复
> * 事务特性: 不支持
> * 支持索引类型: B-tree/FullText/R-tree

Memory引擎

> * 用途: 内存数据
> * 锁: 全表锁定
> * 持久性: 无磁盘I/O，无可持久性
> * 事务特性: 不支持
> * 支持索引类型: Hash/B-tree

InnoDb引擎

> * 用途: 完整的事务支持
> * 锁: 多种隔离级别的行锁
> * 持久性: 基于日志的恢复
> * 事务特性: 支持
> * 支持索引类型: Hash/B-tree


Mysql选择存储引擎的基本原则

* 采用MyISAM引擎

> 1. R/W>100:1，且update较少
> 2. 并发不高，且不需要事务
> 3. 表数据量小
> 4. 硬件资源有限

* 采用InnoDB引擎

> 1. R/W较小，频繁更新大字段
> 2. 表数据量超过1000万，并发高
> 3. 安全性和可靠性要求高

* 采用Memory引擎

> 1. 有足够的内存
> 2. 对数据的一致性要求不高，如在线人数和Session等应用
> 3. 需要定期归档的数据

--摘自[《PHP核心技术与最佳实践》](http://book.douban.com/subject/20370984/)
