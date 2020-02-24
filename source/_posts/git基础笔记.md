---
title: git基础笔记
date: 2018-09-24 21:10:59
tags:
	- git
categories:
	- Linux
---
# Git




参考书籍：[git书籍](https://git-scm.com/book/zh/v2)
### 一.初始配置

git config --global user.name 'yourName'

git config --global user.email yourEmail

git config --list

### 二.git基本操作
![01_git基本操作 .jpg](01.jpeg)

### 三.版本操作

![02_git版本库操作.jpg](02.jpeg)

### 四.日志管理log

#### 4.1 显示方式（加粗表示最常用)
| 选项 | 说明 |
| --- | --- |
| -p | 补丁格式显示差异 |
| --stat | 更新文件统计 |
| --name--status | 显示文件变化 |
| **--graph** | 图形显示分支 |
| **--pretty** | 以其他格式显示，如oneline、format |
| --abbrev-commit | 显示简略SHA-1编号，也就是id号 |
| **--oneline** | 简略显示(--pretty=oneline --abbrev-commit) |


注：format格式祥见参考资料。

#### 4.2 条件筛选
| 选项 | 说明 |
| --- | --- |
| **--grep** | 正则搜索 |
| --author | 作者搜索 |
| --committer | 提交者搜索 |
| --all-match | 多条件都满足才显示 |
| --no-merges | 不显示合并操作历史 |


#### 4.3 时间筛选
| 选项 | 说明 |
| --- | --- |
| **-(n)** | 显示最近n条日志 |
| --since | 从....开始 |
| --before | 在....之前 |


注：日期格式 年-月-日

### 五.比较命令
| 具体形式 | 含义 |
| --- | --- |
| git diff (filename) | 显示暂存和工作区差异 |
| git diff commit (filename) | 显示工作区和指定版本的差异 |
| git diff -cashed commit (filename) | 显示暂存区和版本库的差异 |
| git diff commit1 commit2 (filename) | 显示版本间差异 |


### 六.SSH

#### ssh-keygen
| 参数 |
| --- |
| -b：指定密钥长度 |
| -e：读取openssh的私钥或者公钥文件 |
| -C：添加注释 |
| -f：指定用来保存密钥的文件名 |
| -i：读取未加密的ssh-v2兼容的私钥/公钥文件，然后在标准输出设备上显示openssh兼容的私钥/公钥 |
| -t：指定要创建的密钥类型 |


①ssh-keygen -t rsa -C 字符串

②把公钥复制到 github

### 七.github

#### 7.1 网页比较
| 比较类型 | 例子 | 规律 |
| --- | --- | --- |
| 分支间比较 | [https://github.com/rails/rails/](https://github.com/rails/rails/)**compare**/4-0-stable...3-2-stable | 仓库地址/compare/分支名...分支名 |
| 某一时间段前 | [https://github.com/rails/rails/compare/master@{7.day.ago}...master](https://github.com/rails/rails/compare/master@%7B7.day.ago%7D...master) | 分支名+@+时间段...分支名(同)支持：day、week、month、year |
| 指定日期之间差别 | [https://github.com/rails/rails/compare/master@{2013-01-01}...master](https://github.com/rails/rails/compare/master@%7B2013-01-01%7D...master) |  |


