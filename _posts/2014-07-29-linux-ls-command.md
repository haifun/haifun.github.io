---
layout: post
title:  "Linux ls命令最详细用法介绍"
date:   2014-07-29 10:03:52
categories: Linux
---
##全部命令选项

-a 该ls命令选项可以列出目录下的所有文件，包括以 . 开头的隐含文件。

###示例：

	root@tecmint:~# ls -a

	. .gnupg .dbus .goutputstream-PI5VVW .mission-control
	.adobe deja-dup .grsync .mozilla .themes
	.gstreamer-0.10 .mtpaint .thumbnails .gtk-bookmarks .thunderbird
	.HotShots .mysql_history .htaccess .apport-ignore.xml .ICEauthority
	.profile .bash_history .icons .bash_logout .fbmessenger
	.jedit .pulse .bashrc .liferea_1.8 .pulse-cookie
	.Xauthority .gconf .local .Xauthority.HGHVWW .cache
	.gftp .macromedia .remmina .cinnamon .gimp-2.8
	.ssh .xsession-errors .compiz .gnome teamviewer_linux.deb
	.xsession-errors.old .config .gnome2 .zoncolor-b 该ls命令选项可以把文件名中不可输出的字符用反斜杠加字符编号(就象在C语言里一样)的形式列出。

-c 该ls命令选项可以输出文件的 i 节点的修改时间，并以此排序。

-d 该ls命令选项可以将目录象文件一样显示，而不是显示其下的文件。

-e 该ls命令选项可以输出时间的全部信息，而不是输出简略信息。

-f -U 该ls命令选项可以对输出的文件不排序。

-g 无用。

-i 该ls命令选项可以输出文件的 i 节点的索引信息。

-k 该ls命令选项可以以 k 字节的形式表示文件的大小。

-l 该ls命令选项可以列出文件的详细信息。

###示例：

	root@tecmint:~# ls -l

	total 40588
	drwxrwxr-x 2 ravisaive ravisaive 4096 May 8 01:06 Android Games
	drwxr-xr-x 2 ravisaive ravisaive 4096 May 15 10:50 Desktop
	drwxr-xr-x 2 ravisaive ravisaive 4096 May 16 16:45 Documents
	drwxr-xr-x 6 ravisaive ravisaive 4096 May 16 14:34 Downloads
	drwxr-xr-x 2 ravisaive ravisaive 4096 Apr 30 20:50 Music
	drwxr-xr-x 2 ravisaive ravisaive 4096 May 9 17:54 Pictures
	drwxrwxr-x 5 ravisaive ravisaive 4096 May 3 18:44 Tecmint.com
	drwxr-xr-x 2 ravisaive ravisaive 4096 Apr 30 20:50 Templates-m 该ls命令选项可以横向输出文件名，并以“，”作分格符。

-n 该ls命令选项可以用数字的GUID代替名称。

-o 该ls命令选项可以显示文件的除组信息外的详细信息。

-p -F 该ls命令选项可以在每个文件名后附上一个字符以说明该文件的类型，“*”表示可执行的普通文件；“/”表示目录；“@”表示符号链接；“|”表示FIFOs；“=”表示套接字(sockets)。

-q 该ls命令选项可以用?代替不可输出的字符。

-r 该ls命令选项可以对目录反向排序。

-s 该ls命令选项可以在每个文件名后输出该文件的大小。

-t 该ls命令选项可以以时间排序。

-u 该ls命令选项可以以文件上次被访问的时间排序。

-x 该ls命令选项可以按列输出，横向排序。

-A 该ls命令选项可以显示除 “.”和“..”外的所有文件。

-B 该ls命令选项不输出以 “~”结尾的备份文件。

-C 该ls命令选项可以按列输出，纵向排序。

-G 该ls命令选项可以输出文件的组的信息。

-L 该ls命令选项可以列出链接文件名而不是链接到的文件。

-N 该ls命令选项将不限制文件长度。

-Q 该ls命令选项可以把输出的文件名用双引号括起来。

-R 该ls命令选项可以列出所有子目录下的文件。

-S 该ls命令选项可以以文件大小排序。

-X 该ls命令选项可以以文件的扩展名(最后一个 . 后的字符)排序。

-1 该ls命令选项可以一行只输出一个文件。

–color=no 该ls命令选项可以不显示彩色文件名

–help 该ls命令选项可以在标准输出上显示帮助信息。

–version 该ls命令选项可以在标准输出上输出版本信息并退出。

###ls命令只列出子目录

1. ls -F | grep /$ 或者 alias sub = ”ls -F | grep /$”(linux)

2. ls -l | grep ”^d” 或者 ls -lL | grep ”^d” (Solaris)

###ls命令计算当前目录下的文件数和目录数

下面命令可以分别计算当前目录下的文件和目录个数：

# ls -l * |grep ”^-”|wc -l —- to count files

# ls -l * |grep ”^d”|wc -l —– to count dir

###显示彩色目录列表

打开/etc/bashrc, 加入如下一行:

alias ls=”ls –color”

下次启动bash时就可以像在Slackware里那样显示彩色的目录列表了, 其中颜色的含义如下:

* 1. 蓝色–>目录
* 2. 绿色–>可执行文件
* 3. 红色–>压缩文件
* 4. 浅蓝色–>链接文件
* 5. 灰色–>其他文件


以上就是Linux命令中ls命令的全部用法，熟练掌握它们吧！