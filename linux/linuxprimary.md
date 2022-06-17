# 1 背景
```text
linux版本
- 内核版本
- 发行版本

内核版本官网：
https://www.kernel.org/

常见的发行版本：
Red Hat
CentOS(基于redhat)
Ubuntu(图形化桌面)

linux系统中“万物皆文件”。
```


# 2 系统操作
```shell
# 非图形化登录系统
init 3
# 退出登录
exit

# 关机
init 0

# 切换用户
su - root

# 3种帮助命令
man
# man ls
# man man
# man 7 man 获取第7章帮助
# man 1 ls 获取第1章帮助
# man 1 passwd
# man 5 passwd
# man -a passwd

help
# shell(命令解释器)自带的命令成为内部命令，其它都是外部命令
# 内部命令使用help
# help cd
# 外部命令使用help
# ls --help

# 查看命令类型
# type ls
# type cd

info
# info比help更详细，作为help的补充
# info ls
```

常见目录
```text
/ 根目录
/root root用户家目录
/home/username 普通用户家目录
/etc 配置文件目录
/bin 命令目录
/sbin 管理命令目录
/usr/bin /usr/sbin 系统预装的其它命令
```

	目录操作
```shell
pwd
# 显示当前目录
# cd /etc/test
# cd ../test/d1

ls
查看当前目录文件
# ls [option] [文件/目录]
-l 长格式显示文件
-a 显示隐藏文件
-r 逆序显示
-t 按照时间顺序显示
-R 递归显示
-h

# ls /root
# 列出多个文件或者目录信息
# ls / /root /etc
# ls -l -r
# ls -l -r -t
# ls -lrt
# ls -R ./
# ls -lh 1.tar

只显示目录文件
# ls -ld ./test

cd
# 更改当前目录
# cd - 回到上一次的目录
# cd ..
# cd .

mkdir
-p 建立多级目录
# mkdir /a
# mkdir /a /b /c
# mkdir a b c
# mkdir /a/b/c
# mkdir -p /a/b/c

rmdir
# 删除空目录
# 目录为空时才能删，否则报错

rm
# 删除文件
-r 删除目录，包括目录下的文件
-f 删除文件不进行提示
# 删除非空目录
# rm -r /a
# rm -r -f /a
# rm -rf /a

cp
# 复制文件或者目录
# cp [option] 文件...目录
-r 复制目录
-p 保留用户、权限、时间等文件属性
-a 等同-dpR
# cp /1.log /etc/
# cp /a /b
# cp -v /a /b
# cp -a /a /b

mv
# 移动文件或者重命名
# mv [option] 源文件 目标文件
# mv [option] 源文件 目录

touch
# 创建文件
# touch a.log b.log c.log

通配符
shell内建的符号
常用的：
* 匹配任意字符串
? 匹配一个字符串
[xyz] 匹配xyz任意一个字符
[a-z] 匹配一个范围
[!xyz]或者[^xyz] 不匹配

文本查看
cat
# 文本内容显示到终端
head
# 查看文件开头
# head 1.log
# head -5 1.log
tail
# 查看文件结尾
-f 文件内容更新后显示，显示信息同步更新
# tail -f 1.log
# tail -3 1.log
wc
# 统计文件内容信息
# wc -l 1.log

more
# more 1.log
less
# less 1.log

打包和压缩
tar
# 打包命令
c 打包
x 解包
f 指定操作类型为文件
经常使用的扩展名：
.tar.gz
.tgz
.tar.bz2
# tar cf 1.tar /etc
# tar czf 1.tar.gz /etc
# tar cjf 1.tar.bz2 /etc
# tar xf 1.tar -C /root/

```

用户管理
```shell
useradd
userdel
# userdel wh
# 不保留家目录
# userdel -r wh

passwd
# passwd wh

chage
# 修改用户生命周期

id
# id
# id root
# id wh

# tail -10 /etc/passwd
# tail -10 /etc/shadow


usermod
# 修改用户属性
# usermod -d /home/test wh
# usermod -g group1 wh

change
# 修改用户属性

groupadd
# 新建用户组
# groupadd group1

groupdel
# 删除用户组
```

用户切换
```shell
su
# 切换用户
su - wh
# 使用login shell的方式切换用户

sudo
# 以其它用户身份执行命令

visudo
# 设置需要使用sudo的用户（组）
# 需要按照特定的格式进行编辑


shutdown -h 30
shutdown -c

```

文件
```shell
文件类型
- 普通文件
d 目录文件
b 块特殊文件
c 字符特殊文件
l 符号链接
f 管道文件
s 套接字文件

文件权限表示
字符权限表示法
r 读
w 写
x 执行
数字权限表示
r=4,w=2,x=1

目录文件权限
x 可以进入目录
rx 显示目录内的文件名
wx 修改目录内的文件名

修改权限
chomd
# 修改文件、目录权限
chmod u+x /tmp/test
chmod 755 /tmp/test
chmod g-r /tmp/test
chmod o=w /tmp/test
chmod a+r /tmp/test

chown
# 更改属主、属组
chown user1 /test
chown :group1 /test
chown user1:group2 /test

chgrp
# 可以单独更改属组，不常用
chgrp user1 /test
```

网络管理
```shell
网络状态查看
net-tools vs iproute
1.net-tools
ifconfig
route
netstat

2.iproute2
ip (centos7以后的版本)
ss

ifconfig
普通用户：/sbin/ifconfig

CentOS网络接口名称最长只能有16个字符。若是名字超过16个字符，则超出部分会被截掉。

网络接口命名修改
网卡命名规则受net.ifnames=0 biosdevname=0 这2个参数影响

将网络接口更改一下，比如将ensxx改成eth0：
修改/etc/default/grub，在GRUB_CMDLINE_LINUX的末尾添加两个参数：
net.ifnames=0 biosdevname=0
测试一下：
grub2-mkconfig
要是没错的话，就运行下面这条命令：
更新grub
grub2-mkconfig -o /boot/grub2/grub.cfg

此时重启
reboot
ifconfig

如果网卡的名字叫：eno16777984:
cd /etc/sysconfig/network-scripts
cp ifcfg-eno16777984 ifcfg-eth0
vi ifcfg-eth0
将里面的“eno16777984” 改为 “eth0”即可。
service network restart
nmcli con show
这时候，应该能看到网络接口的名称被更改为eth0。

查看网卡物理连接情况
mii-tool eth0

查看网关
route -n
-n 不解析主机名

网络配置命令
ifconfig <网络接口名 eth0> <ip 地址> [netmask 子网掩码]
ifup <接口>  --启用网卡
ifdown <接口> --禁用网卡

添加网关
route add default gw <网关ip>
route add -host <指定ip> gw <网关ip>
route add -net <指定网段> netmask <子网掩码> gw <网关ip>

ip命令
ip addr ls

```
![[Pasted image 20220327203457.png]]

ip命令
ifconfig不会很快消失，但是它的较新版本ip更加强大，最终将取代它。
![[Pasted image 20220403130707.png]]
[ifconfig与ip使用对比](https://www.gbase8.cn/2035)

网络故障排除命令
```text
ping
traceroute
mtr
nslookup
telnet
tcpdump
netstat
ss

```

网络服务管理
systemctl作为service的替代工具。
![[Pasted image 20220403132047.png]]
网络配置文件
```text
ifcfg-eth0
/etc/hosts
```

[chkconfig](https://www.runoob.com/linux/linux-comm-chkconfig.html)



# 3 服务管理

# 4 shell脚本

# 5 文本操作

# 6 常用服务搭建


