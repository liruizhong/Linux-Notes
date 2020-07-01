### grep 命令使用总结

#### 1. grep 时排除部分目录
排除 `logs` `bin` 目录 
```shell
grep -r 'access_rotate="day"' --exclude-dir=logs --exclude-dir=bin
```
