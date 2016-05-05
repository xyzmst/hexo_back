title: Android Studio Gradle使用笔记
date: 2016-04-08 16:09:33
tags:
---
## 定义
Gradle是一个基于Apache Ant和Apache Maven概念的项目自动化建构工具。

## gradle vs maven

## groovy
groovy是一种基于java的动态语言。它也是跑在jvm上的。groovy为什么能跑在java的虚拟机上呢？这听上去让人有些难以理解。实际上，在groovy的编译阶段，groovy的编译器就会把groovy源码编译成class文件，class文件？没错！就是class文件。我们知道，java在编译后也是编译成class文件，然后交给虚拟机去执行的。换句话说，jvm的输入是class文件，只要你是标准的class文件，jvm是不会管你是由什么语言编译来的，它都会执行！
- 支持动态类型
 ```groovy
  def temp = "groovy" //def是groovy中定义变量的关键字
  temp = 1 //groovy可以不用分号结尾～
  println temp
 ```
- 函数
 ```groovy
  def getValue(){
    retrun "hello world"
  }
 ```
- 闭包
遍历一个 Collection
```groovy
  def list = ["gradle", "groovy", "closure"]
  //{}内的代码块就是一个闭包
  list.each{
      println it//闭包中的it是一个关键字，指向被调用的外部集合的每个值。
  }
```
输出结果
```groovy
  gradle
  groovy
  closure
```
- 遍历map
```groovy
  //[key:value,key:value]代表定义一个map
  def hash = [name:"Andy", "VPN-#":45]
  hash.each{ key, value ->//传递给闭包两个参数
      println "${key} : ${value}"
  }
```
.each 定义
```groovy
public static <T> Iterable<T> each(Iterable<T> self, @ClosureParams(FirstParam.FirstGenericType.class) Closure closure) {
  each(self.iterator(), closure);
  return self;
}
```
当我们在调用.each方法时，第一个参数self传进去代表调用each方法的变量，第二个参数代表each{}包裹起来的代码块。而在groovy中，当函数最后的一个参数是闭包的话，可以省略圆括号。（有同学可能会问each方法的定义明明有两个参数，但在调用时只传入了一个closure参数，没有传入self参数啊。对于这个问题，在这里我们不深究了，可以简单的认为在编译时，groovy编译器会帮我们处理。）

## gradle

### 工作流程

![流程](http://7xrsw2.com1.z0.glb.clouddn.com/image028.png)

1.初始化阶段(Initialization Phases):执行setting.gradle。settings.gradle会被转化为Settings的对象.settings.gradle决定哪个project应该加入构建。对于每一个工程创建一个Project对象。

2.配置阶段(Configuration Phases):在这个阶段，所有参与构建的工程的build.gradle脚本将会被执行。task也是在这个阶段定义的,并被添加到一个有向图中。

3.执行阶段(Execution Phases):Gradle会执行指定的task（例如，在命令行中执行gradle build，将会执行build这个task）
## android gradle 构建系统

```
buildscript {  
    repositories {  
        mavenCentral()  
    }      
    dependencies {  
        classpath 'com.android.tools.build:gradle:0.11.1'  
    }  
}  

apply plugin: 'android'  

android {  
    compileSdkVersion 19  
    buildToolsVersion "19.0.0"  
}  
```


## 相关链接
gradle入门 : http://yoursite.com/2016/03/12/gradlestart/index.html
http://blog.csdn.net/lzyzsd/article/details/42131637
深入浅出gradle（一）：http://blog.csdn.net/lzyzsd/article/details/42131637
