ADSL上网，Ubuntu下是可以的，虽然以前没用过拨号上网，不过查了查也不是很麻烦。

打开终端配置上网：
zhancang@ubuntu:~$ sudo pppoeconf
开始配置上网，出来的是终端界面，因为是笔记本记得还有一个让选择网卡的，选择有线猫的那个就行，之后输入宽带用户名（好多人提示删除username，看来有人被害过）和密码。

联网：
zhancang@ubuntu:~$ sudo pon dsl-provider
[sudo] password for zhancang: 此处输入系统密码，密码不显示
Plugin rp-pppoe.so loaded.
RP-PPPoE plugin version 3.8p compiled against pppd 2.4.5
zhancang@ubuntu:~$

断网：
zhancang@ubuntu:~$ sudo poff

查看日志（ 断网下）：
zhancang@ubuntu:~$ plog
Jan 15 15:34:53 ubuntu pppd[1879]: Terminating on signal 15
Jan 15 15:34:53 ubuntu pppd[1879]: Connect time 76.6 minutes.
Jan 15 15:34:53 ubuntu pppd[1879]: Sent 1524670 bytes, received 6141748 bytes.
Jan 15 15:34:53 ubuntu pppd[1879]: Connection terminated.
Jan 15 15:34:53 ubuntu pppd[1879]: Exit.
zhancang@ubuntu:~$

查看接口信息（ 联网下）：
zhancang@ubuntu:~$ ifconfig ppp0
ppp0      Link encap:点对点协议
inet 地址:131.11.209.211 点对点:131.11.208.1(已改) 掩码:255.255.255.255
UP POINTOPOINT RUNNING NOARP MULTICAST MTU:1492 跃点数:1
接收数据包:4 错误:0 丢弃:0 过载:0 帧数:0
发送数据包:4 错误:0 丢弃:0 过载:0 载波:0
碰撞:0 发送队列长度:3
接收字节:117 (117.0 B) 发送字节:145 (145.0 B)

zhancang@ubuntu:~$
