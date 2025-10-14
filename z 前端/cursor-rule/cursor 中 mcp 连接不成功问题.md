
问题：Cursor 找不到 npx 命令

这是因为 macOS 的 GUI 应用（如 Cursor）的环境变量和终端不同。让我帮您修复：

```
$ which npx

/Users/yangjiajian/.volta/bin/npx
```


完美！您使用的是 Volta 管理 Node.js，路径是 /Users/yangjiajian/.volta/bin/npx。



**方案选择：**
设置全局环境变量（推荐）
- 优点：一劳永逸，所有 GUI 应用都能访问 Volta"
- 缺点：需要注销或重启系统生效"

```
launchctl setenv VOLTA_HOME "$HOME/.volta"

launchctl setenv PATH "$VOLTA_BIN:/usr/local/bin:/System/Cryptexes/App/usr/bin:/usr/bin:/bin:/usr/sbin:/sbin"
```
