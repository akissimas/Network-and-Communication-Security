# Task 1

## Local terminal
1. Εxecute the command (`ifconfig`) to find the IP you want to attack.

```bash
...
br-8d224f6589fc: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.22.0.1  netmask 255.255.0.0  broadcast 172.22.255.255
        inet6 fe80::42:5eff:fe8f:68c8  prefixlen 64  scopeid 0x20<link>
        ether 02:42:5e:8f:68:c8  txqueuelen 0  (Ethernet)
        RX packets 2  bytes 56 (56.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 39  bytes 5337 (5.3 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
...
```
The ip of our interest is **172.22.0.1**.

2. Εxecute the command (`nmap -sT -p0- 172.22.0.*`) to find all open TCP ports on all IP addresses.

```bash
Nmap scan report for akis-virtual-machine (172.22.0.1)
Host is up (0.00019s latency).
Not shown: 65532 closed ports
PORT     STATE SERVICE
3000/tcp open  ppp
3080/tcp open  stm_pproc
3088/tcp open  xdtp
7085/tcp open  unknown

Nmap scan report for 172.22.0.2
Host is up (0.00038s latency).
Not shown: 65535 closed ports
PORT      STATE SERVICE
27017/tcp open  mongod

Nmap scan report for 172.22.0.3
Host is up (0.00036s latency).
Not shown: 65535 closed ports
PORT     STATE SERVICE
6379/tcp open  redis

Nmap scan report for 172.22.0.4
Host is up (0.00017s latency).
Not shown: 65535 closed ports
PORT     STATE SERVICE
3001/tcp open  nessus

Nmap scan report for 172.22.0.5
Host is up (0.00020s latency).
Not shown: 65534 closed ports
PORT      STATE SERVICE
3000/tcp  open  ppp
40605/tcp open  unknown

Nmap scan report for 172.22.0.6
Host is up (0.00022s latency).
Not shown: 65535 closed ports
PORT     STATE SERVICE
8080/tcp open  http-proxy
```

Where:

- **-sT**: TCP connect port scan.

- **-p0-**: Scan all the ports.

The IP we will attack is **172.22.0.4** on port **3001**.

3. (`sudo hping3 -S 172.22.0.4 -p 3001`).

```bash
HPING 172.22.0.4 (br-8d224f6589fc 172.22.0.4): S set, 40 headers + 0 data bytes
len=44 ip=172.22.0.4 ttl=64 DF id=0 sport=3001 flags=SA seq=0 win=64240 rtt=8.6 ms
len=44 ip=172.22.0.4 ttl=64 DF id=0 sport=3001 flags=SA seq=1 win=64240 rtt=3.9 ms
len=44 ip=172.22.0.4 ttl=64 DF id=0 sport=3001 flags=SA seq=2 win=64240 rtt=7.4 ms
len=44 ip=172.22.0.4 ttl=64 DF id=0 sport=3001 flags=SA seq=3 win=64240 rtt=7.0 ms
^C
--- 172.22.0.4 hping statistic ---
4 packets transmitted, 4 packets received, 0% packet loss
round-trip min/avg/max = 3.9/6.7/8.6 ms
```

Where:

- **-S**: Set SYN tcp flag.

- **-p**: The open port that we will use.

- Also, we can use the argument **--flood** to sent packets as fast as possible, without taking care to show incoming replies.

---

## Dummy_service

1. Εxecute (`docker exec -it -uroot --privileged dummy_service /bin/bash`) to connect to dummy_service.

2. Install **tcpdump**.

```bash
apk update
apk add tcpdump
```

3. Εxecute (`tcpdump`).

```bash
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
15:56:53.006777 IP akis-virtual-machine.local.1303 > 6f25f7be4e6b.3001: Flags [S], seq 624811779, win 512, length 0
15:56:53.006826 IP 6f25f7be4e6b.3001 > akis-virtual-machine.local.1303: Flags [S.], seq 2202382046, ack 624811780, win 64240, options [mss 1460], length 0
15:56:53.006844 IP akis-virtual-machine.local.1303 > 6f25f7be4e6b.3001: Flags [R], seq 624811780, win 0, length 0
15:56:54.007402 IP akis-virtual-machine.local.1304 > 6f25f7be4e6b.3001: Flags [S], seq 827161144, win 512, length 0
15:56:54.007465 IP 6f25f7be4e6b.3001 > akis-virtual-machine.local.1304: Flags [S.], seq 2507159814, ack 827161145, win 64240, options [mss 1460], length 0
15:56:54.007507 IP akis-virtual-machine.local.1304 > 6f25f7be4e6b.3001: Flags [R], seq 827161145, win 0, length 0
15:56:54.012212 IP 6f25f7be4e6b.56370 > mongo.poc-datacollector_datacollector-net.27017: Flags [P.], seq 1783226307:1783226365, ack 1922913299, win 903, options [nop,nop,TS val 1097471502 ecr 970704941], length 58
15:56:54.012790 IP mongo.poc-datacollector_datacollector-net.27017 > 6f25f7be4e6b.56370: Flags [P.], seq 1:320, ack 58, win 507, options [nop,nop,TS val 970714942 ecr 1097471502], length 319
15:56:54.012815 IP 6f25f7be4e6b.56370 > mongo.poc-datacollector_datacollector-net.27017: Flags [.], ack 320, win 912, options [nop,nop,TS val 1097471502 ecr 970714942], length 0
15:56:55.007903 IP akis-virtual-machine.local.1305 > 6f25f7be4e6b.3001: Flags [S], seq 72684301, win 512, length 0
15:56:55.007958 IP 6f25f7be4e6b.3001 > akis-virtual-machine.local.1305: Flags [S.], seq 830661964, ack 72684302, win 64240, options [mss 1460], length 0
15:56:55.007998 IP akis-virtual-machine.local.1305 > 6f25f7be4e6b.3001: Flags [R], seq 72684302, win 0, length 0
15:56:56.008376 IP akis-virtual-machine.local.1306 > 6f25f7be4e6b.3001: Flags [S], seq 747836475, win 512, length 0
15:56:56.008416 IP 6f25f7be4e6b.3001 > akis-virtual-machine.local.1306: Flags [S.], seq 2729792661, ack 747836476, win 64240, options [mss 1460], length 0
15:56:56.008445 IP akis-virtual-machine.local.1306 > 6f25f7be4e6b.3001: Flags [R], seq 747836476, win 0, length 0
15:56:58.034958 ARP, Request who-has akis-virtual-machine.local tell 6f25f7be4e6b, length 28
15:56:58.035035 ARP, Request who-has 6f25f7be4e6b tell akis-virtual-machine.local, length 28
15:56:58.035039 ARP, Reply 6f25f7be4e6b is-at 02:42:ac:16:00:04 (oui Unknown), length 28
15:56:58.035040 ARP, Reply akis-virtual-machine.local is-at 02:42:5e:8f:68:c8 (oui Unknown), length 28
```
