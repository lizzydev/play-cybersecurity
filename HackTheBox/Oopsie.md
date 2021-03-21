# Oopsie
## Level: Easy

Ok, it took me way too long to get connected to this machine. But we learned! First things first, I needed to understand how Hack The Box worked and how access
the machines. Before I connected to their vpn, I kept hitting the given IP address with `nmap`
and getting this response:

```
sudo nmap -sS -A 10.10.10.28
[sudo] password for lizzy:
Starting Nmap 7.80 ( https://nmap.org ) at 2021-03-20 23:20 MDT
Note: Host seems down. If it is really up, but blocking our ping probes, try -Pn
Nmap done: 1 IP address (0 hosts up) scanned in 3.33 seconds
```

Which in hindsight makes sense, I wasn't connected to the vpn so when I hit that IP Address I was rejected. Once I was connected to the right VPN, here's what `nmap` looked like:

<details>
    <summary>lizzy@Donut:~/git$ sudo nmap -sS -A 10.10.10.28</summary>

    ```
    Starting Nmap 7.80 ( https://nmap.org ) at 2021-03-20 23:29 MDT
    Warning: 10.10.10.28 giving up on port because retransmission cap hit (10).
    Nmap scan report for 10.10.10.28
    Host is up (0.063s latency).
    Not shown: 992 closed ports
    PORT      STATE    SERVICE     VERSION
    22/tcp    open     ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey:
    |   2048 61:e4:3f:d4:1e:e2:b2:f1:0d:3c:ed:36:28:36:67:c7 (RSA)
    |   256 24:1d:a4:17:d4:e3:2a:9c:90:5c:30:58:8f:60:77:8d (ECDSA)
    |_  256 78:03:0e:b4:a1:af:e5:c2:f9:8d:29:05:3e:29:c9:f2 (ED25519)
    80/tcp    open     http        Apache httpd 2.4.29 ((Ubuntu))
    |_http-server-header: Apache/2.4.29 (Ubuntu)
    |_http-title: Welcome
    89/tcp    filtered su-mit-tg
    2008/tcp  filtered conf
    3766/tcp  filtered sitewatch-s
    8086/tcp  filtered d-s-n
    9100/tcp  filtered jetdirect
    58080/tcp filtered unknown
    No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
    TCP/IP fingerprint:
    OS:SCAN(V=7.80%E=4%D=3/20%OT=22%CT=1%CU=35572%PV=Y%DS=3%DC=T%G=Y%TM=6056DA9
    OS:4%P=x86_64-pc-linux-gnu)SEQ(SP=108%GCD=1%ISR=109%TI=Z%CI=Z%II=I%TS=A)OPS
    OS:(O1=M550ST11NW7%O2=M550ST11NW7%O3=M550NNT11NW7%O4=M550ST11NW7%O5=M550ST1
    OS:1NW7%O6=M550ST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)ECN
    OS:(R=Y%DF=Y%T=40%W=FAF0%O=M550NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
    OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
    OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
    OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
    OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=ED75%RUD=G)IE(R=Y%DFI=N%T=40
    OS:%CD=S)

    Network Distance: 3 hops
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

    TRACEROUTE (using port 111/tcp)
    HOP RTT      ADDRESS
    1   0.45 ms  Donut.mshome.net (***.***.***.***)
    2   64.51 ms 10.10.14.1
    3   64.69 ms 10.10.10.28

    OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 234.23 seconds
    ```
    
</details>
<br>
I cheated and looked at the walkthrough on this one because I got stuck connecting to the VPN. So the `nmap` command is from that walkthrough, but I want to break it down.

## Nmap Breakdown
<a name="nmapBreakdown"></a>
<br>
`nmap -sS -A {IP Address}`
* `-sS` : [TCP SYN scan](https://nmap.org/book/synscan.html)
* * see other [port scanning techniques](https://nmap.org/book/man-port-scanning-techniques.html)
* `-A` : [Aggressive scan options](https://nmap.org/book/man-misc-options.html), to get more info than the default