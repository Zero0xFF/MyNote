# Tomcat远程调试

## 添加调试参数

向tomcat的home目录下bin目录的名为catalina.sh/catalina.bat添加如下参数

```
CATALINA_OPTS='-server -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=12345'
```



