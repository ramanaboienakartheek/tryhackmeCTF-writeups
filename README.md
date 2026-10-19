
# BASIC PENTESTING
### Web App Testing And Privilage Escalation
This room is a perfect place for starting one's career in Pentesting. It shows some of the basic tools and rules, and relies on the basic knowledge of linux and  basic Pentesting tools like Nmap, Gobuster etc.
## Tasks
1. Deploy the Machine and connect to our network
2. I have used the attached Lab Machine and the Attackbox directly, instead of connecting through SSH via OpenVPN. Pro tip: Skipping the procees of connection through SSH via OpenVPN saves a lot of time and energy, in terms of downloading tools and wordlists. Since this room uses more than 1 tool and an infamous built-in list containing over 220,000 common folder and file names (like /admin, /login, /uploads, /backup).
3. Find the services exposed by the machine.

    This is where the basics of reconnaisance come into play. I personally use nmap which is default on the Kali 24.04.5 LTS.1 install I have. We start with a basic nmap command <mark>{nmap -sV -sC -oN nmap/initial [webappIP]}</mark>. This nmap command is commonly used by Ippsec and I have incorporated it into my setup as well. the -sV probes all open ports it finds to determine if we can get the service/version information. The -sC flag runs scripts against open ports as well to determine if there are external/common vulnerabilities that we can use outright. And -oN outputs the output of the nmap scan into a normal format so you have it for future use. In here i directed the output to my directory called Nmap and to file called Initial. The results of the given command are as follows:

