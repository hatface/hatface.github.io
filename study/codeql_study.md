## 安装环境

1、下载vscode插件
```html
https://github.com/github/vscode-codeql/releases/tag/v1.3.5
```

2、下载codeql-cli
```html
https://github.com/github/codeql-cli-binaries
```
添加codeql到path环境变量下

3、设置vscode插件
3.1、设置codeql-cli的路径


4、clone starter工程
```html
git clone https://github.com/github/vscode-codeql-starter.git
git submodule update --remote //可以下载所有的库
```

## 开始分析

1、建立数据库
在需要分析的代码根目录执行如下的代码
```bash
codeql database create jstest --language=javascript
```

--command 可以自定义命令
```bash
codeql database create jstest --language=javascript --command "mvn package"
```
