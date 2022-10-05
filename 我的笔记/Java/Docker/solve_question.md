## 强制修改docker中的mysql的登录密码

> mysql版本为5.7。

步骤：

1. 在容器中的mysql配置文件中添加上“忘记密码模式”。（**修改配置文件允许无密码登录**）

   ```mysql
   #进入docker容器中的mysql
   docker exec -it 容器ID /bin/bash
   
   #编辑mysql配置文件 注:若显示无vi命令，执行命令 apt-get update 和 apt-get install vim 下载vim
   vi /etc/mysql/conf.d/docker.cnf
   
   #在配置文件中添加"忘记密码模式":
   skip-grant-tables
   
   #退出容器，重启容器
   exit
   docker restart 容器ID
   ```

2. 免密进入mysql设置root用户新密码，刷新权限后退出。（**登录进mysql后修改密码**）

   ```mysql
   #进入docker容器中的mysql
   docker exec -it 容器ID /bin/bash
   
   #免密登录mysql
   mysql -uroot
   
   #使用mysql库中的user表，并修改root用户密码[where跟着的条件是用户名和权限(localhost权限表示只能本地运行，而改为%表示可以远程本地都连)]
   update mysql.user set authentication_string=password('新密码') where user='root' and Host = 'localhost';
   
   #刷新权限
   flush privileges;
   
   #退出mysql
   exit
   ```

3. 在mysql配置文件中去掉“忘记密码模式”，重启服务。（**将配置文件改回来**）

   ```mysql
   #进入docker容器中的mysql
   docker exec -it 容器ID /bin/bash
   
   #编辑mysql配置文件 注:若显示无vi命令，执行命令 apt-get update 和 apt-get install vim 下载vim
   vi /etc/mysql/conf.d/docker.cnf
   
   #在配置文件中去掉"忘记密码模式":
   #skip-grant-tables
   
   #退出容器，重启容器
   exit
   docker restart 容器ID
   ```

   

