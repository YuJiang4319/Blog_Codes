---
title: Git工作流常用命令
date: 2024-08-05 16:25:38
tags:
  - Git
categories:	
  - Program
description: 
---

#### 配置基本用户信息：

```git
git config --global user.name <你的用户名>
git config --global user.email <你的邮箱地址>
```

#### 创建一个新仓库：

```git
git init
```

#### 从远程服务器克隆一个仓库：

```
git clone <远程仓库URL>
```

#### 显示当前工作目录下的文件提交状态：

```
git status
```

#### 将指定文件Stage（放入暂存区）：

```git
git add <文件路径>   //放入单个文件
git add .			//放入所有变化的文件
```

#### 将指定文件Unstage（移出暂存区）：

```
git reset <文件路径>
```

#### 创建一个提交到本地仓库并提供提交信息：

```
git commit -m "提交说明信息"
```

#### 显示提交历史：

```
git log
```

#### 向远程仓库推送：

```
git push
```

#### 从远程仓库拉取：

```
git pull
```

