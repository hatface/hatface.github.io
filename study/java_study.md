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