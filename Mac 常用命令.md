

### address already in use

```
lsof -i tcp:3009 // 找到端口进程
kill 51392 // 杀死该进程
```
