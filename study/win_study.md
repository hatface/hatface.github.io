## 任务管理
```bash
wmic process where caption="java.exe" get processid,caption,commandline /value 打印java.exe运行的程序的命令行，进程id等
taskkill /pid 12345 杀死12345进程号的进程
tasklist | findStr "java" 查看java的进程

```
