#  下载地址

[https://www.oracle.com/java/technologies/downloads/](https://www.oracle.com/java/technologies/downloads/#sjre8-linux)

> 【注意】：下载请选择正确的版本和系统架构，否则将会在使用中报错。



# 解压

**创建存放java环境的目录**

```bash
mkdir /usr/java
```

**解压到java文件夹**

```bash
tar -zxvf <jdkxxx.tar.gz> -C /usr/java/
```



# 配置环境变量

**环境变量的文件位置**：`/etc/profile`

**编辑环境变量文件：**

```bash
vi /etc/profile
```

**文件末尾添加以下配置：**

```
export JAVA_HOME=/usr/java/jdk1.8.0_371
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib:$CLASSPATH
export JAVA_PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin
export PATH=$PATH:${JAVA_PATH}
```

> 建议：手动在vim中输入该配置
>
> 直接复制粘贴可能会执行错误，windows的换Llinux的有差别。
>
> 这时候需要用工具进行字符转换

# 测试运行

**查看版本信息**

```bash
java -version
```

输出版本信息说明配置成功。

如：

```bash
[root@localhost ~]# java -version
java version "1.8.0_371"
Java(TM) SE Runtime Environment (build 1.8.0_371-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.371-b11, mixed mode)
[root@localhost ~]# 
```

