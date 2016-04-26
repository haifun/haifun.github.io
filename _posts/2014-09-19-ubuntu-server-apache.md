---
layout: post
title:  "ubuntu server apache"
date:   2014-09-19 10:36:52
categories: git
---

在Ubuntu中安装apache

　　安装指令：sudo apt-get install apache2

　　安装结束后：

　　产生的启动和停止文件是：/etc/init.d/apache2

　　启动：sudo apache2ctl -k start

　　停止：sudo apache2ctl -k stop

　　重新启动：sudo apache2ctl -k restart

　　配置文件保存在：/etc/apache2

 需要说明的是，普通的apache发行版本配置文件是：

　　httpd.conf

　　Ubuntu发行版本的主配置文件是：

　　apache2.conf

　　在apache2.conf引用到了以下文件：

　　# 包含动态模块的配置:

　　Include /etc/apache2/mods-enabled/*.load

　　Include /etc/apache2/mods-enabled/*.conf

　　# 包含用户自己的配置:

　　Include /etc/apache2/httpd.conf

　　# 包含端口监听的配置:

　　Include /etc/apache2/ports.conf

　　# 包含一般性的配置语句片断:

　　Include /etc/apache2/conf.d/

　　# 包含虚拟主机的配置指令:

　　Include /etc/apache2/sites-enabled/

　　修改httpd.conf

　　增加以下内容：

　　ServerName 127.0.0.1:80


ubuntu apache2配置

1.apache2.conf 是主配置文件，httpd.conf 用户配置文件

2.虚拟目录在 httpd.conf 中

    <VirtualHost *>
        DocumentRoot "路径"
        ServerName 名称
        <Directory "路径"> allow from all Options +Indexes </Directory>
    </VirtualHost>
    
3.根设置（默认主目录）在 /etc/apache2/sites-available/default
4.重启命令

    sudo /etc/init.d/apache2 restart或者
    cd /etc/init.d
    sudo apache2 -k restart
    stop 停止；start 启动5.日志文件在 /var/log/apache2/
    <VirtualHost *:80>
    ServerName www.kimoqi.com
    DocumentRoot /home/vsftpd/kimoqi
    </VirtualHost>
    <VirtualHost *:80>
    ServerName www.arwenedu.com
    DocumentRoot /home/vsftpd/wangguan/webapps
    </VirtualHost>
    <VirtualHost *:80>
    ServerName www.arwenedu.org.cn
    DocumentRoot /home/vsftpd/wangguan/chem
    </VirtualHost>
    
    vi /etc/httpd/conf/httpd.conf

在Windows下，Apache的配置文件通常只有一个，就是httpd.conf。但我在Ubuntu Linux上用apt-get install apache2命令安装了Apache2后，竟然发现它的httpd.conf（位于/etc/apache2目录）是空的！进而发现Ubuntu的 Apache软件包的配置文件并不像Windows的那样简单，它把各个设置项分在了不同的配置文件中，看起来复杂，但仔细想想设计得确实很合理。

严格地说，Ubuntu的Apache（或者应该说Linux下的Apache？我不清楚其他发行版的apache软件包）的配置文件是 /etc/apache2/apache2.conf，Apache在启动时会自动读取这个文件的配置信息。而其他的一些配置文件，如 httpd.conf等，则是通过Include指令包含进来。在apache2.conf中可以找到这些Include行：

引用

 # Include module configuration:
Include /etc/apache2/mods-enabled/*.load
Include /etc/apache2/mods-enabled/*.conf

 # Include all the user configurations:
Include /etc/apache2/httpd.conf

 # Include ports listing
Include /etc/apache2/ports.conf
……

 # Include generic snippets of statements
Include /etc/apache2/conf.d/

 # Include the virtual host configurations:
Include /etc/apache2/sites-enabled/
结合注释，可以很清楚地看出每个配置文件的大体作用。当然，你完全可以把所有的设置放在apache2.conf或者httpd.conf或者任何一个配置文件中。Apache2的这种划分只是一种比较好的习惯。

安装完Apache后的最重要的一件事就是要知道Web文档根目录在什么地方，对于Ubuntu而言，默认的是/var/www。怎么知道的呢？ apache2.conf里并没有DocumentRoot项，httpd.conf又是空的，因此肯定在其他的文件中。经过搜索，发现在 /etc/apache2/sites-enabled/000-default中，里面有这样的内容：

引用

    NameVirtualHost *
    <VirtualHost *>
    ServerAdmin webmaster@localhost
    
    DocumentRoot /var/www/
……
这是设置虚拟主机的，对我来说没什么意义。所以我就把apache2.conf里的Include /etc/apache2/sites-enabled/一行注释掉了，并且在httpd.conf里设置DocumentRoot为我的用户目录下的某 个目录，这样方便开发。

再看看/etc/apache2目录下的东西。刚才在apache2.conf里发现了sites-enabled目录，而在 /etc/apache2下还有一个sites-available目录，这里面是放什么的呢？其实，这里面才是真正的配置文件，而sites- enabled目录存放的只是一些指向这里的文件的符号链接，你可以用ls /etc/apache2/sites-enabled/来证实一下。所以，如果apache上配置了多个虚拟主机，每个虚拟主机的配置文件都放在 sites-available下，那么对于虚拟主机的停用、启用就非常方便了：当在sites-enabled下建立一个指向某个虚拟主机配置文件的链 接时，就启用了它；如果要关闭某个虚拟主机的话，只需删除相应的链接即可，根本不用去改配置文件。

mods-available、mods-enabled和上面说的sites-available、sites-enabled类似，这两个目录 是存放apache功能模块的配置文件和链接的。当我用apt-get install php5安装了PHP模块后，在这两个目录里就有了php5.load、php5.conf和指向这两个文件的链接。这种目录结果对于启用、停用某个 Apache模块是非常方便的。

最后一个要说的是ports.conf，这里面设置了Apache使用的端口。如果需要调整默认的端口设置，建议编辑这个文件。或者你嫌它实在多 余，也可以先把apache2.conf中的Include /etc/apache2/ports.conf一行去掉，在httpd.conf里设置Apache端口。

ubuntu里缺省安装的目录结构很有一点不同。在ubuntu中module和 virtual host的配置都有两个目录，一个是available，一个是enabled，available目录是存放有效的内容，但不起作用，只有用ln 连到enabled过去才可以起作用。对调试使用都很方便，但是如果事先不知道，找起来也有点麻烦。

/etc/apache2/sites-available 里放的是VH的配置，但不起作用，要把文件link到 sites-enabled 目录里才行。

<VirtualHost *>  
        ServerName 域名  
  
        DocumentRoot 把rails项目里的public当根目录  
        <Directory public根目录>  
                Options ExecCGI FollowSymLinks  
                AllowOverride all  
                allow from all  
                Order allow,deny  
        </Directory>  
        ErrorLog /var/log/apache2/error-域名.log  
</VirtualHost>
　　进一步的配置和使用，就可以查阅APACHE的手册了




Apache配置文件httpd.conf说明 

DocumentRoot "/var/www/html" ---Apache默认服务器主目录路径

DirectoryIndex index.html index.htm index.php index.html.var ---默认文档，多个文件之间用空格分开

Listen 192.168.1.1:80       设置监听ip是192.168.1.1的地址和端口为80

Listen 192.168.1.2:8080     设置监听ip是192.168.1.2的地址和端口为8080

ServerRoot "/etc/httpd"     设置相对根目录的路径 ，通常是指存放配置文件和日志文件的地方。缺省是：/etc/httpd 一般包括conf和logs子目录

ErrorLog logs/error_log     设置错误日志    注意：如果日志文件存放路径不是以“/”开头，意味著该文件是相对于 ServerRoot目录

CustomLog logs/access_log combined      访问日志      （combined指明日志使用的格式，还有common格式）

ServerAdmin lindenstar@163.com     设置网络管理员的Email    -当客户端服务器发生错误时，服务器通常会向客户端返回错误提示页面，为了方便解决错误，这个网页中通常有管理员的Email地址，可以通过使用 ServerAdmin语句来设置管理员的EMail地址

ServerName www.iigoogle.com:80       设置服务器主机名称 (如果有域名可以填入域名，没有域名则可填入服务器IP地址)

AddDefaultCharset GB2312           设置默认字符集，定义服务器返回给客户机默认字符集(由于西欧UTF-8是Apache默认字符集，因此当访问有中文的网页时会出现乱码，这时只要将字符集改成GB2312，再重启Apache服务即可)

Alias /down    "/software /download"     创建虚拟目录（创建名为down的虚拟目录，它对应的物理路径是：/software /download）
Alias /ftp     "/var/ftp"                创建虚拟目录（创建名为ftp的虚拟目录，它对应的物理路径是：/var/ftp）

<Directory "/var/www/html">       设置目录权限（<Directory "目录路径">此次写设置目录权限的语句</Directory>）
      Options FollowSymLinks        page:116
      AllowOverride None
</Directory>


基于域名的虚拟主机 
NameVirtualHost 220.123.55.99       ---先用NameVirtualHost指令指定哪个IP地址负责响应对虚拟主机的请求

    <VirtualHost www.iigoogle.com>
          ServerName www.iigoogle.com:80
          ServerAdmin iigoogle@163.com
          DocumentRoot /www/docs/iigoogle
          DirectoryIndex index.jsp   
          ErrorLog logs/www/iigoogle/error_log
          CustomLog logs/www/iigoogle/access_log common
    </VirtualHost>
    
另一种写法

    NameVirtualHost 220.123.55.99:80    
    <VirtualHost www.iigoogle.com:80>
          ServerName www.iigoogle.com
          ServerAdmin iigoogle@163.com
          DocumentRoot /www/docs/iigoogle.com    
          ErrorLog logs/www/iigoogle/error_log
          CustomLog logs/www/iigoogle/access_log common
    </VirtualHost>
