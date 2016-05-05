title: gradle build use jack
date: 2016-03-14 22:33:39
categories:
- 编程
tags:

----------------------
## 配置
1. project目录及对应module下的gradle-wrapper.properties
```xml
distributionUrl=https\://services.gradle.org/distributions/gradle-2.10-all.zip
```
2. project目录下的 gradle.build    
```xml
classpath 'com.android.tools.build:gradle:2.1.0-alpha1'
```

3. module下的gradle.build
```xml
    android{
      ...
      buildToolsVersion '24 rc1'
      ...
      defaultConfig {
        ...
       useJack = true
        ...
       }
      ...
    }
```
4. compile fileTree 要替换成 compile files

5. so 要用这种写法

 ```xml
task copyNativeLibs(type: Copy) {
      from(new File(project(':modulename').buildDir, 'libs/armeabi')){ include '.so' }
      into new File(buildDir, 'native-libs')
    }
```


## 问题
build 通过了但是 出了一个问题
```java
java.io.IOException: com.android.ide.common.process.ProcessException: org.gradle.process.internal.ExecException: Process 'command '/Library/Java/JavaVirtualMachines/jdk1.8.0_51.jdk/Contents/Home/bin/java'' finished with non-zero exit value 3

```

[google发现也有人遇到同样的问题，但是还没有解决](https://code.google.com/p/android/issues/detail?id=202973)

## 新发现
```java
Caused by: com.android.sched.util.config.ConfigurationError: Configuration has not been initialized
        at com.android.sched.util.config.UninitializedConfig.getObjectIfAny(UninitializedConfig.java:58)
        at com.android.sched.util.config.ThreadConfig.updateStatistic(ThreadConfig.java:68)
        at com.android.sched.util.config.ThreadConfig.get(ThreadConfig.java:54)
        at com.android.jack.reporting.CommonReporter.<init>(CommonReporter.java:49)
        at com.android.jack.reporting.DefaultReporter.<init>(DefaultReporter.java:32)
        at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
        at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
        at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
        at java.lang.reflect.Constructor.newInstance(Constructor.java:422)
        at com.android.sched.util.config.ReflectDefaultCtorFactory.create(ReflectDefaultCtorFactory.java:68)
        ... 9 more

```
