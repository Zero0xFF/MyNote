# 在OpenJDK中寻找JDK源码

## 1.获取JDK具体版本号小b

```bat
PS [cmd_path]> java -version
java version "1.8.0_65"
Java(TM) SE Runtime Environment (build 1.8.0_65-b17)
Java HotSpot(TM) 64-Bit Server VM (build 25.65-b01, mixed mode)
# 根据上面命令可知jdk版本号是build 1.8.0_65-b17
```

## 2.在github openjdk项目上寻对应版本号的tag下载

```
https://github.com/openjdk/jdk8u/tags?after=jdk8u66-b13
注意：应该通过直接修改url参数的方式寻找tag,手动翻页效率实在太低
```

## 3. 导入JDK源码库

首先将下载的文件解压缩，得到如图所示的文件集合。

![](pic\2024-04-04 163755.png)

将jdk/src/share/classes导入jetbrain。具体方法通过流程File====>Project Structure=====>Platform Settings=====>SDKs=====>Sourcepath，向Sourcepath添加jdk/src/share/classes。

![](pic\20240404170320.png)

![](pic\20240404170422.png)
