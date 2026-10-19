
# BASIC PENTESTING
### Web App Testing And Privilage Escalation
This room is a perfect place for starting one's career in Pentesting. It shows some of the basic tools and rules, and relies on the basic knowledge of linux and basic Pentesting tools like Nmap, Gobuster etc.
## Tasks
1. Deploy the Machine and connect to our network
2. I have used the attached Lab Machine and the Attackbox directly, instead of connecting through SSH via OpenVPN. Pro tip: Skipping the procees of connection through SSH via OpenVPN saves a lot of time and energy, in terms of downloading tools and wordlists. Since this room uses more than 1 tool and an infamous built-in list containing over 220,000 common folder and file names (like /admin, /login, /uploads, /backup).
3. We find the services exposed by the machine.
   
    This is where the basics of reconnaisance come into play. I personally use nmap which is default on the Kali 24.04.5 LTS.1 install I have. We start with a basic nmap command <mark>nmap -sV -sC -oN nmap/initial [webappIP]</mark>. This nmap command is commonly used by Ippsec and I have incorporated it into my setup as well. the -sV probes all open ports it finds to determine if we can get the service/version information. The -sC flag runs scripts against open ports as well to determine if there are external/common vulnerabilities that we can use outright. And -oN outputs the output of the nmap scan into a normal format so you have it for future use. In here i directed the output to my directory called Nmap and to file called Initial. The results of the given command are as follows:

<mark>Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-10-19 08:08 UTC
Nmap scan report for ip-10-49-141-174.ap-south-1.compute.internal (10.49.141.174)
Host is up (0.00016s latency).
Not shown: 994 closed tcp ports (reset)
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 82:d4:7e:d8:77:54:ce:2e:8e:b5:f4:13:0b:51:a5:9d (RSA)
|   256 41:06:ad:b0:62:cc:86:b8:6f:5c:35:85:ba:01:6c:46 (ECDSA)
|_  256 e4:ab:8d:fd:db:c8:52:47:79:41:7b:3a:3e:e2:e9:8d (ED25519)
80/tcp   open  http        Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.41 (Ubuntu)
139/tcp  open  netbios-ssn Samba smbd 4.6.2
445/tcp  open  netbios-ssn Samba smbd 4.6.2
8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
| ajp-methods: 
|_  Supported methods: GET HEAD POST OPTIONS
8080/tcp open  http        Apache Tomcat 9.0.7
|_http-title: Apache Tomcat/9.0.7
|_http-favicon: Apache Tomcat
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_nbstat: NetBIOS name: BASIC2, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2026-10-19T08:09:08
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.14 seconds</mark>



