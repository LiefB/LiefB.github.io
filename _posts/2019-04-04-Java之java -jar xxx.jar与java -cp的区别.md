---
layout:     post
title:      Java之java -jar xxx.jar与java -cp的区别
subtitle:   java -jar与java -cp
date:       2019-04-04
author:     LiefB
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Java
---



> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://www.cnblogs.com/yy3b2007com/p/9634538.html

# java -cp

java -cp 和 -classpath 一样，是指定类运行所依赖其他类的路径，通常是类库和 jar 包，需要全路径到 jar 包，多个 jar 包之间的连接符在window 下使用分号 “;”而在Linux 下使用 “:”。
windows 环境：

<pre>java -cp .;d:\work\other.jar;d:\work\my.jar packname.mainclassname </pre>

linux 环境：

```
java -cp .:/hone/myuser/work/other.jar:/hone/myuser/work/my.jar packname.mainclassname
```
表达式支持通配符，例如：

```
java -cp .;c:\work\my.jar;c:\work\*.jar packname.mainclassname 
java -cp .:/home/myuser/work/lib/my.jar:/home/myuser/work/dependency_jars/*.jar packname.mainclassname
``` 

# java -jar

```
java -jar my.jar
```

执行该命令时，会用到目录 META-INF\MANIFEST.MF 文件，在该文件中，有一个叫 Main－Class 的参数，它说明了 java -jar 命令执行的类。
java -jar 方式不可以指定附加依赖 jar 包。

# 备注
> 1\. 打包时指定了主类，可以直接用 java -jar {xxx.jar}。
> 2\. 打包时没有指定主类，可以用 java -cp {xxx.jar} {主类名称（绝对路径）}。
> 3\. 要引用其他的 jar 包，可以用 java -{[classpath|cp]} {$CLASSPATH}:{xxxx.jar} {主类名称（绝对路径）}。其中 -classpath 指定需要引入的类。