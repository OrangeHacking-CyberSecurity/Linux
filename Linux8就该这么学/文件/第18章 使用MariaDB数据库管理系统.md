# [第18章 使用MariaDB数据库管理系统](https://www.linuxprobe.com/basic-learning-18.html)

**章节简述：**

MySQL数据库项目自从被Oracle公司收购之后，从开源软件转变成为了“闭源”软件，这导致IT行业中的很多企业以及厂商纷纷选择使用了数据库软件的后起之秀—MariaDB数据库管理系统。MariaDB数据库管理系统也因此快速占据了市场。

本章将介绍数据库以及数据库管理系统的理论知识，然后再介绍MariaDB数据库管理系统的内容，最后将通过动手实验的方式，帮助各位读者掌握MariaDB数据库管理系统的一些常规操作。比如，账户的创建与管理、账户权限的授权；新建数据库、新建数据库表单；对数据库执行新建、删除、修改和查询等操作。最后还介绍了数据库的备份与恢复方法，不仅能做到“增删改查”，更能胜任生产环境中的数据库管理工作。

本章目录结构

- [18.1 数据库管理系统](https://www.linuxprobe.com/basic-learning-18.html#181)
- [18.2 初始化mariaDB服务](https://www.linuxprobe.com/basic-learning-18.html#182_mariaDB)
- [18.3 管理用户以及授权](https://www.linuxprobe.com/basic-learning-18.html#183)
- [18.4 创建数据库与表单](https://www.linuxprobe.com/basic-learning-18.html#184)
- [18.5 管理表单及数据](https://www.linuxprobe.com/basic-learning-18.html#185)
- [18.6 数据库的备份及恢复](https://www.linuxprobe.com/basic-learning-18.html#186)

##### **18.1 数据库管理系统**

数据库是指按照某些特定结构来存储数据资料的数据仓库。在当今这个大数据技术迅速崛起的年代，互联网上每天都会生成海量的数据信息，数据库技术也从最初只能存储简单的表格数据的单一集中存储模式，发展到了现如今存储海量数据的大型分布式模式。在信息化社会中，能够充分有效地管理和利用各种数据，挖掘其中的价值，是进行科学研究与决策管理的重要前提。同时，数据库技术也是管理信息系统、办公自动化系统、决策支持系统等各类信息系统的核心组成部分，是进行科学研究和决策管理的重要技术手段。

数据库管理系统是一种能够对数据库中存放的数据进行建立、修改、删除、查找、维护等操作的软件程序。它通过把计算机中具体的物理数据转换成适合用户理解的抽象逻辑数据，有效地降低数据库管理的技术门槛，因此即便是从事[Linux](https://www.linuxprobe.com/)运维工作的工程师也可以对数据库进行基本的管理操作。但是，[刘遄](https://www.linuxprobe.com/)老师有必要提醒各位读者，本书的技术主线依然是[Linux系统](https://www.linuxprobe.com/)的运维，而数据库管理系统只不过是在此主线上的一个内容不断横向扩展、纵向加深的分支，不能指望在一两天之内就可以精通数据库管理技术。如果有读者在学完本章内容之后对数据库管理技术产生了浓厚兴趣，并希望谋得一份相关的工作，那么就需要额外为自己定制一个学习规划了。

![第18章 使用MariaDB数据库管理系统第18章 使用MariaDB数据库管理系统](https://www.linuxprobe.com/wp-content/uploads/2015/10/marialANDmysql.png)

图18-1 MariaDB与Mysql数据库管理系统著名LOGO

这种凤凰涅槃般的浴火重生，是由MySQL项目创始人Michael Widenius带领着团队完成的。根据MariaDB官网的介绍，他有两个可爱的小天使女儿，大女儿叫做My，而二女儿叫做Maria，因此这也是第二款他用亲人名字命名的软件。

```
Why is the Software Called MariaDB?

The 'MySQL' name is trademarked by Oracle, and they have chosen to keep that trademark to themselves. The name MySQL (just like the MyISAM storage engine) comes from Monty's first daughter My. The first part of 'MySQL' is pronounced like the English adjective, even if this doesn't match the correct pronunciation of the Finnish name.

MariaDB continues this tradition by being named after his younger daughter, Maria.

The name Maria was initially given to a storage engine. After MariaDB was started, to avoid confusion, it was renamed to Aria. The new name was decided as a result of a contest.
```

既然是讲解数据库管理技术，就肯定绕不开MySQL。MySQL是一款市场占有率非常高的数据库管理系统，技术成熟、配置步骤相对简单，而且具有良好的可扩展性。但是，由于Oracle公司在2009年收购了MySQL的母公司Sun，因此MySQL数据库项目也随之纳入Oracle麾下，逐步演变为保持着开源软件的身份，但又申请了多项商业专利的软件系统。开源软件是全球黑客、极客、程序员等技术高手在开源社区的大旗下的公共智慧结晶，自己的劳动成果被其他公司商业化自然也伤了一大批开源工作者的心，因此由MySQL项目创始者重新研发了一款名为MariaDB的全新数据库管理系统。

该软件当前由开源社区进行维护，是MySQL的分支产品，而且几乎完全兼容MySQL。MariaDB与MySQL具有高度的兼容性，具有库二进制奇偶校验的直接替换功能，与Mysql API和[命令](https://www.linuxcool.com/)均保持一致。并且MariaDB还自带了一个新的存储引擎Aria，用来替代MyISAM，与MySQL一样好用。

与此同时，由于各大公司之间存在着竞争关系或利益关系，外加MySQL在被收购之后逐渐由开源向闭源软件转变，很多公司抛弃了MySQL。当前，谷歌、维基百科等技术领域决定将MySQL数据库上的业务转移到MariaDB数据库，Linux开源系统的领袖[红帽](https://www.linuxprobe.com/)公司也决定在RHEL 8、[CentOS](https://www.linuxprobe.com/) 8以及最新的Fedora系统中，将MariaDB作为默认的数据库管理系统，而且[红帽](https://www.linuxprobe.com/)公司更是将数据库知识加入到了[RHCE](https://www.linuxprobe.com/)认证的考试内容中。随后，还有数十个常见的Linux系统（如openSUSE、Slackware等）也作出了同样的表态。

但是，坦白来讲，虽然IT行业巨头都决定采用MariaDB数据库管系统，这并不意味着MariaDB较之于MySQL有明显的优势。刘遄老师用了近两周的时间测试了MariaDB与MySQL的区别，并进行了多项性能测试，并没有发现媒体所说的那种明显的优势。可以说，MariaDB和MySQL在性能上基本保持一致，两者的操作命令也十分相似。从务实的角度来讲，在掌握了MariaDB数据库的命令和基本操作之后，在今后的工作中即使遇到MySQL数据库，也可以快速上手。

##### **18.2 初始化mariaDB服务**

相较于MySQL，MariaDB数据库管理系统有了很多新鲜的扩展特性，例如对微秒级别的支持、线程池、子查询优化、进程报告等。在配置妥当软件仓库后，即可安装部署MariaDB数据库主程序及服务端程序了。

```
[root@linuxprobe ~]# dnf install -y mariadb mariadb-server
Updating Subscription Management repositories.
Unable to read consumer identity
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
Last metadata expiration check: 0:00:19 ago on Tue 27 Apr 2021 05:04:27 PM CST.
Dependencies resolved.
===========================================================================================
 Package                    Arch   Version              Repository             Size
===========================================================================================
Installing:
 mariadb                    x86_64 3:10.3.11-1.module+el8+2765+cfa4f87b    AppStream 6.2 M
 mariadb-server             x86_64 3:10.3.11-1.module+el8+2765+cfa4f87b    AppStream  16 M
Installing dependencies:
 mariadb-common             x86_64 3:10.3.11-1.module+el8+2765+cfa4f87b    AppStream  62 k
 mariadb-connector-c        x86_64 3.0.7-1.el8                             AppStream 148 k
 mariadb-connector-c-config noarch 3.0.7-1.el8                             AppStream  13 k
 mariadb-errmsg             x86_64 3:10.3.11-1.module+el8+2765+cfa4f87b    AppStream 232 k
 perl-DBD-MySQL             x86_64 4.046-2.module+el8+2515+0650e81c        AppStream 156 k
Installing weak dependencies:
 mariadb-backup             x86_64 3:10.3.11-1.module+el8+2765+cfa4f87b    AppStream 6.2 M
 mariadb-gssapi-server      x86_64 3:10.3.11-1.module+el8+2765+cfa4f87b    AppStream  49 k
 mariadb-server-utils       x86_64 3:10.3.11-1.module+el8+2765+cfa4f87b    AppStream 1.6 M
Enabling module streams:
 mariadb                           10.3                                                   
 perl-DBD-MySQL                    4.046                                                  

Transaction Summary
===========================================================================================
Install  10 Packages
………………省略部分输出信息………………
Installed:
  mariadb-3:10.3.11-1.module+el8+2765+cfa4f87b.x86_64                                      
  mariadb-server-3:10.3.11-1.module+el8+2765+cfa4f87b.x86_64                               
  mariadb-backup-3:10.3.11-1.module+el8+2765+cfa4f87b.x86_64                               
  mariadb-gssapi-server-3:10.3.11-1.module+el8+2765+cfa4f87b.x86_64                        
  mariadb-server-utils-3:10.3.11-1.module+el8+2765+cfa4f87b.x86_64                         
  mariadb-common-3:10.3.11-1.module+el8+2765+cfa4f87b.x86_64                               
  mariadb-connector-c-3.0.7-1.el8.x86_64                                                   
  mariadb-connector-c-config-3.0.7-1.el8.noarch                                            
  mariadb-errmsg-3:10.3.11-1.module+el8+2765+cfa4f87b.x86_64                               
  perl-DBD-MySQL-4.046-2.module+el8+2515+0650e81c.x86_64                                   

Complete!
```

在安装完毕后，记得启动服务程序，并将其加入到开机启动项中：

```
[root@linuxprobe ~]# systemctl start  mariadb 
[root@linuxprobe ~]# systemctl enable mariadb 
Created symlink /etc/systemd/system/mysql.service → /usr/lib/systemd/system/mariadb.service.
Created symlink /etc/systemd/system/mysqld.service → /usr/lib/systemd/system/mariadb.service.
Created symlink /etc/systemd/system/multi-user.target.wants/mariadb.service → /usr/lib/systemd/system/mariadb.service.
```

在确认MariaDB数据库软件程序安装完毕并成功启动后请不要立即使用。为了确保数据库的安全性和正常运转，需要先对数据库程序进行初始化操作。这个初始化操作涉及下面5个步骤。

> 1：设置root管理员在数据库中的密码值；
>
> （注意，该密码并非root管理员在系统中的密码，这里的密码值默认应该为空，可直接按回车键）
>
> 2：设置root管理员在数据库中的专有密码；
>
> 3：删除匿名账户，并使用root管理员从远程登录数据库，以确保数据库上运行的业务的安全性；
>
> 4：删除默认的测试数据库，取消测试数据库的一系列访问权限；
>
> 5：刷新授权列表，让初始化的设定立即生效。

对于上述数据库初始化的操作步骤，刘遄老师已经在下面的输出信息旁边进行了简单注释，确保各位读者更直观地了解要输入的内容：

```
[root@linuxprobe ~]# mysql_secure_installation 

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): 输入管理员原始密码，默认为空值，直接回车即可
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n] y（设置管理员密码）
New password: 输入新的密码
Re-enter new password: 再次输入密码
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y（删除匿名账户）
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y（禁止管理员从远程登录）
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y（删除测试数据库及其访问权限）
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y（刷新授权表，让初始化后的设定立即生效）
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

在很多生产环境中都需要使用站库分离的技术（即网站和数据库不在同一个服务器上），如果需要让root管理员远程访问数据库，可在上面的初始化操作中设置策略，以允许root管理员从远程访问。然后还需要设置防火墙，使其放行对数据库服务程序的访问请求，数据库服务程序默认会占用3306端口，在防火墙策略中服务名称统一叫作mysql：

```
[root@linuxprobe ~]# firewall-cmd --permanent --add-service=mysql
success
[root@linuxprobe ~]# firewall-cmd --reload
success
```

一切准备就绪。现在我们将首次登录MariaDB数据库。管理数据库的命令为mysql，其中，“-u”参数用来指定以root管理员的身份登录，而“-p”参数用来验证该用户在数据库中的密码值。

```
[root@linuxprobe ~]# mysql -u root -p
Enter password: 输入刚才设置的管理员密码后敲击回车
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 16
Server version: 10.3.11-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

初次使用数据库管理工具的读者，可以输入help命令查看MariaDB能做的操作，语句的用法和MySQL一摸一样：

```
MariaDB [(none)]> help

General information about MariaDB can be found at http://mariadb.org

List of all MySQL commands:
Note that all text commands must be first on line and end with ';'
?         (\?) Synonym for `help'.
clear     (\c) Clear the current input statement.
connect   (\r) Reconnect to the server. Optional arguments are db and host.
delimiter (\d) Set statement delimiter.
edit      (\e) Edit command with $EDITOR.
ego       (\G) Send command to mysql server, display result vertically.
exit      (\q) Exit mysql. Same as quit.
go        (\g) Send command to mysql server.
help      (\h) Display this help.
nopager   (\n) Disable pager, print to stdout.
notee     (\t) Don't write into outfile.
pager     (\P) Set PAGER [to_pager]. Print the query results via PAGER.
print     (\p) Print current command.
prompt    (\R) Change your mysql prompt.
quit      (\q) Quit mysql.
rehash    (\#) Rebuild completion hash.
source    (\.) Execute an SQL script file. Takes a file name as an argument.
status    (\s) Get status information from the server.
system    (\!) Execute a system shell command.
tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
use       (\u) Use another database. Takes database name as argument.
charset   (\C) Switch to another charset. Might be needed for processing binlog with multi-byte charsets.
warnings  (\W) Show warnings after every statement.
nowarning (\w) Don't show warnings after every statement.

For server side help, type 'help contents'
```

在登录MariaDB数据库后执行数据库命令时，都需要在命令后面用分号（;）结尾，这也是与Linux命令最显著的区别。大家需要慢慢习惯数据库命令的这种设定。下面执行如下命令查看数据库管理系统中当前都有哪些数据库：

```
MariaDB [(none)]> SHOW databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.000 sec)
```

小试牛刀过后，接下来使用数据库命令将root管理员在数据库管理系统中的密码值修改为linuxprobe。这样退出后再尝试登录，如果还坚持输入原先的密码，则将提示访问失败。

```
MariaDB [(none)]> SET password = PASSWORD('linuxprobe');
Query OK, 0 rows affected (0.001 sec)

MariaDB [(none)]> exit
Bye
[root@linuxprobe ~]# mysql -u root -p
Enter password: 此处输入管理员在数据库中的旧密码
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)
```

而输入新密码（linuxprobe）后，便可顺利进入到数据库管理工具中：

```
[root@linuxprobe ~]# mysql -u root -p
Enter password: 此处输入管理员在数据库中的新密码
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 20
Server version: 10.3.11-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

**出现问题?大胆提问!**

> 因读者们硬件不同或操作错误都可能导致实验配置出错，请耐心再仔细看看操作步骤吧，不要气馁~
>
> Linux技术交流学习请加读者群（**推荐**）：https://www.linuxprobe.com/club
>
> *本群特色：确保每一位群友都是《Linux就该这么学》的读者，答疑更有针对性，不定期领取定制礼品。

##### **18.3 管理用户以及授权**

在生产环境中总不能一直“死啃”root管理员。为了保障数据库系统的安全性，以及让其他用户协同管理数据库，我们可以在MariaDB数据库管理系统中为他们创建多个专用的数据库管理账户，然后再分配合理的权限，以满足他们的工作需求。为此，可使用root管理员登录数据库管理系统，然后按照“CREATE USER 用户名@主机名 IDENTIFIED BY '密码'; ”的格式创建数据库管理账户。再次提醒大家，一定不要忘记每条数据库命令后面的分号（;）。

```
MariaDB [(none)]> CREATE USER luke@localhost IDENTIFIED BY 'linuxprobe';
Query OK, 0 rows affected (0.00 sec)
```

创建的账户信息可以使用select命令语句来查询。下面命令查询的是账户luke的主机名称、账户名称以及经过加密的密码值信息：

```
MariaDB [(none)]> use mysql;
Database changed
MariaDB [mysql]> SELECT HOST,USER,PASSWORD FROM user WHERE USER="luke";
+-----------+------+-------------------------------------------+
| HOST      | USER | PASSWORD                                  |
+-----------+------+-------------------------------------------+
| localhost | luke | *55D9962586BE75F4B7D421E6655973DB07D6869F |
+-----------+------+-------------------------------------------+
1 row in set (0.001 sec)
```

不过，用户luke仅仅是一个普通账户，没有数据库的任何操作权限。不信的话，可以切换到luke账户来查询数据库管理系统中当前都有哪些数据库。可以发现，该账户甚至没法查看完整的数据库列表（刚才使用root账户时可以查看到3个数据库列表）：

```
MariaDB [mysql]> exit
Bye
[root@linuxprobe ~]# mysql -u luke -p
Enter password: 输入luke用户的数据库密码
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 21
Server version: 10.3.11-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> SHOW databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
+--------------------+
1 row in set (0.001 sec)
```

数据库管理系统所使用的命令一般都比较复杂。我们以grant命令为例进行说明。grant命令用于为账户进行授权，其常见格式如表18-1所示。在使用grant命令时需要写上要赋予的权限、数据库及表单名称，以及对应的账户及主机信息。其实，只要理解了命令中每个字段的功能含义，也就不觉得命令复杂难懂了。

表18-1                    GRANT命令的常见格式以及解释

| 命令                                           | 作用                                             |
| ---------------------------------------------- | ------------------------------------------------ |
| GRANT 权限 ON 数据库.表单名称 TO 用户名@主机名 | 对某个特定数据库中的特定表单给予授权             |
| GRANT 权限 ON 数据库.* TO 用户名@主机名        | 对某个特定数据库中的所有表单给予授权             |
| GRANT 权限 ON *.* TO 用户名@主机名             | 对所有数据库及所有表单给予授权                   |
| GRANT 权限1,权限2 ON 数据库.* TO 用户名@主机名 | 对某个数据库中的所有表单给予多个授权             |
| GRANT ALL PRIVILEGES ON *.* TO 用户名@主机名   | 对所有数据库及所有表单给予全部授权（需谨慎操作） |



当然，账户的授权工作肯定是需要数据库管理员来执行的。下面以root管理员的身份登录到数据库管理系统中，针对mysql数据库中的user表单向账户luke授予查询、更新、删除以及插入等权限。

刘遄老师特别懂同学们现在心里想的是什么~哈哈，我起初也觉得在每条数据库命令后都要加上;（分号）来结束特别的不方便，时常还会忘记，但敲的命令多了也就自然习惯了。授权操作执行后来查看下luke用户的权限吧：

```
[root@linuxprobe ~]# mysql -u root -p
Enter password: 输入管理员的数据库密码
MariaDB [(none)]> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A
Database changed
MariaDB [mysql]> GRANT SELECT,UPDATE,DELETE,INSERT ON mysql.user TO luke@localhost;
Query OK, 0 rows affected (0.001 sec)
```

在执行完上述授权操作之后，我们再查看一下账户luke的权限：

```
MariaDB [(none)]>  SHOW GRANTS FOR luke@localhost;
+---------------------------------------------------------------------------------------------+
| Grants for luke@localhost                                                                   |
+---------------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO 'luke'@'localhost' IDENTIFIED BY PASSWORD '*55D9962586BE75F4B7D421E6655973DB07D6869F' |
| GRANT SELECT, INSERT, UPDATE, DELETE ON `mysql`.`user` TO 'luke'@'localhost'                |
+---------------------------------------------------------------------------------------------+
2 rows in set (0.000 sec)
```

上面输出信息中显示账户luke已经拥有了针对mysql数据库中user表单的一系列权限了。这时我们再切换到账户luke，此时就能够看到mysql数据库了，而且还能看到表单user（其余表单会因无权限而被继续隐藏）：

```
[root@linuxprobe ~]# mysql -u luke -p
Enter password: 输入luke用户的数据库密码

MariaDB [(none)]> SHOW databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
+--------------------+
2 rows in set (0.000 sec)

MariaDB [(none)]> use mysql;
Database changed

MariaDB [mysql]> SHOW tables;
+-----------------+
| Tables_in_mysql |
+-----------------+
| user            |
+-----------------+
1 row in set (0.001 sec)

MariaDB [mysql]> exit
Byes
```

大家不要心急，我们接下来会慢慢学习数据库内容的修改方法。当前，先切换回管理员，移除刚才的授权。

```
[root@linuxprobe ~]# mysql -u root -p
Enter password: 输入管理员的数据库密码
MariaDB [(none)]> use mysql;
Database changed
MariaDB [(none)]> REVOKE SELECT,UPDATE,DELETE,INSERT ON mysql.user FROM luke@localhost;
Query OK, 0 rows affected (0.00 sec)
```

可以看到，除了移除授权的命令（revoke）与授权命令（grant）不同之外，其余部分都是一致的。这不仅好记而且也容易理解。执行移除授权命令后，再来查看账户luke的信息：

```
MariaDB [(none)]> SHOW GRANTS FOR luke@localhost;
+---------------------------------------------------------------------------------------------+
| Grants for luke@localhost                                                                   |
+---------------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO 'luke'@'localhost' IDENTIFIED BY PASSWORD '*55D9962586BE75F4B7D421E6655973DB07D6869F' |
+---------------------------------------------------------------------------------------------+
1 row in set (0.001 sec)
```

不再需要某个用户时，可以直接用DROP命令进行删除：

```
MariaDB [(none)]> DROP user luke@localhost;
Query OK, 0 rows affected (0.000 sec)
```

##### **18.4 创建数据库与表单**

在MariaDB数据库管理系统中，一个数据库可以存放多个数据表，数据表单是数据库中最重要最核心的内容。我们可以根据自己的需求自定义数据库表结构，然后在其中合理地存放数据，以便后期轻松地维护和修改。表18-2罗列了后文中将使用到的数据库命令以及对应的作用。

表18-2                    用于创建数据库的命令以及作用

| 命令用法                                                     | 作用                   |
| ------------------------------------------------------------ | ---------------------- |
| CREATE database 数据库名称。                                 | 创建新的数据库         |
| DESCRIBE 表单名称;                                           | 描述表单               |
| UPDATE 表单名称 SET attribute=新值 WHERE attribute > 原始值; | 更新表单中的数据       |
| USE 数据库名称;                                              | 指定使用的数据库       |
| SHOW databases;                                              | 显示当前已有的数据库   |
| SHOW tables;                                                 | 显示当前数据库中的表单 |
| SELECT * FROM 表单名称;                                      | 从表单中选中某个记录值 |
| DELETE FROM 表单名 WHERE attribute=值;                       | 从表单中删除某个记录值 |



建立数据库是管理数据的起点。现在尝试创建一个名为linuxprobe的数据库，然后再查看数据库列表，此时就能看到它了：

```
MariaDB [(none)]>  CREATE DATABASE linuxprobe;
Query OK, 1 row affected (0.001 sec)

MariaDB [(none)]>  SHOW databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| linuxprobe         |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.001 sec)
```

MariaDB与MySQL同属关系型数据库（Relational Database Management System），“关系型”数据库有些类似于表格的概念，一个关系型数据库由一个或多个表格/表单组成，如图18-2所示。

![第18章 使用MariaDB数据库管理系统第18章 使用MariaDB数据库管理系统](https://www.linuxprobe.com/wp-content/uploads/2021/04/%E6%95%B0%E6%8D%AE%E5%BA%93%E6%A6%82%E5%BF%B5.png)

图18-2 数据库存储概念

上图中，表头代表每一列的名称；列代表具有相同数据类型的数据集合；行代表用来描述事物的具体信息；值代行的具体信息，每个值均与该列的其他数据类型相同；键代表用来识别某个特定事物的方法，在当前列中具有唯一性。

比如在新建的linuxprobe数据库中创建表单mybook，然后进行表单的初始化，即定义存储数据内容的结构。我们分别定义3个字段项，其中，长度为15个字符的字符型字段name用来存放图书名称，整型字段price和pages分别存储图书的价格和页数。当执行完下述命令之后，就可以看到表单的结构信息了：

```
MariaDB [(none)]> use linuxprobe;
Database changed
MariaDB [linuxprobe]> CREATE TABLE mybook (name char(15),price int,pages int);
Query OK, 0 rows affected (0.009 sec)

MariaDB [linuxprobe]> DESCRIBE mybook;
+-------+----------+------+-----+---------+-------+
| Field | Type     | Null | Key | Default | Extra |
+-------+----------+------+-----+---------+-------+
| name  | char(15) | YES  |     | NULL    |       |
| price | int(11)  | YES  |     | NULL    |       |
| pages | int(11)  | YES  |     | NULL    |       |
+-------+----------+------+-----+---------+-------+
3 rows in set (0.002 sec)
```

##### **18.5 管理表单及数据**

接下来向mybook数据表单中插一条图书信息。为此需要使用INSERT命令，并在命令中写清表单名称以及对应的字段项。执行该命令之后即可完成图书写入信息。下面我们使用该命令插入一条图书信息，其中书名为linuxprobe，价格和页数分别是60元和518页。在命令执行后也就意味着图书信息已经成功写入到数据表单中，然后就可以查询表单中的内容了。我们在使用select命令查询表单内容时，需要加上想要查询的字段；如果想查看表单中的所有内容，则可以使用星号（*）通配符来显示：

```
MariaDB [linuxprobe]> INSERT INTO mybook(name,price,pages) VALUES('linuxprobe','60', '518');
Query OK, 1 row affected (0.001 sec)

MariaDB [linuxprobe]> SELECT * from mybook;
+------------+-------+-------+
| name       | price | pages |
+------------+-------+-------+
| linuxprobe |    60 |   518 |
+------------+-------+-------+
1 row in set (0.000 sec)
```

对数据库运维人员来讲，需要做好四门功课—增、删、改、查。这意味着创建数据表单并在其中插入内容仅仅是第一步，还需要掌握数据表单内容的修改方法。例如，我们可以使用update命令将刚才插入的linuxprobe图书信息的价格修改为55元，然后在使用select命令查看该图书的名称和定价信息。注意，因为这里只查看图书的名称和定价，而不涉及页码，所以无须再用星号通配符来显示所有内容。

```
MariaDB [linuxprobe]> UPDATE mybook SET price=55 ;
Query OK, 1 row affected (0.002 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [linuxprobe]> SELECT name,price FROM mybook;
+------------+-------+
| name       | price |
+------------+-------+
| linuxprobe |    55 |
+------------+-------+
1 row in set (0.000 sec)
```

想指定某一条进行修改？没问题的，用where命令进行限定即可，例如再插入两条书籍信息：

```
MariaDB [linuxprobe]> INSERT INTO mybook(name,price,pages) VALUES('linuxcool','85', '300');
Query OK, 1 row affected (0.001 sec)
MariaDB [linuxprobe]> INSERT INTO mybook(name,price,pages) VALUES('linuxdown','105', '500');
Query OK, 1 row affected (0.001 sec)
```

然后仅修改名称为linuxcool的书籍价格为60元，不影响其他书籍信息：

```
MariaDB [linuxprobe]> UPDATE mybook SET price=60 where name='linuxcool';
Query OK, 1 row affected (0.001 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [linuxprobe]> select * from mybook;
+------------+-------+-------+
| name       | price | pages |
+------------+-------+-------+
| linuxprobe |    55 |   518 |
| linuxcool  |    60 |   300 |
| linuxdown  |   105 |   500 |
+------------+-------+-------+
3 rows in set (0.001 sec)
```

我们还可以使用delete命令删除某个数据表单中的内容。下面我们使用delete命令删除数据表单mybook中的所有内容，然后再查看该表单中的内容，可以发现该表单内容为空了。

```
MariaDB [linuxprobe]> DELETE FROM mybook;
Query OK, 3 row affected (0.001 sec)

MariaDB [linuxprobe]> SELECT * FROM mybook;
Empty set (0.000 sec)
```

一般来讲，数据表单中会存放成千上万条数据信息。比如我们刚刚创建的用于保存图书信息的mybook表单，随着时间的推移，里面的图书信息也会越来越多。在这样的情况下，如果我们只想查看其价格大于某个数值的图书时，又该如何定义查询语句呢？

下面先使用insert插入命令依次插入4条图书信息：

```
MariaDB [linuxprobe]> INSERT INTO mybook(name,price,pages) VALUES('linuxprobe1','30','518');
Query OK, 1 row affected (0.05 sec)
MariaDB [linuxprobe]> INSERT INTO mybook(name,price,pages) VALUES('linuxprobe2','50','518');
Query OK, 1 row affected (0.05 sec)
MariaDB [linuxprobe]> INSERT INTO mybook(name,price,pages) VALUES('linuxprobe3','80','518');
Query OK, 1 row affected (0.01 sec)
MariaDB [linuxprobe]> INSERT INTO mybook(name,price,pages) VALUES('linuxprobe4','100','518');
Query OK, 1 row affected (0.00 sec)
```

要想让查询结果更加精准，就需要结合使用select与where命令了。其中，where命令是在数据库中进行匹配查询的条件命令。通过设置查询条件，就可以仅查找出符合该条件的数据。表18-3列出了where命令中常用的查询参数以及作用。

表18-3                    where命令中使用的参数以及作用

| 参数    | 作用             |
| ------- | ---------------- |
| =       | 相等             |
| <>或!=  | 不相等           |
| >       | 大于             |
| <       | 小于             |
| >=      | 大于或等于       |
| <=      | 小于或等于       |
| BETWEEN | 在某个范围内     |
| LIKE    | 搜索一个例子     |
| IN      | 在列中搜索多个值 |



现在进入动手环节。分别在mybook表单中查找出价格大于75元或价格不等于80元的图书，其对应的命令如下所示。在熟悉了这两个查询条件之后，大家可以自行尝试精确查找图书名为linuxprobe2的图书信息。

```
MariaDB [linuxprobe]> SELECT * FROM mybook WHERE price>75;
+-------------+-------+-------+
| name        | price | pages |
+-------------+-------+-------+
| linuxprobe3 |    80 |   518 |
| linuxprobe4 |   100 |   518 |
+-------------+-------+-------+
2 rows in set (0.001 sec)

MariaDB [linuxprobe]> SELECT * FROM mybook WHERE price!=80;
+-------------+-------+-------+
| name        | price | pages |
+-------------+-------+-------+
| linuxprobe1 |    30 |   518 |
| linuxprobe2 |    50 |   518 |
| linuxprobe4 |   100 |   518 |
+-------------+-------+-------+
3 rows in set (0.000 sec)
```

匹配的条件越多，获得信息就越精准。在WHERE命令的后面追加AND操作符，可以进行多次匹配。例如找到这本价格为30元，页数为518的书籍名称：

```
MariaDB [linuxprobe]> SELECT * from mybook WHERE price=30 AND pages=518 ;
+-------------+-------+-------+
| name        | price | pages |
+-------------+-------+-------+
| linuxprobe1 |    30 |   518 |
+-------------+-------+-------+
1 row in set (0.000 sec)
```

##### **18.6 数据库的备份及恢复**

前文提到，本书的技术主线是Linux系统的运维方向，不会对数据库管理系统的操作进行深入的讲解，因此大家掌握了上面这些基本的数据库操作命令之后就足够了。下面要讲解的是数据库的备份以及恢复，这些知识比较实用，希望大家能够掌握。

mysqldump命令用于备份数据库数据，格式为“mysqldump [参数] [数据库名称]”。其中参数与mysql命令大致相同，-u参数用于定义登录数据库的账户名称，-p参数代表密码提示符。下面将linuxprobe数据库中的内容导出成一个文件，并保存到root管理员的家目录中：

```
[root@linuxprobe ~]# mysqldump -u root -p linuxprobe > /root/linuxprobeDB.dump
Enter password: 输入管理员的数据库密码
```

然后进入MariaDB数据库管理系统，彻底删除linuxprobe数据库，这样mybook数据表单也将被彻底删除。然后重新建立linuxprobe数据库：

```
[root@linuxprobe ~]# mysql -u root -p
Enter password: 输入管理员的数据库密码

MariaDB [(none)]> DROP DATABASE linuxprobe;
Query OK, 1 row affected (0.04 sec)

MariaDB [(none)]> SHOW databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.02 sec)

MariaDB [(none)]> CREATE DATABASE linuxprobe;
Query OK, 1 row affected (0.00 sec)
```

接下来是见证数据恢复效果的时刻！使用输入重定向符把刚刚备份的数据库文件导入到mysql命令中，然后执行该命令。接下来登录到MariaDB数据库，就又能看到linuxprobe数据库以及mybook数据表单了。数据库恢复成功！

```
[root@linuxprobe ~]# mysql -u root -p linuxprobe < /root/linuxprobeDB.dump 
Enter password: 输入管理员的数据库密码
[root@linuxprobe ~]# mysql -u root -p
Enter password: 输入管理员的数据库密码
MariaDB [(none)]> use linuxprobe;
Database changed

MariaDB [linuxprobe]> SHOW tables;
+----------------------+
| Tables_in_linuxprobe |
+----------------------+
| mybook               |
+----------------------+
1 row in set (0.000 sec)

MariaDB [linuxprobe]> describe mybook;
+-------+----------+------+-----+---------+-------+
| Field | Type     | Null | Key | Default | Extra |
+-------+----------+------+-----+---------+-------+
| name  | char(15) | YES  |     | NULL    |       |
| price | int(11)  | YES  |     | NULL    |       |
| pages | int(11)  | YES  |     | NULL    |       |
+-------+----------+------+-----+---------+-------+
3 rows in set (0.002 sec)
```

**出现问题?大胆提问!**

> 因读者们硬件不同或操作错误都可能导致实验配置出错，请耐心再仔细看看操作步骤吧，不要气馁~
>
> Linux技术交流学习请加读者群（**推荐**）：https://www.linuxprobe.com/club
>
> *本群特色：确保每一位群友都是《Linux就该这么学》的读者，答疑更有针对性，不定期领取定制礼品。

**本章节的复习作业(答案就在问题的下一行哦，用鼠标选中即可看到的~)**

1．RHEL 7系统为何选择使用MariaDB替代MySQL数据库管理系统？

**答：**因为MariaDB由开源社区进行维护，且不受商业专利限制。

2．初始化MariaDB或MySQL数据库管理系统的命令是什么？

**答：**是mysql_secure_installation命令，建议每次安装MariaDB或MySQL数据库管理系统后都执行这条命令。

3．用来查看已有数据库或数据表单的命令是什么？

**答：**要查看当前已有的数据库列表，需执行SHOW databases;命令；要查看已有的数据表单列表，则需执行SHOW tables;命令。

4．切换至某个指定数据库的命令是什么？

**答：**执行“use 数据库名称”命令即可切换成功。

5．若想针对某个账户进行授权或取消授权操作，应该执行什么命令？

**答：**针对账户进行授权，需执行GRANT命令；取消授权则需执行REVOKE命令。

6．若只想查看mybook表单中的name字段，应该执行什么命令？

**答：**应执行SELECT name FROM mybook命令。

7．若只想查看mybook表单中价格大于75元的图书信息，应该执行什么命令？

**答：**应执行SELECT * FROM mybook WHERE price>75命令。

8． 要想把linuxprobe数据库中的内容导出为一个文件（保存到root管理员的家目录中），应该执行什么命令？

**答：**应执行mysqldump -u root -p linuxprobe > /root/linuxprobeDB.dump命令。