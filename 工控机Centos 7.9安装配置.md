# 系统安装

1. 修改系统时区
2. 修改安装时的依赖**（最小化安装，并勾选：【development tools】，为了添加基本的环境，如 GCC 等）**
3. 系统安装到工控机里（**选择自己配置磁盘分配**）
4. 修改磁盘分配**（多的空间都分配给公共）**
5. 设置用户密码（默认：eternal）



# 检查网口配置

查看和管理网络接口，检查网卡配置是否对

```bash
ip addr
```

> 如缺少网口信息，则需要执行下面的安装网卡驱动



# 安装网卡驱动（如果网口配置缺失）

**检测**

```bash
ip addr
```



网卡驱动`e1000e`->系统内核`kernel`->perl语言环境->gcc编译器

## 挂载u盘

**查看系统设备识别表：**

```bash
lsblk
```

这将列出系统中的所有块设备，包括硬盘、分区和可移动设备。通常，U盘会被标识为类似于`/dev/sdb`或`/dev/sdc`的设备。

**创建一个用于挂载U盘的目录**

可以选择在任何位置创建该目录。例如在`用户主目录下`创建一个名为`usb`的文件夹`mkdir ~/usb`或者在`opt`目录下创建`mkdir /optusb`

```bash
mkdir /opt/usb
```

**将U盘挂载到刚创建的目录**

```bash
# <xxx>是识别表中识别出的U盘的标识名称
sudo mount /dev/<xxx> /opt/usb
```

**完成使用U盘后，可以使用以下命令卸载它**

```bash
# <xxx>是上述挂载的目录
sudo umount <xxx>
```

## （可跳）安装 Perl（perl语言环境）-安装内核需要

**解压文件**

```bash
# <xxx>是驱动文件的名称,<path>是路径地址
tar -xzvf <xxx>.tar.gz -C /<path>
```

**进入到解压的文件夹**

**运行Perl的配置脚本**

```bash
./Configure -des
```

这将启动Perl的配置过程，并询问您一些选项。按照需要进行选择或直接按Enter键接受默认值。

**编译Perl**

```bash
make
```

执行make命令来编译Perl，这将使用make工具编译Perl的源代码。

**(可选）运行测试套件**

运行make test来运行Perl的测试套件，以确保Perl正确编译和运行。

```bash
make test
```

**安装Perl**

```bash
sudo make install
```

这将使用管理员权限安装Perl到系统中。

> 安装完成后，您可以在终端或命令行界面中输入`perl -v`命令来验证Perl是否正确安装，并查看Perl的版本信息。



## （可跳）安装系统内核-安装网卡驱动需要

运行以下命令来安装.rpm文件

```bash
# <xxx>是文件名称
sudo rpm -i <xxx>.rpm
```

> 如果在安装.rpm文件时出现"failed dependencies"（依赖关系失败）的错误，这意味着安装所需的某些依赖项在您的系统中缺失或版本不兼容。解决这个问题的一种方法是手动安装缺失的依赖项。



## 安装网卡驱动程序

要安装 tar.gz 文件中的驱动程序，可以按照以下步骤进行操作：

**解压文件**

首先，使用以下命令解压 tar.gz 文件：

解压到指定目录

```
# <xxx>是驱动文件的名称,<path>是路径地址
tar -xzvf <xxx>.tar.gz -C /<path>
```

解压到当前目录

```
# <xxx>是驱动文件的名称
tar -xzvf <xxx>.tar.gz 
```

**进入到文件解压的目录**

```bash
cd /<path>
```

**查阅安装说明**

在解压后的目录中，通常会提供有关驱动程序安装的说明文件，如 README 或 INSTALL。您可以使用文本编辑器打开这些文件，并查阅其中的安装说明。

```
nano README
```

这将使用 `nano` 文本编辑器打开 README 文件，您可以使用其他喜欢的编辑器进行替换。

**执行安装命令**

根据驱动程序的要求，在终端中执行相应的安装命令。这可能是一个可执行文件、脚本或 Makefile。

```
sudo make install
```

或者

```
sudo ./install.sh
```

根据驱动程序的要求，您可能需要使用管理员权限（sudo）来执行安装命令。

**完成安装**

完成安装后，根据驱动程序的说明，您可能需要重启计算机或重新加载相关模块才能使驱动程序生效。



> 注意：这只是一个一般的步骤指南，实际安装过程可能会因驱动程序的不同而有所差异。确保在执行安装之前阅读提供的文档和说明，以了解特定驱动程序的安装要求和步骤。

## 再次检查配置

**重启服务器**

```bash
reboot
```

执行上面检查网卡配置的方法，如果网口对了则进行下列网卡配置。

# 网卡配置

在大多数Linux发行版中，网卡配置文件通常位于`/etc/sysconfig/network-scripts/`目录下。

**进入网卡配置文件位置：**

```bash
cd /etc/sysconfig/network-scripts
```

**编辑配置文件**

```bash
# <name>：文件名称
vi <name>
```

其它操作：

```bash
# 按 i 进入编辑
# 按 ESC 退出编辑状态
# 按 shift+: 可以输入命令
# 输入 wq 写入文件并退出文件编辑
```



**网卡配置：**

```vim
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.0.88
NETMASK=255.255.255.0
GATEWAY=192.168.0.1
```

**（如果文件缺失）**

在目录`/etc/sysconfig/network-scripts`下，从已有的网卡配置文件中复制一份文件，重命名为新检测的网卡名称。

```bash
# <NAME>：网卡名称
cp ifcfg-enp2s0 ifcfg-<NAME>
```

配置新的网卡配置文件信息。

<name>：网卡名称

<device>：网卡名称

```vim
BOOTPROTO=static
NAME=<name>
DEVICE=<device>
ONBOOT=yes
IPADDR=192.168.1.88
NETMASK=255.255.255.0
GATEWAY=192.168.1.254
```

**重启网络**

```bash
systemctl restart network
```

