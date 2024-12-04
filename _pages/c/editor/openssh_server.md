---
title: 安裝openssh-server
date: 2024-12-02
keywords: Ubuntu, openssh-server
---

## openssh-server

以下環境

本機MAC

Ubuntu 24.04

VMware Fusion

以下為Ubuntu虛擬機終端機設定

- 更新package list
```
$ sudo apt update
```
- 安裝openssh-server
```
$ sudo apt install openssh-server
```
按下y

- 啟動ssh
```
$ sudo systemctl status ssh
```
離開按q
```
$ sudo systemctl enable --now ssh
```
- 本機連ssh
```
$ ssh localhost
```
按下y

- 重新啟動ssh
$ sudo systemctl restart ssh

## 安裝c++
```
$ sudo apt install gcc
$ sudo apt install g++
$ sudo apt install build-essential
$ gcc --version
$ g++ --version
```
## 安裝man page
```
$ sudo apt install manpages-dev manpages-posix-dev
$ man strcpy
```
按q退出
若跟user command有衝突的函式，請在函式名前輸入3
```
$ man 3 sleep
```
1是user command
3是Standard C library