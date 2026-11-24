## 安装


Linux 可以直接下载

```bash
# Ubuntu
sudo apt install nmap
# CentOS
yum install nmap
```

如果是其他OS 直接进入官网下载对应操作系统的即可：[Download the Free Nmap Security Scanner for Linux/Mac/Windows](https://nmap.org/download.html).

---

## C 段主机扫描

使用 `ipconfig` 获取本机以太网的IP地址和子网掩码后，计算出 CIDR（无类别域间路由，Classless Inter-Domain Routing）。比如我的 `ipconfig` 返回结果如下：

```
以太网适配器 以太网:

   连接特定的 DNS 后缀 . . . . . . . : ustc.edu.cn
   IPv6 地址 . . . . . . . . . . . . : 2001:da8:d800:611:52ff:ef61:9b86:5f8c
   临时 IPv6 地址. . . . . . . . . . : 2001:da8:d800:611:2132:316c:3b9e:8991
   临时 IPv6 地址. . . . . . . . . . : 2001:da8:d800:611:21cb:5cd6:de06:c781
   临时 IPv6 地址. . . . . . . . . . : 2001:da8:d800:611:88a4:7ab3:c38e:30f8
   本地链接 IPv6 地址. . . . . . . . : fe80::652c:6ff8:624:42d0%5
   IPv4 地址 . . . . . . . . . . . . : 114.214.192.81
   子网掩码  . . . . . . . . . . . . : 255.255.252.0
   默认网关. . . . . . . . . . . . . : fe80::8261:6cff:fef5:4e01%5
                                       114.214.195.254
```

子网掩码展开成二进制表示，前22位代表网络，后10位代表主机。将 `114.214.192.81` 和 `255.255.252.0` 进行和运算可得 `114.214.192.0`，所以当前主机局域网的 CIDR 为 `114.214.192.0/22`。


```bash
nmap -sP 114.214.192.0/22
```
`-s` 参数代表端口扫描模式 scan type


返回结果：

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-03-12 23:48 中国标准时间
Nmap scan report for 114.214.192.1
Host is up (0.0010s latency).
MAC Address: 5C:DD:70:91:72:E2 (Hangzhou H3C Technologies, Limited)
Nmap scan report for 114.214.192.3
Host is up (0.0020s latency).
...
...
Nmap done: 1024 IP addresses (513 hosts up) scanned in 8.12 secondsds
```

- `Host is up`：主机目前活跃
- `0.0010s latency`：扫描请求到收到响应之间的延迟时间为0.001秒
- `Hangzhou H3C Technologies, Limited`：这个MAC地址是由杭州华三通信技术有限公司（H3C）注册的。H3C是一家提供网络设备和解决方案的公司，所以这台设备可能是一个路由器、交换机或其他网络设备。

> 114.214.192.1 不显示制造商是因为网络位的第一个和最后一个网络IP是保留IP，分别代表本机和广播。

---


## 单 IP 扫描

nmap 可以直接接受 IP 输入，默认就是扫描该 IP，可以加上 `-vv` 参数来展示更加详细的扫描结果。

```
nmap -vv 114.214.192.3
```

返回结果：

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-03-12 19:45 中国标准时间
Initiating ARP Ping Scan at 19:45
Scanning 114.214.192.3 [1 port]
Completed ARP Ping Scan at 19:45, 0.04s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 19:45
Completed Parallel DNS resolution of 1 host. at 19:45, 5.51s elapsed
Initiating SYN Stealth Scan at 19:45
Scanning 114.214.192.3 [1000 ports]
Completed SYN Stealth Scan at 19:45, 1.68s elapsed (1000 total ports)
Nmap scan report for 114.214.192.3
Host is up, received arp-response (0.023s latency).
Scanned at 2024-03-12 19:45:40 中国标准时间 for 1s
All 1000 scanned ports on 114.214.192.3 are in ignored states.
Not shown: 1000 closed tcp ports (reset)
MAC Address: 5C:DD:70:91:72:E2 (Hangzhou H3C Technologies, Limited)

Read data files from: C:\Program Files (x86)\Nmap
Nmap done: 1 IP address (1 host up) scanned in 7.37 seconds
           Raw packets sent: 1009 (44.380KB) | Rcvd: 1001 (40.028KB)
```

下面是简单的返回结果解读：

> #### ARP Ping 扫描
Initiating ARP Ping Scan at 19:45
Scanning 114.214.192.3 [1 port]
Completed ARP Ping Scan at 19:45, 0.04s elapsed (1 total hosts)
- nmap 使用 ARP Ping 来检测目标主机是否在线。ARP Ping 扫描用于确定局域网内的设备是否活跃。
- 扫描结果显示目标主机 114.214.192.3 在线，并且给出了接收 ARP 响应的延迟时间 0.023s。

<div></div>

> #### DNS 解析
Initiating Parallel DNS resolution of 1 host. at 19:45
Completed Parallel DNS resolution of 1 host. at 19:45, 5.51s elapsed
- nmap 还进行了并行的 DNS 解析，尝试解析目标 IP 地址对应的域名，但报告中没有显示解析结果。
- DNS 解析耗时 5.51s。

<div></div>

> #### SYN Stealth 扫描
Initiating Parallel DNS resolution of 1 host. at 19:45
Completed Parallel DNS resolution of 1 host. at 19:45, 5.51s elapsed
- nmap 进行了 SYN Stealth 扫描，这是一种隐蔽的 TCP 扫描方式，通常不会被防火墙记录。
- 扫描了目标 IP 地址上的 1000 个端口。
- 扫描耗时 1.68s。

<div></div>

> #### 扫描结果
Initiating Parallel DNS resolution of 1 host. at 19:45
Completed Parallel DNS resolution of 1 host. at 19:45, 5.51s elapsed
- 扫描报告表明所有扫描的 1000 个端口都处于“ignored states”。这通常意味着这些端口没有被明确标识为开放或关闭，可能是因为 nmap 没有足够的权限或信息来确定它们的状态。
- 报告还指出 1000 个 TCP 端口被重置（closed tcp ports (reset)），这通常意味着 nmap 尝试建立连接时，目标主机对这些端口做出了响应，表明它们是关闭的。

---

## 单机端口扫描

### 扫描指定端口

使用 `nmap -p 端口 IP` 即可扫描，其中多个端口可以用逗号隔开，扫描 a 到 b 范围内的使用 `a-b`。比如扫描 `114.214.195.241` 的 80，443，801 这三个端口：

```bash
nmap -p "80,443,801" 114.214.195.241
```
返回结果：

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-03-12 23:32 中国标准时间
Nmap scan report for 114.214.195.241
Host is up (0.014s latency).

PORT    STATE    SERVICE
80/tcp  filtered http
443/tcp filtered https
801/tcp filtered device
MAC Address: 5C:DD:70:91:72:E2 (Hangzhou H3C Technologies, Limited)

Nmap done: 1 IP address (1 host up) scanned in 1.53 seconds
```

上面的结果表示三个端口都处于 `filtered` 状态，这代表它们可能处于某种网络级别的过滤器（例如防火墙）下。

### 扫描所有端口

`nmap IP` 默认就是扫描前 10000 个端口：

```bash
nmap 101.43.239.71
```

返回结果：

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-03-12 23:42 中国标准时间
Nmap scan report for 101.43.239.71
Host is up (0.022s latency).
Not shown: 992 filtered tcp ports (no-response)
PORT     STATE  SERVICE
22/tcp   open   ssh
80/tcp   open   http
443/tcp  open   https
3389/tcp closed ms-wbt-server
8001/tcp open   vcom-tunnel
8002/tcp open   teradataordbms
8888/tcp closed sun-answerbook
9090/tcp closed zeus-admin

Nmap done: 1 IP address (1 host up) scanned in 4.88 seconds
```
---


## 路由跟踪

`--traceroute` 代表路由跟踪， `-sn` 代表不进行端口扫描
```bash
nmap --traceroute -sn 101.43.239.71
```

返回结果

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-03-12 23:47 中国标准时间
Nmap scan report for 101.43.239.71
Host is up (0.019s latency).

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   1.00 ms  0.0.0.0
2   1.00 ms  202.38.96.158
3   0.00 ms  202.38.64.58
4   0.00 ms  202.38.96.201
5   ... 9
10  19.00 ms 123.126.0.38
11  34.00 ms 61.49.142.154
12  ...
13  21.00 ms 101.43.239.71

Nmap done: 1 IP address (1 host up) scanned in 6.07 seconds
```
---

## 试探操作系统类型

```bash
nmap -O 101.43.239.71
```

返回结果：

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-03-12 23:51 中国标准时间
Nmap scan report for 101.43.239.71
Host is up (0.041s latency).
Not shown: 992 filtered tcp ports (no-response)
PORT     STATE  SERVICE
22/tcp   open   ssh
80/tcp   open   http
443/tcp  open   https
3389/tcp closed ms-wbt-server
8001/tcp open   vcom-tunnel
8002/tcp open   teradataordbms
8888/tcp closed sun-answerbook
9090/tcp closed zeus-admin
Aggressive OS guesses: Linux 3.10 - 4.11 (92%), Linux 5.1 (89%), Linux 3.18 (89%), Linux 3.2 - 4.9 (89%), Linux 3.13 (88%), Linux 3.13 or 4.2 (88%), Linux 4.10 (88%), Linux 4.2 (88%), Linux 4.4 (88%), Asus RT-AC66U WAP (88%)
No exact OS matches for host (test conditions non-ideal).

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.62 seconds
```

---


## all 全部

使用 `-A` 参数返回最详细的结果（包括之前除了C段扫描外的一切结果）：

```bash
nmap -A 49.232.136.95
```

返回结果：

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-03-13 00:12 中国标准时间
Nmap scan report for 49.232.136.95
Host is up (0.032s latency).
Not shown: 915 filtered tcp ports (no-response), 82 closed tcp ports (reset)
PORT      STATE SERVICE         VERSION
22/tcp    open  ssh             OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 0c:23:ed:9a:eb:b1:6a:23:38:29:9a:8d:fa:3f:df:35 (RSA)
|   256 5f:52:71:e9:9d:ab:d3:e5:6f:a7:b9:24:5a:bb:7f:16 (ECDSA)
|_  256 d8:25:fa:0f:7f:fe:0a:b5:f3:58:d0:eb:f9:cb:b3:5e (ED25519)
80/tcp    open  http            nginx 1.14.0 (Ubuntu)
|_http-title: Welcome to nginx!
|_http-server-header: nginx/1.14.0 (Ubuntu)
10001/tcp open  ssl/scp-config?
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject:
| Not valid before: 1-01-01T00:00:00
|_Not valid after:  1-01-01T00:00:00
Aggressive OS guesses: Linux 5.0 - 5.4 (92%), Linux 5.0 (91%), Linux 4.15 - 5.8 (89%), HP P2000 G3 NAS device (89%), Linux 2.6.32 (88%), Infomir MAG-250 set-top box (88%), Ubiquiti AirMax NanoStation WAP (Linux 2.6.32) (88%), Linux 5.0 - 5.5 (88%), Netgear RAIDiator 4.2.21 (Linux 2.6.37) (88%), Linux 5.3 - 5.4 (88%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 13 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   1.00 ms  0.0.0.0
2   1.00 ms  202.38.96.158
3   1.00 ms  202.38.64.58
4   1.00 ms  202.38.96.201
5   ... 9
10  41.00 ms 123.126.0.218
11  24.00 ms 61.49.142.146
12  ...
13  23.00 ms 49.232.136.95

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 98.79 seconds
```