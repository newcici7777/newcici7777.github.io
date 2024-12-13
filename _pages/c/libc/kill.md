---
title: kill
date: 2024-12-13
keywords: c++, kill()
---

linux提供kill指令向程序(process)發送訊號。

語法
```
int kill(pid_t pid, int sig);
```
kill函式會將參數sig訊號傳給相關pid的程序(process)。

- pid > 0 傳給訊號給特定的pid程序。
- pid = 0 傳給訊號父pid相關的程序，包含父pid程序，此父pid程序的所有子程序都會收到sig訊號
- pid = -1 廣播所有process程序

