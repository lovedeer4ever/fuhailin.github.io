---
title: 理解Linux下面的文件权限
date: 2018-12-19 20:12:31
tags: Linux
categories: Linux
top:
description: 理解Linux操作系统下文件权限的相关问题，如何在命令行下查看、修改权限，如何使用`chmod`命令
---

# 查看权限
在终端中输入`ls -l directory/filename`来查看文件的权限；
在终端中输入`ls -ld directory`来查看目录的权限；

![](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/1.png)

<!-- more -->

在命令行中查看权限的结果是这样的：

![](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/03-01-02.png)

输出的结果主要由四部分组成：
 1. `Type`: 很多种 (最常见的是 - 为文件, d 为文件夹, 其他的还有l, n … )
 2. 用户权限
 3. 接下来的数字显示的是文件的硬链接数。
 4. 最后一部分是Owner和Group所有者的格式。

 # 权限组
 在Linux下面，每个用户和目录有三种不同的用户权限组：
  - **owner**: Owner权限仅适用于文件或目录的所有者，它们不会影响其他用户的操作；
  - **group**: Group权限仅适用于已分配给文件或目录的组，它们不会影响其他用户的操作；
  - **all users**: “所有用户”权限适用于系统上的所有其他用户，这是您要注意最多的权限组。

 # 权限类型

 每一个文件或者目录又有三种基本的权限类型：
  - **read**: 读取权限是指用户读取文件内容的功能。
  - **write**: 写入权限是指用户编写或修改文件或目录的功能。
  - **execute**:  执行权限会影响用户执行文件或查看目录内容的能力。

# 修改权限

在命令行中修改权限类型的命令是**chmod**
在命令行中修改权限所有者的命令是**chown**


## 精确定义权限

精确定义权限需要定义权限组和权限类型

可以使用的权限组有：
**u** - Owner
**g** - Group
**o** - Others
**a** - All users

可以使用权限分配操作符 **+**(加权限)和 **-**(减权限)来告诉操作系统增加还是移除对应的权限。

可以使用的权限类型有：
**r** - 读取
**w** - 写去
**x** - 执行

例如，如果我把 *file1* 的权限设置为`_rw_rw_rw`，其意思就是文件`Owner、Group、all users`将具有**读取**和**写入**的权限

`chmod a-rw file1`：将*all users*的*读取、写入*权限移除

`chmod a+rw file1`: 为*all users*增加*读取、写入*权限

`chmod +rw folder/`: 为当前用户增加对 `folder`目录的 *读取、写入*权限

`chmod -R +rw folder/`: 为当前用户增加对 `folder`目录及其子文件目录的 *读取、写入*权限；`-R`:for recursive traversal of all files and subfolders

`chown -R hailin:hailingroup folder/`: 将当前目录的所有者修改为：`hailin`，用户组修改为`hailingroup`
******************

# 一个执行Python文件的技巧

如果一个` .py` 没有 `x` 权限, 在 terminal 中你就需要这样执行:

```
$ python3 t.py
This is a Python script!
```

如果你有了 `x` (可执行权限), 你运行这个文件可以直接这样打:

```
$ ./t.py
This is a Python script!
```

如果你天天要运行这个脚本, 每次运行的时候少几个字还是挺好的. 如果你决定要这样做, 你在这个 Python 脚本的开头还需要加一句话.

```py
#!/usr/bin/python3        # 这句话是为了告诉你的电脑执行这个文件的时候用什么来加载
print("This is a Python script!")
```

未完待续

**Good References:**
[Understanding Linux File Permissions](https://www.linux.com/learn/understanding-linux-file-permissions)
https://morvanzhou.github.io/tutorials/others/linux-basic/3-01-file-permissions/
