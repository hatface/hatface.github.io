## Java知识随手记
- 普通jar包（区别于springboot的打包方式）获取jar路径
```java
        String baseDir;
        String jarPath = ReportController.class.getProtectionDomain().getCodeSource().getLocation().getFile();
        try {
            baseDir = URLDecoder.decode(new File(jarPath).getParent(), "UTF-8");
        } catch (UnsupportedEncodingException e) {
           baseDir = new File(jarPath).getParent();
        }
        dbPath = new File(baseDir, "db").getCanonicalPath();
```

- 读取resources目录下的文件
```java
InputStream path = this.getClass().getResourceAsStream("/data.txt");
BufferedReader reader = new BufferedReader(new InputStreamReader(path));
```

## ThreadLocal
使用ThreadLocal可以获得一个只在特定线程才有的变量

### 原理
由于每一个Thread对象有一个ThreadMap成员变量，ThreadLocal使用Thread.currentThread();获得当前的Thread对象，并获得Thread对象的
ThreadMap对象，所以可以起到使用ThreadLocal可以获得一个只有在特定线程才有的变量。


## NIO
参考
```bash
https://ifeve.com/overview/
```