## tomcat 安装javaagent并启动远程调试
配置catalina.bat
```bash
setlocal
set "%JAVA_OPTS%"
set "JAVA_OPTS= -javaagent:D:\workspace\xxxx\xxx\xxxx\agent\rasp.jar %JAVA_OPTS%" 
```

使用jpda start的方式启动tomcat
```bash
catalina.bat jpda start
```