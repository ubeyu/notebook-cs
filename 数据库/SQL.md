# -------------数据库使用笔记---------------
## MySQL环境安装踩坑
```
下载mysql-8.0.21-winx64.zip解压到指定文件夹，在cmd中cd到bin，运行mysql -u root -p：
报错一：
      由于找不到 MSVCP140.dll ，无法继续执行代码。
      解决：安装下载 Microsoft Visual C++ 2015 Redistributable Update 3 环境
      链接： https://www.microsoft.com/en-us/download/confirmation.aspx?id=53840
报错二：
      在安装  Microsoft Visual C++ 2015 Redistributable Update 3 时，报 "0x80070666 - 已安装这个产品的另外一个版本" 错误。
      原因：由于vc++运行库 2017 版和 2015 版的函数接口完全相同，2017就是2015版的修订版本，不需要安装2015版了。
      解决：若要安装 2015，把 2017 的卸载了，再安装 2015 版。
报错三：
      由于找不到VCRUNTIME140_1.dll，无法继续执行代码。重新安装程序可能会解决此问题。
      原因：缺少动态链接库(.ddl文件)，具体原因可能多种多样，系统本身问题、安装软件被删除、误删等，跟mysql本身的安装无关。
      解决：下载并安装VCRUNTIME140_1.dll。
      链接：https://www.jb51.net/dll/vcruntime140_1.dll.html
问题四：
      新装的数据库需要初始化root用户的密码。
```
## MySQL常用指令

**进入bin运行：**
```
mysql -u root -p
```
**删除指定数据库：**
```
drop database why_home_database;	
```
**添加指定数据库：**
```
create database why_home_database; 
```
**1.查看所有数据库：**
```
show databases;
```
**2.进入数据库：**
```
use why_home_database;
```
**3.查看当前库的表：**
```
show tables;
```
**4.查看表内容（若为空无法看）：**
```
SELECT * FROM home_blog;
```
**5.查看表结构：**
```
desc home_blog;
```
**6.home_user内插入一条记录：**</br>
```
INSERT INTO home_user (id,avatar, create_time , email, nickname , password, type ,update_time ,username) VALUES (1,' https://images.unsplash.com/photo-1597098495323-fc5d2ac74f75?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=800&q=60', '2020-08-22 10:55:55', '363057994@qq.com','winter','password',1, '2020-08-23 20:16:00', 'why');
```
**7.home_user内更新一条记录：**</br>
更新MD5密码：
```
UPDATE home_user SET password='MD5password' WHERE id=1;
```
更新头像：
```
UPDATE home_user SET avatar='https://c-ssl.duitang.com/uploads/item/201802/27/20180227232445_umtpo.jpeg' WHERE id=1;
```
**8.查询并观察结果:**
```
SELECT * FROM home_user;
```
</br></br>




## why_home_database说明：</br>
**1.home_blog_tags属于多对多关系的中间表。**</br>

**2.hibernate_sequence属于：**</br>
1)发现数据库保存的时候会生成hibernate_sequence表，用来记录其他表的主键。若删除该表，将报错
could not read a hi value - you need to populate the table: hibernate_sequence。</br>
2)服务器重启时主键从1开始记录，由于数据库里有主键为1的数据，于是会报主键重复的错误。</br>

**3.Type_id，User_id属于外键：**</br>
1. 如果Key是空的, 那么该列值的可以重复, 表示该列没有索引, 或者是一个非唯一的复合索引的非前导列</br>
2. 如果Key是PRI,  那么该列是主键的组成部分</br>
3. 如果Key是UNI,  那么该列是一个唯一值索引的第一列(前导列),并别不能含有空值(NULL)</br>
4. 如果Key是MUL,  那么该列的值可以重复, 该列是一个非唯一索引的前导列(第一列)或者是一个唯一性索引的组成部分但是可以含有空值NULL</br>

**4.Blog_id属于外键，Parent_comment_id属于自关联的外。**</br>

**在多对一关系中，多属性的那张表中应有1属性的id外键。**

</br></br>

## 查询数据库运行状态的基本命令：</br>
查询数据库连接</br>
```
show full  processlist;
show status like '%Max_used_connections%';
show status like '%Threads_connected%';#当前连接数
show status like '%table_lock%';#表锁定
show status like 'innodb_row_lock%';#行锁定
show status like '%qcache%'; #查询缓存情况
show variables like "%query_cache%";
SHOW STATUS LIKE 'Qcache%';
show variables like "%binlog%";
show status like 'Aborted_clients';#由于客户没有正确关闭连接已经死掉，已经放弃的连接数量
show variables like '%max_connections%';//查看最大连接数量
show variables like '%timeout%';#查看超时时间
show variables like 'log_%'; #查看日志是否启动
```



## 采用yum安装MySQL后，如果想要完全卸载MySQL，可以采用如下方式：

#### 1、查看mysql安装了哪些东西：
```
rpm -qa |grep -i mysql
```

#### 2、开始卸载
```
yum remove mysql-community-common-5.7.20-1.el7.x86_64
yum remove mysql-community-client-5.7.20-1.el7.x86_64
yum remove mysql57-community-release-el7-11.noarch
yum remove mysql-community-libs-5.7.20-1.el7.x86_64
yum removemysql-community-server-5.7.20-1.el7.x86_64
```

#### 3、查看是否卸载完成
```
rpm -qa |grep -i mysql
```

#### 4、查找mysql相关目录
```
find / -name mysql
```

#### 5、删除相关目录
```
rm -rf 查找出来的目录
```

#### 6、删除/etc/my.cnf
```
rm -rf /etc/my.cnf
```

#### 7、删除/var/log/mysqld.log（如果不删除这个文件，会导致新安装的mysql无法生存新密码，导致无法登陆）

```
rm -rf /var/log/mysqld.log
```

#### 8、删除默认密码

```
rm -rf /root/.mysql_sercret
```
