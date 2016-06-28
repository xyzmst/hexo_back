title: 巧用 GitHub 创建自己的私人 Maven 仓库
date: 2016-06-18 13:59:47
tags:
---

### 使用 gradle-mvn-push 编译脚本
在项目根目录创建 gradle.properties 文件
```
COMPILE_SDK_VERSION=23
BUILD_TOOLS_VERSION=23.0.2
MIN_SDK_VERSION=14
TARGET_SDK_VERSION=23

VERSION_NAME=0.0.1-alpha
VERSION_CODE=1
GROUP=org.blankapp

POM_DESCRIPTION=
POM_URL=https://github.com/lijy91/BlankApp
POM_SCM_URL=https://github.com/lijy91/BlankApp
POM_SCM_CONNECTION=scm:git@github.com:lijy91/BlankApp.git
POM_SCM_DEV_CONNECTION=scm:git@github.com:lijy91/BlankApp.git
POM_LICENCE_NAME=The Apache Software License, Version 2.0
POM_LICENCE_URL=http://www.apache.org/licenses/LICENSE-2.0.txt
POM_LICENCE_DIST=repo
POM_DEVELOPER_ID=lijy91
POM_DEVELOPER_NAME=JianyingLi <lijy91@foxmail.com>

RELEASE_REPOSITORY_URL=file:///Users/Lijy91/Documents/Projects/mvn-repo/
SNAPSHOT_REPOSITORY_URL=file:///Users/Lijy91/Documents/Projects/mvn-repo/snapshots/
```


### 在 library
 创建 gradle.properties
 文件

app/library/gradle.properties

```
POM_NAME=BlankApp Library
POM_ARTIFACT_ID=blankapp
POM_PACKAGING=aar
```


### 参考链接
http://www.jianshu.com/p/d2fae8c7d93f