---
title: kill
date: 2024-12-13
keywords: c++, kill()
---

linux提供kill指令向進程(process)發送訊號。

語法
```
int kill(pid_t pid, int sig);
```
kill函式會將參數sig訊號傳給相關pid的進程(process)。

- pid > 0 傳給訊號給特定的pid進程。
- pid = 0 傳給訊號父pid相關的進程，包含父pid進程，此父pid進程的所有子進程都會收到sig訊號
- pid = -1 廣播所有process進程

