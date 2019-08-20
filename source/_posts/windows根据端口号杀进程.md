---
title: Windows运维脚本
categories: windows
tags:
- 运维脚本
---


# windows根据端口号,查杀进程

```

@echo off
for /f "tokens=5" %%i in ('netstat -aon ^| findstr ":8086"') do (
    set n=%%i
)
taskkill /f /pid %n%
```


