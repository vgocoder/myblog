+++
author = "vgocoder"
title = "macOS Big Sur下使用tar.gz压缩包安装MySQL"
date = "2021-04-16"
description = "MySQL官方的安装包太大，就下载了个压缩包试试，相比安装包有点折腾，以下是折腾过程。"
tags = [
    "macOSBigSur",
    "MacM1",
    "tar.gz",
    "mysql",
]
categories = [
    "技术",
    "育儿",
]
series = ["Mac M1"]
aliases = ["migrate-from-jekyl"]
+++

MySQL官方的安装包太大，就下载了个压缩包试试，相比安装包有点折腾，以下是折腾过程。

```shell
# 1.下载安装压缩包
# 2. 解压
sudo tar zxf mysql-8.0.23-macos10.15-x86_64.tar.gz -C /usr/local
# 2.1 设置软链接
cd /usr/local 
sudo ln -s mysql-8.0.23-macos10.15-x86_64 ./mysql
# 3. 更改 mysql 安装目录所属用户与用户组
sudo chown -R root:wheel mysql
# 4 初始化
cd /usr/local/mysql
sudo bin/mysqld --initialize --user=mysql
# 初始化后得到一个【临时初始化密码】，后面要用到
# 5. 启动服务
# 5.1 启动
cd /usr/local/mysql
sudo ./support-files/mysql.server start  
# 5.2 重启(可选)
sudo ./support-files/mysql.server restart  
# 5.3 停止(可选)
sudo ./support-files/mysql.server stop  
# 5.4 检查 MySQL 运行状态(可选)
sudo ./support-files/mysql.server status
# 6. 重设密码
cd /usr/local/mysql
sudo ./bin/mysql -uroot -p
# 6.1 输入【临时初始化密码】

```

```mysql
# 6.2 修改密码
mysql>
	ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password' PASSWORD EXPIRE NEVER;
```

  ```shell
# 7. 设置mysql执行路径(可选)
echo 'export PATH="$PATH:/usr/local/mysql/bin"' >> ~/.zshrc
source ~/.zshrc
# 7.1 如果用其他shell配置文件替换为对应配置文件，譬如："~/.bash_profile"
echo 'export PATH="$PATH:/usr/local/mysql/bin"' >> ~/.bash_profile
source ~/.bash_profile
  ```

