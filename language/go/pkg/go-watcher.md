---
title: "go-component/go-watcher源码分析"
slug: language/go/pkg/go-watcher
date: 2023-08-29T10:21:34+08:00
categories:
    - 源码分析
tags:
    - 设计与实现
    - golang
    - package
draft: true
---

## 介绍
是什么 + 作用

### 功能点
go-watcher main.go
main.go的改动是有实时热重启的。

但是参数方面，比如是一个配置文件，配置文件的改动是不会监听的，更不会影响到main.go的热重启
-w的作用好像也没有见到，当前目录或者指定目录，有文件新增/删除，或者文件内容有变化也不会影响到main.go的热重启

### 怎么用
怎么用

## 源码分析
逐步分析实现功能点的源码
