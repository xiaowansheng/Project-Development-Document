#  Yum在线安装

在CentOS上安装MariaDB的步骤如下：

1. 打开终端或SSH连接到您的CentOS服务器。

2. 更新系统软件包列表，确保您拥有最新的软件包信息。执行以下命令：
   ```
   sudo yum update
   ```

3. 安装MariaDB。执行以下命令：
   ```
   sudo yum install mariadb-server
   ```

4. 安装过程完成后，启动MariaDB服务。执行以下命令：
   ```
   sudo systemctl start mariadb
   ```

5. 运行以下命令来确保MariaDB服务在系统启动时自动启动：
   ```
   sudo systemctl enable mariadb
   ```

6. 接下来，运行MariaDB的安全性脚本以加固默认配置并设置root用户的密码。执行以下命令：
   ```
   sudo mysql_secure_installation
   ```

   按照提示操作，您将被要求设置root密码、删除匿名用户、禁止root远程登录等。按照您的需求进行设置。

7. 完成安装和配置后，您可以使用以下命令检查MariaDB服务的运行状态：
   ```
   sudo systemctl status mariadb
   ```

   如果状态显示为"active"，则表示MariaDB已成功安装并正在运行。

现在您已成功在CentOS上安装了MariaDB。您可以使用MySQL客户端或其他工具连接到MariaDB并开始使用它。

## 开机自启

略

# Yum 离线包安装

要使用安装包在CentOS上安装MariaDB，请按照以下步骤进行操作：

1. 前往MariaDB官方网站的下载页面：https://mariadb.org/download/

2. 在下载页面上，选择您要安装的MariaDB版本。根据您的CentOS版本选择正确的安装包，例如，选择适用于CentOS 7的MariaDB 10.11版本。

3. 在下载页面上找到适用于CentOS的RPM安装包，点击下载链接将安装包保存到您的CentOS服务器上。

4. 打开终端或SSH连接到您的CentOS服务器。

5. 在终端中，使用以下命令进行安装，将命令中的`<package_name>`替换为您下载的安装包的文件名：
   ```
   sudo yum install <package_name>
   ```

   例如，如果您下载的安装包文件名为`mariadb-10.11.4-centos7-x86_64.rpm`，则安装命令如下：
   ```
   sudo yum install mariadb-10.11.4-centos7-x86_64.rpm
   ```

6. 系统将提示您确认安装，请输入`y`并按下Enter键继续。

7. 安装完成后，启动MariaDB服务。执行以下命令：
   ```
   sudo systemctl start mariadb
   ```

8. 运行以下命令来确保MariaDB服务在系统启动时自动启动：
   ```
   sudo systemctl enable mariadb
   ```

9. 运行以下命令检查MariaDB服务的运行状态：
   ```
   sudo systemctl status mariadb
   ```

   如果状态显示为"active"，则表示MariaDB已成功安装并正在运行。

现在您已经使用安装包成功在CentOS上安装了MariaDB。您可以使用MySQL客户端或其他工具连接到MariaDB并开始使用它。

## 开机自启

略

# Tar.gz 包安装

要使用tar.gz包在CentOS上安装MariaDB，请按照以下步骤进行操作：

## 下载 

前往MariaDB官方网站的下载页面：https://mariadb.org/download/

1. 在下载页面上，选择您要安装的MariaDB版本。根据您的CentOS版本选择正确的tar.gz安装包，例如，选择适用于CentOS 7的MariaDB 10.11.4版本。
2. 在下载页面上找到适用于CentOS的tar.gz安装包，点击下载链接将安装包保存到您的CentOS服务器上。

## 解压

1. 在终端中，使用以下命令解压缩tar.gz文件，将命令中的`<package_name>`替换为您下载的安装包的文件名：
   ```
   tar xvf <package_name>
   ```

   例如，如果您下载的安装包文件名为`mariadb-10.11.4-linux-systemd-x86_64.tar.gz`，且要解压到`/opt/mariadb`目录下，则解压缩命令如下：
   ```
   tar xvf mariadb-10.11.4-linux-systemd-x86_64.tar.gz -C /opt/
   ```

2. 解压缩后，进入解压缩后的目录。例如，使用以下命令进入解压缩后的目录：

   ```
   cd /opt
   ```

3. 重命名文件夹

   ```bash
   mv <package_name> mariadb
   ```

   例如：

   ```bash
   mv mariadb-10.11.4-linux-systemd-x86_64 mariadb
   ```

   

## 创建系统用户-mysql

1. 创建mysql组：

   ```
   groupadd mysql
   ```

2. 创建mysql用户，并将其添加到mysql组中：

   ```
   useradd -g mysql mysql
   ```
   
   `useradd` 命令用于在 Linux 系统上创建新用户。
   
   解释一下命令的参数：
   
   - `sudo`：以管理员权限运行命令。
   - `useradd`：用于创建新用户的命令。
   - `-g mysql`：指定新用户的初始组为 `mysql` 组。
   - `mysql`：新用户的用户名。

## 赋权给mysql用户

进入mysql目录并将所有文件的所有权和组设置为mysql

```bash
cd /opt/mariadb
```

```bash
chown -R mysql .
```

```bash
chgrp -R mysql .
```

## 初始化数据库

执行mysql_install_db脚本以初始化数据库(数据路径`datadir`默认：`/var/lib/mysql`)

 指定数据路径

```bash
scripts/mysql_install_db --user=mysql --datadir=/opt/mariadb/data
```

或（默认）

```bash
scripts/mysql_install_db --user=mysql
```

## 创建符号链接（也可以配置环境变量）

在 `/usr/local/bin/` 目录下创建一个符号链接，指向 `/opt/mariadb/bin/mariadb` 可执行文件。

这将使得可以直接使用`mysql`命令去执行`mariadb`命令，而不需要指定执行函数的完整路径。

```bash
ln -fs /opt/mariadb/bin/mariadb /usr/local/bin/mysql
```

`-f` 标志表示如果已存在同名文件或目录，则会被覆盖。

> 当前还不能执行mysql命令，因为mariadb数据库还没有启动。



## 所有权设置

将文件的所有权设置为root

```bash
chown -R root .
```

将自定义的数据目录的所有权设置为mysql用户，如：上述指定目录`/opt/mariadb/data`。(默认：`/var/lib/mysql`)

```bash
chown -R mysql /opt/mariadb/data
```

## 配置my.cnf

备份默认配置文件

```bash
cp /etc/my.cnf /etc/my.cnf.bak
```

移动mariadb配置文件模板

```bash
#配置文件模板在mariadb包解压之后的support-files路径下
cp support-files/wsrep.cnf  /etc/my.cnf
```

编辑配置文件`my.cnf`

```bash
vi /etc/my.cnf
```

```bash
# 修改配置文件中的安装路径和数据路径(这两个路径就是前面提到的了)
# 默认的：basedir为解压后的文件夹路径，如：/opt/mariadb
# 在[mysqld]下写
basedir=/<mariadb-path>
datadir=/<mariadb-data-path>
```





## （跳过）简单启动

启动MariaDB服务器

```bash
bin/mysqld_safe --user=mysql &
```

如果遇到不能创建`/var/log/mariadb`文件或文件夹错误，或者授权拒绝，则需要对该文件夹进行授权。

> 执行命令`bin/mysqld_safe --user=mysql &`是为了启动MariaDB服务器并作为后台进程运行。该命令将以mysql用户的身份启动MariaDB，并使其在后台运行。
>
> 注意，使用`mysqld_safe`启动服务器是一种简单的方法，但不适用于生产环境，因为它不提供高可用性和故障恢复机制。在生产环境中，应该使用专业的服务管理工具（如systemd、supervisor等）来管理MariaDB服务器的启动和运行。
>
> 确保MariaDB已成功启动后，您可以使用MySQL客户端连接到服务器进行数据库操作。例如，使用以下命令连接到MariaDB服务器：
> ```
> mysql -u root -p
> ```
> 根据您的配置，输入root用户的密码，即可进入MySQL命令行界面。
>
> 请注意，根据您的特定需求和环境，可能需要进一步进行配置和安全性设置，例如设置root密码、配置防火墙规则等。确保参考MariaDB的官方文档和最佳实践来确保您的数据库安全和性能。



## （暂时无效，跳过）配置短链接

**配置环境变量（后面直接使用 mysql 等命令）**（**！！！该设置无效**）

创建一个名为 `mysql.sh` 的文件，并将 `export PATH=/usr/local/mysql/bin:$PATH` 写入该文件

```bash
echo 'export PATH=/opt/mariadb/bin:$PATH' >  /etc/profile.d/mysql.sh
```

运行以下命令以确保环境变量立即生效

```bash
source /etc/profile.d/mysql.sh
```

这将使当前终端会话立即加载 `/etc/profile.d/mysql.sh` 中设置的环境变量。

现在，可以在终端中直接使用 `mysql` 命令，而不需要指定完整路径。



## 配置启动脚本

```bash
vi support-files/mysql.server
```

```bash
# 配置参数
# 默认的：basedir为解压后的文件夹路径，如：/opt/mariadb
basedir=/opt/mariadb
datadir=/opt/mariadb/data
```



**启动脚本移至启动路径**

```bash
cp support-files/mysql.server /etc/rc.d/init.d/mysql.server
```

**设置mariadb开机自启动**

```bash
# 将 服务加入 开机自启动列表中
chkconfig --add mysql.server
 # 设置开机自起
chkconfig mysql.server on
```

**使用启动脚本启动mariadb**

```bash
/etc/init.d/mysql.server start 
# 或者
systemctl start mysql.server
```

**检查是否设置成功**

```bash
chkconfig --list mysql.server
```





# 其它配置

## 设置root用户密码

无密码登录mysql

```bash
mysql -uroot -p
```

root用户授权改密码(三个都需要执行)

```bash
grant all privileges on *.* to 'root'@'%' identified by 'root';
grant all privileges on *.* to 'root'@'localhost' identified by 'root';
grant all privileges on *.* to 'root'@'127.0.0.1' identified by 'root';
flush privileges;
```

退出，使用新密码登录

```bash
mysql -uroot -proot
```

## 开放端口号

要在 CentOS 上开放端口 3306（MySQL 默认端口），可以按照以下步骤使用 firewalld 防火墙管理工具进行操作：

1. 检查 firewalld 服务状态：
   ```
   sudo systemctl status firewalld
   ```

   如果 firewalld 未运行，可以启动它：
   ```
   sudo systemctl start firewalld
   ```

2. 检查防火墙的活动区域：
   ```
   sudo firewall-cmd --get-active-zones
   ```

   根据输出结果，确定要在哪个区域开放端口。通常情况下，`public` 区域是最常用的区域。

3. 在相应的区域中添加需要开放的端口。使用以下命令开放 TCP 端口 3306：
   ```
   sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent
   ```

   这将将端口 3306 添加到 public 区域的防火墙规则中，并使用 `--permanent` 参数使配置持久化。

4. 重新加载防火墙配置：
   ```
   sudo firewall-cmd --reload
   ```

5. 验证端口是否已成功开放。可以使用以下命令来列出 public 区域中已开放的端口：
   ```
   sudo firewall-cmd --zone=public --list-ports
   ```

   在输出中可以看到 3306/tcp。



