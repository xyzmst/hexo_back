title: 非常简单的用GitHub创建自己的私人Maven仓库
date: 2016-07-20 17:20:01
tags:
---
查了不少博客 用github搭建了个人的maven仓库，但是每次都配置好多东西，很烦，
比如需要放一个第三方的jar到仓库里方便自己引用，又不想装一堆没用的东西，看了下
maven语法 ，自己配置了下，完全没问题

### 语法
pom
```
<?xml version="1.0" encoding="UTF-8"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <modelVersion>4.0.0</modelVersion>
  <groupId>org.xyzmst</groupId>
  <artifactId>wxsdk</artifactId>
  <version>1.0</version>
  <packaging>jar</packaging>
</project>
```
groupId 这是工程组的标识,可以起个属于自己的记号，比如：org.xyzmst
artifactId 实际的名字，比如 放一个 阿狸支付宝的jar 起名：alisdk
version 版本号，没什么可说的


maven-metadata
```
<?xml version="1.0" encoding="UTF-8"?>
<metadata>
  <groupId>org.xyzmst</groupId>
  <artifactId>wxsdk</artifactId>
  <version>1.0</version>
  <versioning>
    <versions>
      <version>1.0</version>
    </versions>
    <lastUpdated>20160623090603</lastUpdated>
  </versioning>
</metadata>

```

接下来就是按照目录结构开始配置
目录结构是这样的 根据包名每层都是一个文件夹，参考 https://github.com/xyzmst/maven-repo/
根据版本命名文件夹，然后把需要的jar或者aar放到文件夹下，每个版本需要一个pom，所有的名字都是项目名连接符加版本号
比如：wxsdk-1.0.pom wxsdk-1.0.jar
项目根目录加一个maven-metadata.xml 完活




### 相关链接
- http://www.jianshu.com/p/d2fae8c7d93f

