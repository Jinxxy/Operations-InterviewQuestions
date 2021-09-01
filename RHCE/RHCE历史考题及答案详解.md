1. 远程登陆

    1. 远程访问

    ```
    # 访问远程计算机172.17.55.2，使用root用户，密码为：123456
    ssh root@172.17.55.2
    # 输入密码
    Last login：.............
    # 退出远程登录
    exit
    ```

    2. 生成密钥

    `ssh-keygen -t rsa`

    3. 复制公钥到172.17.55.2

    `ssh-copy-id -i .ssh/id_ras.pub root@172.17.55.2`

    4. 无需输入密码登录

2. 用yum命令安装软件

```shell
# 安装软件
yum install name
yum groupinstall name
# 卸载软件
yum remove name
```

3. 电脑启动的配置

```shell
# 启动引导菜单
vim /etc/grub.conf
# 启动第一个系统
default=0
# 启动第二个系统
default=1
```

4. 设置默认的运行级别

```shell
vim /etc/inittab
# 5为运行级别(0-6)
id:5:initdefault
```

5. NFS设置

网络文件系统(此命令可以拷贝传输文件类似于FTP)
```shell
# 查看目前已挂载的NFS系统
showmount -e
# 挂载的NFS
mount 172.17.55.2:/var/ftp/pub /mnt
```

6. ssh远程拷贝文件

```shell
# scp -r 源文件 本地
scp -r 172.17.55.2:/home/share/hi /root/a
```

7.

```shell

```


```shell

```


```shell

```

```shell

```

```shell

```

```shell

```

```shell

```

```shell

```

```shell

```

```shell

```

```shell

```

```shell

```

```shell

```

```shell

```

```shell

```

```shell

```

```shell

```

```shell

```
