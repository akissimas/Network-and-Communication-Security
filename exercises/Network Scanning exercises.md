# [Network Scanning - exercises](https://git.swarmlab.io:3000/docs/Documentation)

1. Connect to master
    - master ip address: **172.20.0.2**

2. Find All TCP Ports connections (`netstat -at`)

```bash
Active Internet connections (servers and established)
	Proto Recv-Q Send-Q Local Address           Foreign Address         State      
	tcp        0      0 localhost:40395         0.0.0.0:*               LISTEN     
	tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN     
	tcp6       0      0 [::]:22                 [::]:*                  LISTEN
```

3. Find All UDP Ports connections (`netstat -au`)

```bash
	Proto Recv-Q Send-Q Local Address           Foreign Address         State      
	udp        0      0 localhost:35469         0.0.0.0:*
```

4. run

```bash
cd /project/courses/fluentd
# run
./fluentd.yml.sh
```

5. Find All TCP Ports connections (`netstat -at`)

```bash
	Active Internet connections (servers and established)
	Proto Recv-Q Send-Q Local Address           Foreign Address         State      
	tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN     
	tcp        0      0 localhost:37661         0.0.0.0:*               LISTEN     
	tcp        0      0 f2e4b48face7:42718      hybrid-linux_worker_:22 TIME_WAIT  
	tcp        0      0 f2e4b48face7:39500      hybrid-linux_worker_:22 TIME_WAIT  
	tcp        0      0 f2e4b48face7:41112      hybrid-linux_worker_:22 TIME_WAIT  
	tcp        0      0 f2e4b48face7:50696      hybrid-linux_worker_:22 TIME_WAIT  
	tcp        0      0 f2e4b48face7:56710      f2e4b48face7:22         TIME_WAIT  
	tcp6       0      0 [::]:22                 [::]:*                  LISTEN     
```

6. Find All UDP Ports connections (`netstat -au`)

```bash
	Proto Recv-Q Send-Q Local Address           Foreign Address         State      
	udp        0      0 localhost:35930         0.0.0.0:*                     
```

7. Find All live hosts (`nmap -sP 172.20.0.* | grep Nmap | cut -d' ' -f5-6`)

```bash
	hybrid-linux_worker_1.hybrid-linux_hybrid-linux (172.20.0.3)
	hybrid-linux_worker_2.hybrid-linux_hybrid-linux (172.20.0.4)
	hybrid-linux_worker_3.hybrid-linux_hybrid-linux (172.20.0.5)
	hybrid-linux_worker_4.hybrid-linux_hybrid-linux (172.20.0.6)
```

8. Find All open TCP Ports in All hosts (`nmap -sT -p0-65535 172.20.0.*`)

```bash
	Nmap scan report for 172.20.0.1 (172.20.0.1)
	Host is up (0.00065s latency).
	Not shown: 65531 closed ports
	PORT      STATE SERVICE
	3080/tcp  open  stm_pproc
	3088/tcp  open  xdtp
	5000/tcp  open  upnp
	35981/tcp open  unknown

	Nmap scan report for f2e4b48face7 (172.20.0.2)
	Host is up (0.000093s latency).
	Not shown: 65534 closed ports
	PORT   STATE SERVICE
	22/tcp open  ssh

	Nmap scan report for hybrid-linux_worker_1.hybrid-linux_hybrid-linux (172.20.0.3)
	Host is up (0.00088s latency).
	Not shown: 65534 closed ports
	PORT   STATE SERVICE
	22/tcp open  ssh

	Nmap scan report for hybrid-linux_worker_2.hybrid-linux_hybrid-linux (172.20.0.4)
	Host is up (0.00072s latency).
	Not shown: 65534 closed ports
	PORT   STATE SERVICE
	22/tcp open  ssh

	Nmap scan report for hybrid-linux_worker_3.hybrid-linux_hybrid-linux (172.20.0.5)
	Host is up (0.00027s latency).
	Not shown: 65534 closed ports
	PORT   STATE SERVICE
	22/tcp open  ssh

	Nmap scan report for hybrid-linux_worker_4.hybrid-linux_hybrid-linux (172.20.0.6)
	Host is up (0.00029s latency).
	Not shown: 65534 closed ports
	PORT   STATE SERVICE
	22/tcp open  ssh
```

9. SSH connect

```bash
	ssh docker@172.20.0.3

	## user syntax ##
	ssh -t docker@172.20.0.3 'ip a'

	## sudo syntax ##
	ssh -t docker@172.20.0.3 'echo docker | sudo -S cat /etc/passwd'
```
