---
title: Linux常用command笔记
date: 2019-02-18 20:05:47
tags: Linux
categories: Linux
top:
---
我安装的Linux发行版本是Ubuntu 16.04和Ubuntu 18.04
下面记录一些使用过程中遇到的Linux命令：

<!-- more -->

### 删除软件

```
sudo apt-get remove <application_name>
```

```
sudo apt-get purge <package-name>
```


### 解压*.tar.xz文件
`tar -xf file.tar.xz`
https://scottlinux.com/2014/01/07/extracting-or-uncompressing-tar-xz-files-in-linux/

#### 压缩文件成*.tar.xz
`tar -zcvf archive-name.tar.gz directory-name`

### 删除目录或文件
删除空文件夹：`rmdir directoryname`
Remove a directory with files and subdirectories (non-empty directory)：`rm -r directoryname`
Remove a single file：`rm file.txt`

### `mv`移动或重命名文件或目录
`mv` can do two jobs.

 1. It can move files or directories
 2. It can rename files or directories

To just rename a file or directory type this in Terminal:

> mv old_name new_name

with space between the old and new names.

To move a file or directory type this in Terminal.

> mv file_name ~/Desktop

it will move the file to the desktop.

If is a directory you should add `-R` before the directory name:

> mv -R directory_name ~/Desktop
### `wget`下载命令
https://www.cnblogs.com/wuheng1991/p/5332764.html
> wget http://cn.wordpress.org/wordpress-3.1-zh_CN.zip
### `cp`复制命令
> cp filename direction
> cp folder direction

### 新建文件
> vi filename     :打开或新建文件，并将光标置于第一行首

查看显卡型号：`lspci |grep VGA` （lspci是linux查看硬件信息的命令），屏幕会打印出主机的集显几独显信息
![这里写图片描述](2018071513101933)

查看nvidia芯片信息：`lspci |grep -i nvidia`，会打印出nvidia系列的硬件信息，如果主机安装了没有视频输出的GPU（如tesla系列），这个命令会很有用
![这里写图片描述](20180715131139920)

### DiskUsage du
查看硬盘使用情况
`df -hl` 查看磁盘剩余空间

`df -h` 查看每个根路径的分区大小;`-h`:human readable

`du -sh` [目录名] ：**D**isk **U**sage返回该目录的大小

`du -sm` [文件夹] 返回该文件夹总M数
![这里写图片描述](20180718110351393)

### `top`
查看CPU使用情况
`top`
![这里写图片描述](20180718150831127)
`top` `1`
![这里写图片描述](20180718150928345)

### `tail` 命令
https://www.cnblogs.com/mfryf/p/3336804.html
`tail -f filename`：监视filename文件的尾部内容（默认10行，相当于增加参数 -n 10），刷新显示在屏幕上。退出，按下CTRL+C。

`tail -n 20 filename`：显示filename最后20行。

`tail -r -n 10 filename`：逆序显示filename最后10行。

### `pwd`
print working directory

http://linux.51yip.com/search/awk

### `md5sum` : MD5算法一般用于检查文件完整性，不同的文件内容生成相同的报文摘要的概率是极其小的。
`md5sum filename`
![这里写图片描述](20180809174303523)

### `scp`
secure copy，远程拷贝文件

 1. **将本地文件上传到服务器上**
`scp -P 2222 /home/lnmp0.4.tar.gz root@www.vpser.net:/root/lnmp0.4.tar.gz`
![这里写图片描述](20180809180429836)

### `unzip`
解压zip文件
`unzip file.zip`
`unzip file.zip -d destination_folder`

### `diff`
比较文件不同
`diff file1 file2`

### `taskset`
指定job在哪几块CPU上运行
`taskset -c 0-7`：指定job在1-8号CPU上运行

### `nohup`
allows to run job in the background after you log out from a shell
[Nohup Command in Linux](https://linuxhint.com/nohup_command_linux/)

### `cat`
**cat**enate 命令用于连接文件并打印到标准输出设备上
`cat file.txt`：将*file.txt*的内容打印在屏幕上
[13 Basic Cat Command Examples in Linux](https://www.tecmint.com/13-basic-cat-command-examples-in-linux/)

### `touch`
创建空文件，或者改变文件的时间戳属性
`touch file.txt`：创建一个新的空文件*file.txt*

### 查看NVIDIA显卡信息
由于我已经切换到ＮＶＩＤＩＡ专有驱动：`nvidia-smi`
![驱动](20171228211613210)

![nvidia-smi](20171228211819829)

`watch -n 5 nvidia-smi`:每隔5秒更新一下显卡使用情况, `ctrl+c`退出

### `more`
more 允许你向前查看文本文件。
`more file.txt`：创建一个新的空文件*file.txt*
使用<kbd>Enter</kbd>可以向下翻页，输入 <kbd>q</kbd> 可以退出，输入 <kbd>/</kbd> 字符并在其后加上你想要查找的文字(**区分大小写**)可以搜索。例如你要查看的字段是 “terminal”，只需输入：`/terminal`

### 管道符<kbd>|</kbd>
将管道符<kbd>|</kbd>左边命令的输出输入给右边的命令
`ls | more`：有很多文件的目录，可以组合 `more` 跟 `ls` 命令完整查看这个目录当中的内容
`grep ‘productivity’ core.md Dict.md lctt2014.md lctt2016.md lctt2018.md README.md | more` ：组合 more 和 grep 命令，实现在多个文件中找到指定的文本 “productivity”
`ps -u Hailin | more`：列出你用户下(Hailin)正在运行的进程