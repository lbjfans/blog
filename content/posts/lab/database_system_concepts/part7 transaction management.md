---
share: true
title: database system concepts --  transaction
date: 2024-08-10T14:41:00+08:00
tags:
  - lab
  - database
  - "#database_system_concepts"
  - database_system_concepts
dir: posts/lab/database_system_concepts
summary: 数据库系统概览--并发和恢复部分，笔记 + 课后题
---

# c18 Concurrency Control

## 笔记

> 概述

处理多个事务时，需要用到并发控制；最常用的是二阶段锁和快照隔离

> 锁

