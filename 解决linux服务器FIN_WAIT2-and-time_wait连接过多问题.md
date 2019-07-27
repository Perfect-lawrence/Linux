# 解决linux服务器FIN_WAIT2,time_wait连接过多问题

```bash
netstat -an|awk ‘/tcp/ {print $6}’|sort|uniq -c
     4 CLOSE_WAIT
    165 ESTABLISHED
      3 FIN_WAIT1
    139 FIN_WAIT2
      6 LISTEN
     16 TIME_WAIT
```
* 状态：描述
* CLOSED：无连接是活动的或正在进行
* LISTEN：服务器在等待进入呼叫
* SYN_RECV：一个连接请求已经到达，等待确认
* SYN_SENT：应用已经开始，打开一个连接
* ESTABLISHED：正常数据传输状态
* FIN_WAIT1：应用说它已经完成
* FIN_WAIT2：另一边已同意释放
* ITMED_WAIT：等待所有分组死掉
* CLOSING：两边同时尝试关闭
* TIME_WAIT：另一边已初始化一个释放
* LAST_ACK：等待所有分组死掉

## 如发现系统存在大量TIME_WAIT状态的连接，通过调整内核参数解决

```bash
vim /etc/sysctl.conf
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 0
net.ipv4.tcp_fin_timeout = 30

/sbin/sysctl -p 
```
* net.ipv4.tcp_syncookies = 1 表示开启SYN cookies。当出现SYN等待队列溢出时，启用cookies来处理，可防范少量SYN攻击，默认为0，表示关闭；

* net.ipv4.tcp_tw_reuse = 1 表示开启重用。允许将TIME-WAIT sockets重新用于新的TCP连接，默认为0，表示关闭；

* net.ipv4.tcp_tw_recycle = 0 表示开启TCP连接中TIME-WAIT sockets的快速回收，默认为0，表示关闭。 这个选项不推荐启用。在NAT(Network Address Translation)网络下，会导致大量的TCP连接建立错误。

* net.ipv4.tcp_fin_timeout 修改系統默认的 TIMEOUT 时间
