SSH stands for SECURE SHELL, this a remote conection, that gives you a shell

Now to practice this lets use overthewire site, overthewire is a community that can help to learn and practice security concepts in the form of games.

  

This begins with Bandit, this wargame is aimed at beginners.

[https://overthewire.org/wargames/bandit/](https://overthewire.org/wargames/bandit/)

  

Now this begin on Level 0

## Level 0

The fierst level provides us the username **bandit0** and password **bandit0**

To connect we use

`sshpass -p 'bandit0' ssh bandit0@bandit.labs.overthewire.org -p 2220`

  

First of all let’s the TERM variable so we can use an interactive shell

![[/Untitled 558.png|Untitled 558.png]]

Let’s change this to be xterm

Now we can use this interactive shell

![[/Untitled 1 23.png|Untitled 1 23.png]]

  

By entering and listing i see a file called **readme,** if i print it

![[/Untitled 2 18.png|Untitled 2 18.png]]

This is the password for the user **bandit1**

## Level 0 → 1

In this level the file is located in a directory called ‘-’ located in /home

  

Now this file has a - as filename, and this for the commands means that we are about to pass a parameter.

![[/Untitled 3 18.png|Untitled 3 18.png]]

For the command cat to work we have to use the absolut path, and as always there is multiples ways of doing this

![[/Untitled 4 18.png|Untitled 4 18.png]]

Now this is the pass for the next lab, which is **bandit2**

## Level 1 → 2

Now we have a file called **spaces in this filename,** and why is this a difficult name?

  

Well if we the filesystem would take this as separates names

![[/Untitled 5 13.png|Untitled 5 13.png]]

To use cat (or any other name) we do it like this

![[/Untitled 6 11.png|Untitled 6 11.png]]

Now we have the pass for bandit2

## Level 2 → 3

Now in this case we have a hidden file

  
So listing with ls wont show the file unless i use the parameter -a (all)  

![[/Untitled 7 11.png|Untitled 7 11.png]]

![[/Untitled 8 11.png|Untitled 8 11.png]]

Password for bandit4

[https://en.wikipedia.org/wiki/List_of_file_signatures](https://en.wikipedia.org/wiki/List_of_file_signatures)

## Level 4 → 5

In this case when i list i have many files, and listing one by one would be time-consuming

![[/Untitled 9 9.png|Untitled 9 9.png]]

![[/Untitled 10 7.png|Untitled 10 7.png]]

In this case if i want to know the file type i have to escape the dash at the beggining because this is interpreted as parameter

![[/Untitled 11 7.png|Untitled 11 7.png]]

While trying to use commands i realized that i cannot use ls -l to use file, because this gives a string.  
In this case i have to use find to  
_find_ all the files

![[/Untitled 12 6.png|Untitled 12 6.png]]

Now i have to know which one of this is readable or what type of data they have inside

![[/Untitled 13 6.png|Untitled 13 6.png]]

Now I cat the file with ASCII Text

![[/Untitled 14 6.png|Untitled 14 6.png]]

This is the next pass

[https://en.wikipedia.org/wiki/List_of_file_signatures](https://en.wikipedia.org/wiki/List_of_file_signatures)

## Level 5 → 6

IN this level not only we have a lot of files we also have a lot of directories

![[/Untitled 15 6.png|Untitled 15 6.png]]

Knowing that the file is Humand Readable, 1033 bytes in size, and its not executable

![[/Untitled 16 6.png|Untitled 16 6.png]]

Next pass

`find . -type f ! -executable -size 1033c`

## Level 6 → 7

In this case the pass is stored **somewhere in the server** and it has the following propierties

Owned by user bandit7

Owned by group bandit6

33 bytesin sizes

![[/Untitled 17 5.png|Untitled 17 5.png]]

The file is located in /var/lib/dpkg/info/bandit7.password

![[/Untitled 18 5.png|Untitled 18 5.png]]

## Level 7 → 8

In this case the level tells us that the password is on the **data.txt** file, and is next to the word millionth

Using cat an then grep found this

![[/Untitled 19 5.png|Untitled 19 5.png]]

Usign AWK I prettify the pass

![[/Untitled 20 5.png|Untitled 20 5.png]]

This is the next pass

> AWK cheatsheet: [https://bl831.als.lbl.gov/~gmeigs/scripting_help/awk_cheat_sheet.pdf](https://bl831.als.lbl.gov/~gmeigs/scripting_help/awk_cheat_sheet.pdf)  
> AWK Shortcuts:  
> [https://www.shortcutfoo.com/app/dojos/awk/cheatsheet](https://www.shortcutfoo.com/app/dojos/awk/cheatsheet)  
> CUT cheatsheet:  
> [https://bencane.com/2012/10/22/cheat-sheet-cutting-text-with-cut/](https://bencane.com/2012/10/22/cheat-sheet-cutting-text-with-cut/)

## Level 8 → 9

This level tells me that the pass is inside the **data.txt** and is the unique line

So using sort and uniq

![[/Untitled 21 5.png|Untitled 21 5.png]]

This is the next pass

  

> SORT Cheatsheet: [https://www.ibidemgroup.com/edu/tutorial-sort-linux-unix/](https://www.ibidemgroup.com/edu/tutorial-sort-linux-unix/)  
> UNIQ CHeatsheet:  
> [https://victorhckinthefreeworld.com/2021/10/21/el-comando-uniq-de-gnu/](https://victorhckinthefreeworld.com/2021/10/21/el-comando-uniq-de-gnu/)

## Level 9 → 10

In this level the pass is stored in the **data.txt** and is one of the few human readable and preceded by a few = chars

![[/Untitled 22 5.png|Untitled 22 5.png]]

Using awk and tail i make the pass pretty

![[/Untitled 23 4.png|Untitled 23 4.png]]

## Level 10 → 11

For this level we have the pass in the data.txt file, but it is encoded in b64

![[/Untitled 24 4.png|Untitled 24 4.png]]

So lets de-encode this with base64 command

![[/Untitled 25 4.png|Untitled 25 4.png]]

So here is the next pass

## Level 11 → 12

In this level we are told that the data inside data.txt is **rotated** by 13 positions

  

So, we know that the data is rotated 13 positions, looking up I know that this is called ROT13 cipher

[https://en.wikipedia.org/wiki/ROT13](https://en.wikipedia.org/wiki/ROT13)

  

So basically is rotating a letter 13 times so A → N

![[/Untitled 26 4.png|Untitled 26 4.png]]

In this case i know that the first letter is N because 13th times after the A

![[/Untitled 27 4.png|Untitled 27 4.png]]

Perfect, now lets print only the pass

![[/Untitled 28 4.png|Untitled 28 4.png]]

The next pass

## Level 12 → 13

In this level the passwd is stored in the **data.xt** file, but in this case the file is a hexdump of a repeatedly compressed file.

![[/Untitled 29 4.png|Untitled 29 4.png]]

So i know that this is a file that has been repeatedly compressed, but i do not know how many times, this could be 1 or 100, so lets make a script for this

  

![[/Untitled 30 4.png|Untitled 30 4.png]]

In this line i reversed the dumped hex data and stored it in the same file named data

![[/Untitled 31 4.png|Untitled 31 4.png]]

First , of all this is a compresed file, so ill rename it

  

Now we can begin to decompress this files N times

Ill use `7z x <filename>` to decompress

  

![[/Untitled 32 3.png|Untitled 32 3.png]]

In this case I am making a oneliner to parse the filename that is going to be extracted

![[/Untitled 33 3.png|Untitled 33 3.png]]

This is the final command that i will use in the script

![[/Untitled 34 3.png|Untitled 34 3.png]]

  

Script wprking

![[/Untitled 35 3.png|Untitled 35 3.png]]

Files decompresed

![[/Untitled 36 3.png|Untitled 36 3.png]]

The last one was a ASCII text, so it stopped there

![[/Untitled 37 3.png|Untitled 37 3.png]]

Script decompression

```Bash
#!/bin/bash

\#Function for interrupt script
function ctrl_c(){
 echo -e "\n\n[!]Saliendo...\n"
 exit 1
}

\#CTRL+C
trap ctrl_c INT

first_file_name="data.gz"
decompressed_file_name="$(7z l data.gz | tail-n 3 | head -n 1 | awk 'NF{print $NF}')" 17
7z x $first_file_name &>/dev/null


while [ $decompressed_file_name ]; do
echo -e "\n\n[+] Nuevo archivo descomprimido: $decompressed_file_name"
7z x $decompressed_file_name &>/dev/null
decompressed_file_name="$(7z 1 $decompressed_file_name | tail-n 3 | head -n 1 | awk 'NF{print $NF}')"
done
```

  

## Level 13→14

In this one I dont have a password, i get a ssh private key to log in, what can I do?

![[/Untitled 38 3.png|Untitled 38 3.png]]

So what is SSH? SSH stands for SecureSHell, and it one of ways to connect remotely to machine.

  
And what is a private key? A private key is one of the two keys involved in cryptography, along with the public key. As the name suggests, the private key should be kept confidential because it grants access to sensitive information. It is an essential component in cryptographic systems and is used for tasks like secure communication, digital signatures, and encryption. Unlike the public key, which can be freely shared, the private key must be safeguarded to prevent unauthorized access and ensure the security of the system.  

  

So in this case we are provided with a private key, so I ll try to connect with it

![[/Untitled 39 3.png|Untitled 39 3.png]]

And yes, we have permission

![[/Untitled 40 3.png|Untitled 40 3.png]]

And there it is, the password for the next lab

> SSH key pairs are two cryptographically secure keys that can be used to authenticate a client to an SSH server. Each key pair is composed of a public key and a private key.
> 
> The private key is held by the client and must be kept absolutely secret. Compromising the private key may allow an unauthorized attacker to log into servers that are configured with the associated public key without additional authentication. As an additional precaution, it is advisable to encrypt the key on disk with a passphrase.
> 
> The associated public key can be shared freely without any negative consequences. The public key can be used to encrypt messages that only the private key can decrypt. This property is used as a form of authentication using the key pair.

## Level 14→15

In this level we told that the password for the next level can be retrieved by submitting the password of the current level to the port 30000 @ localhost

  

Using nc, i connect to server and provided the pass

![[/Untitled 41 3.png|Untitled 41 3.png]]

> Netcat is a command line tool for writing and reading data on the network. For data transmission, Netcat uses TCP/IP and UDP network protocols. The tool originally comes from the Unix world; since then, it has spread to all platforms.
> 
> Thanks to its universality, Netcat is called "the Swiss Army Knife of TCP/IP". It can be used, for example, to diagnose errors and problems affecting the functionality and security of a network. Netcat can also scan ports, stream data or simply transfer data. In addition, it can configure chat and web servers and initiate mail queries. This minimalist software, developed in the mid-1990s, can operate in server and client mode.

  

## Level 15→16

> [!info] OpenSSL Cookbook 3rd Edition  
>  
> [https://www.feistyduck.com/library/openssl-cookbook/online/ch-testing-with-openssl.html](https://www.feistyduck.com/library/openssl-cookbook/online/ch-testing-with-openssl.html)  

In this level we have to provide the password to localhost@30001 with a secure connection

  
In this case I will be using NCAT, which was writen for the NMAP project.  
It features SOCKS4, SOCKS5, HTTP proxies and SSL support.  

![[/Untitled 42 3.png|Untitled 42 3.png]]

Next pass

  

Using the provided commands

`openssl s_client -connect localhost:30001`

![[/Untitled 43 3.png|Untitled 43 3.png]]

## Level 16→17

In this level the credentials for the next level can be retrieved by submitting the password to a port on [**localhost**](http://localhost) in some port in the range of 31000 to 32000

  

In this case with nmap i can scan the ports in the given range, but inthis case I will try to make an scanner by my own.

I will leverage of the following command `echo '' > /dev/null/127.0.0.1/$port` this command returns an exit status 0 if the port status is OPEN

![[/Untitled 44 3.png|Untitled 44 3.png]]

Script to scan ports

```Bash
#!/bin/bash

function ctrl_c(){
 echo -e "\n[!]Saliendo...\n"
 exit 1
}

# CTRL + C
trap ctrl_c INT


for port in $(seq 1 65535); do
	(echo " > /dev/tcp/127.0.0.1/$port) 2>/dev/null && echo -e "\n[*] $port OPEN" &
done; wait
```

In this case I will create a temporary working directory,

![[/Untitled 45 3.png|Untitled 45 3.png]]

An this is the script

![[/Untitled 46 3.png|Untitled 46 3.png]]

And this is my script and correlation with nmap

![[/Untitled 47 3.png|Untitled 47 3.png]]

Done, in the port 31790 when you send the actual pass, the response is a PRIVATE KEY

![[/Untitled 48 3.png|Untitled 48 3.png]]

  

```Bash
-----BEGIN RSA PRIVATE KEY----- MIIEogIBAAKCAQEAVmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ imZzeyGC0gtZPGujUSxiJSWI/oTqexh+CAMTSMLOJf7+BrJ0bArnxd9Y7YT2bRPQ Ja6Lzb558YW3FZ1870R10+rW4LCDCNd2lUVLE/GL2GWyuKNOK5iCd5TbtJzEkQTu DSt2mcNn4rhAL+JFr5604T6z8WWAW18BR6yGrMq7Q/KALHYW30ekePQAZLOVUYbW JGTi65CxbCnzc/w4+mqQyvmzpWtMAZJTzAzQxNbkR2MBGySXDLrjg0LWN6sK7wNX X0YVztz/zbikPjfkU1jHS+9EbVNj+D1XF0JuaQIDAQABAOIBABagpxpM1aoLWfvD KHcj10nqcoBc40E11aFYQwik7xfW+24pRNUDE6SFth0ar69 jp5RlLWD1NhPx3iBl J9n0M80J0VToum43U0S8YXF8WwhXriYGnc1sskbwpXOUDc9uX4+UESZH22P290vd d8wErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9Albs sgTcCXKMQnPw9nC YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52y0Q9q0kwFTEQpjtF4uNtJom+asvlpmS8A vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+Dj HoAXyxcUp1DGL51s0mama +TOWWgECgYEA8JtPxPOGRJ+IQKX262jM3dEIkza8ky5moIwUqYdsx0NXHgRRHORT 8c8hAuRBb2G82s08vUHk/fur850Efc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSKCgYEAypHd HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIVGCSx+X315SiWg0A R57hJglezIiVjv3aGwHwvlZvtszK6zV60XFAu0ECgYAbjo46T4hyP5tJi93V5HDi Ttiek 7xRVxUl+i07rWKGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m90QWCg R8VdwSk8r9FGLS+9aKcV5PI/WEKƖwgXinB30hYimtiG2Cg5JCqIZFHxD6MjEGOiu L8ktHMPvodBwNs SBULpG0QKBgBAplTfC1H0nWiMGOUзKPWYWt006CdTkmJOML8Ni blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfZOKU4ZxEnabvXnvWkU YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM 77pBAOGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSSJLAbxFpdlc1gvtGCWW+9Cq0b dxviW8+TFVEB1104f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9G0tt9JPsX8MBTakzh3 vBgsyi/sN3RqRBCGU40f0oZyfAMT8s1m/uYv5206IgeuZ/ujbjY= ---END RSA PRIVATE KEY-----
```

This is the next pass

  

## Level 17→18

So now i will use the id_rsa given in the previous level.  
  
And this level tell us that there are two files in the /home and the pass is the only line that changed between the two files  

  

Using the id_rsa i cna connect to the next machine

![[/Untitled 49 3.png|Untitled 49 3.png]]

REMEMBER 600 PERMISSION FOR ID_RSA

  

I also catted the password being the user bandit 17

![[/Untitled 50 3.png|Untitled 50 3.png]]

When listing the home directory i founded the files mentioned in the description

![[/Untitled 51 3.png|Untitled 51 3.png]]

The < means that something was eliminated and > means that was concatenated

![[/Untitled 52 3.png|Untitled 52 3.png]]

This is the next pass

  

## Level 18→19

So this next password is stored in the readme file located in /home.

The problem here is that when we want to log in via SSH the connections is SHUT and we have a message that says ByeBye!.

  

This tells me that the .bashrc has been edited.

  

So using SSH commands I ll cat the content of .bashrc

![[/Untitled 53 3.png|Untitled 53 3.png]]

This prints all the .bashrc file, and in the very end I found the following

![[/Untitled 54 3.png|Untitled 54 3.png]]

This is checking if the bash is being used as posix, so using the `bash --posix` I can run this bash

  

> The **Portable Operating System Interface** (**POSIX**; [IPA](https://en.wikipedia.org/wiki/International_Phonetic_Alphabet): [/ˈpɒz.ɪks/](https://en.wikipedia.org/wiki/Help:IPA/English)[[1]](https://en.wikipedia.org/wiki/POSIX\#cite_note-FAQ-1)) is a family of [standards](https://en.wikipedia.org/wiki/Standardization) specified by the [IEEE Computer Society](https://en.wikipedia.org/wiki/IEEE_Computer_Society) for maintaining compatibility between [operating systems](https://en.wikipedia.org/wiki/Operating_system).[[1]](https://en.wikipedia.org/wiki/POSIX#cite_note-FAQ-1) POSIX defines both the system and user-level [application programming interfaces](https://en.wikipedia.org/wiki/Application_programming_interface) (APIs), along with command line [shells](https://en.wikipedia.org/wiki/Unix_shell) and utility interfaces, for software compatibility (portability) with variants of [Unix](https://en.wikipedia.org/wiki/Unix) and other operating systems.[[1]](https://en.wikipedia.org/wiki/POSIX#cite_note-FAQ-1)[[2]](https://en.wikipedia.org/wiki/POSIX#cite_note-IET-2) POSIX is also a [trademark](https://en.wikipedia.org/wiki/Trademark) of the IEEE.[[1]](https://en.wikipedia.org/wiki/POSIX#cite_note-FAQ-1) POSIX is intended to be used by both application and system developers.[[3]](https://en.wikipedia.org/wiki/POSIX#cite_note-3)

  

And yes, it does works, as seen the password is located in the readme file

![[/Untitled 55 3.png|Untitled 55 3.png]]

Next pass

  

## Level 19→20

In this level we can connect properly, but only way to see bandit20’s password is to cat the content in /etc/bandit_pass/bandit20, and the owner user is bandit20.  
  
In the /home we have a binary that SUID permissions, let’s leverage from it.  

  

![[/Untitled 56 3.png|Untitled 56 3.png]]

> `**ruid**`: This is the **real user ID** of the user that started the process.
> 
> `**euid**`: This is the **effective user ID**, is what the system looks to when deciding **what privileges the process should have**. In most cases, the `euid` will be the same as the `ruid`, but a SetUID binary is an example of a case where they differ. When a **SetUID** binary starts, the `**euid**` **is set to the owner of the file**, which allows these binaries to function.
> 
> `suid`: This is the **saved user ID,** it's used when a privileged process (most cases running as root) needs to **drop privileges** to do some behavior, but needs to then **come back** to the privileged state.

![[/Untitled 57 3.png|Untitled 57 3.png]]

As seen the EUID is bandit20, so lets try to cat the conten in /etc/bandit_pass

![[/Untitled 58 3.png|Untitled 58 3.png]]

And there it is, the next password.

  

## Level 20→21

In this level we have to use a SUID binary to but this binary makes a conection and pass the password if it is correct I returns the next pass

  

Okay so what the binary does is that it connects to [localhost](http://localhost) and the specified port, and this is all done by bandit21 user, so I leverage from this.

  
Using the previous password i reconected to lvl19lvl20 and set a netcat listen port  

`nc -nvlp 8080`

Using this I then left the port listening for connections

![[/Untitled 59 3.png|Untitled 59 3.png]]

---

Then in the bandit20 user I used the SUID binary and used the same 8080 port

![[/Untitled 60 3.png|Untitled 60 3.png]]

---

And received the connection in the nc on the bandit 19 user

![[/Untitled 61 3.png|Untitled 61 3.png]]

And as seen I then passed the bandit20’s password, and the SUID binary sent the response with the next password.

![[/Untitled 62 3.png|Untitled 62 3.png]]

  

## Level 21→22

> [!info] Crontab.guru - The cron schedule expression editor  
> An easy to use editor for crontab schedules.  
> [https://crontab.guru/](https://crontab.guru/)  

In this level we begin to use cron jobs, so let’s see in the /etc/cron.d to see what is being executed.

  

![[/Untitled 63 3.png|Untitled 63 3.png]]

So there is a command being executed as bandit22, let’s see what is that script doing

![[/Untitled 64 3.png|Untitled 64 3.png]]

The script given 644 permissions to file located in /tmp and then writes the pass from bandit22 in that same file, so lets read what the pass is

![[/Untitled 65 3.png|Untitled 65 3.png]]

Theres is the password

  

> Cron is a Linux task manager that allows you to run commands at a specific time, for example, every minute, day, week or month. If we want to work with cron, we can do it through the crontab command.
> 
> The cron configuration format is as follows: Minute Hour Day-of-the-Month Month Day-of-the-Week Command-To-Execute.
> 
> The time interval is specified by 5 fields representing, from left to right:
> 
> Minutes: from 0 to 59.  
> Hours: 0 to 23.  
> Day of the month: from 1 to 31.  
> Month: from 1 to 12.  
> Day of the week: from 1 to 6 Monday to Saturday (1=Monday, 2=Tuesday, etc.) and 0 or 7 on Sunday.  
> If we would like to specify all possible values of a parameter (minutes, hours, etc.) we can use the asterisk (*). This implies that if instead of a number we use an asterisk, the indicated command will be executed every minute, hour, day of the month, month or day of the week, as in the following example:  
> 
>   
> 
> * * * * * /home/user/script.sh

  

## Level 22→23

> [!info] Cron & crontab, explicados  
> Lucain publicó hace ya un tiempo un excelente tutorial sobre cron y crontab que me parece vale la pena compartir.  
> [https://blog.desdelinux.net/cron-crontab-explicados/](https://blog.desdelinux.net/cron-crontab-explicados/)  

We have another cron job doing its thing, so in this case let’s check again what is executing.

  

![[/Untitled 66 3.png|Untitled 66 3.png]]

  

And again, we have a script being executed every minute

  

![[/Untitled 67 3.png|Untitled 67 3.png]]

Okay, the scripts uses the name from whom is executing the script (bandit23) and then pipes it to md5sum and cut a part, with that output creates a directory where it stores the password for the next user

  

Using the same commands and the username, i got the filename

![[/Untitled 68 3.png|Untitled 68 3.png]]

And the pass is

![[/Untitled 69 3.png|Untitled 69 3.png]]

  

## Level 23→24

Againg, in this case the description says I will have to create a script

![[/Untitled 70 3.png|Untitled 70 3.png]]

Let’s see what is going on

![[/Untitled 71 3.png|Untitled 71 3.png]]

Again, a script..

![[/Untitled 72 3.png|Untitled 72 3.png]]

In this script, another scripts are being loaded from the /var/spool/bandit24/foo

  

Now in the /foo directory I have write permissions, so I can deposit things there. These things will be excuted and then removed

  

Let’s create a temp directory and store the name so we can use it

![[/Untitled 73 3.png|Untitled 73 3.png]]

In this case i stored the path in a variable so i dont have to remember it.

Then i created the following script

![[/Untitled 74 3.png|Untitled 74 3.png]]

And given 777 permission to the script and also the tmp directory so the script can write

So then I copy the script to the given path and give exec permissions to it, in case they were not dragged

![[/Untitled 75 3.png|Untitled 75 3.png]]

  

Then using `watch -n 1 ls -l` I keep looking it worked

![[/Untitled 76 3.png|Untitled 76 3.png]]

![[/Untitled 77 3.png|Untitled 77 3.png]]

Done!

  

## Level 24→25

In this level there is a daemon listening on port 30002 and returns the password for bandit25 if bandit24’s pass and a 4-digit pin code is given

  

![[/Untitled 78 3.png|Untitled 78 3.png]]

Using nc I know that I have to provide the password and a pin separated by a space, so I can make a bruteforcing script

  

There is no need to first connect using netcat and then passing the string, we use piping so we pass the otput of echo and pipe it to nc

![[/Untitled 79 3.png|Untitled 79 3.png]]

So in this case ill use a for loop and create a file containing all the combinations of pass pin

![[/Untitled 80 3.png|Untitled 80 3.png]]

![[/Untitled 81 3.png|Untitled 81 3.png]]

Using GREP Ill try strip all the words that i dont need

![[/Untitled 82 3.png|Untitled 82 3.png]]

  

In this case this wasnt working, maybe it was a resorces problem, so what i did is that I splitted the combinations in two files, from 0000..4999 and from 5000..9999. In this case i know that the pin is in the second interval

![[/Untitled 83 3.png|Untitled 83 3.png]]

As an exercise I will try and found the pin TO-DO

  

## Level 25→26

In this case th level description says that bandit26 is not using a bash, we have brake out of it

![[/Untitled 84 3.png|Untitled 84 3.png]]

  

While trying to connect the connection is closed

![[/Untitled 85 3.png|Untitled 85 3.png]]

The description tells us that bandit 26 is not using a bash, so lets see what is using

![[/Untitled 86 3.png|Untitled 86 3.png]]

When connecting and passing commands these are not interpreted, so I can think that is no an command interpreter. So i will check what is running

![[/Untitled 87 3.png|Untitled 87 3.png]]

So Ill see what is inside the path

  

This is what the file have inside

![[/Untitled 88 3.png|Untitled 88 3.png]]

This script is changing TERM and then executing MORE command with text.txt file

> `more` is a shell command that allows the display of files in an interactive mode. Specifically, this interactive mode only works when the content of the file is too large to fully be displayed in the terminal window. One command that is allowed in the interactive mode is `v`. This command will open the file in the editor ‘vim’.

  

So basically I have to make the terminal window shorter so the more command actually works.

Doing this it will execute MORE and the text.txt file, then perssing v i can enter to the interactive mode as a VIM editor.

![[/Untitled 89 3.png|Untitled 89 3.png]]

And here i can make a variable that its value is /bin/bash

![[/Untitled 90 3.png|Untitled 90 3.png]]

![[/Untitled 91 3.png|Untitled 91 3.png]]

  

## Level 26→27

No descripttion in this level, just hurry up

  

![[/Untitled 92 3.png|Untitled 92 3.png]]

So we have a SUID binary

  

So I can just cat the pass from bandit27

![[/Untitled 93 3.png|Untitled 93 3.png]]

  

## Level 27→28

In this case there is a git repo and the password is in there

So we have to use git clone and the given path

  

![[/Untitled 94 3.png|Untitled 94 3.png]]

The pass is the same as for the user bandit27

![[/Untitled 95 3.png|Untitled 95 3.png]]

  

## Level 28→29

This level has another repo

![[/Untitled 96 3.png|Untitled 96 3.png]]

In this case there is a [READM](http://READM.md)E.md, this is a Markdown, a structiuring language.

![[/Untitled 97 3.png|Untitled 97 3.png]]

This makes me think that the pass hidden in the last commit, but lets check the previous ones

![[/Untitled 98 3.png|Untitled 98 3.png]]

As we can see in the last commit they fixed some infoleak issues

![[/Untitled 99 3.png|Untitled 99 3.png]]

Done, changing to a previous commit, i can see the password  
USed COMMITS  

  

## Level 29→30

Same thing, a repo

![[/Untitled 100 3.png|Untitled 100 3.png]]

Nothing in this commit

![[/Untitled 101 3.png|Untitled 101 3.png]]

Maybe i can use branches here

![[/Untitled 102 3.png|Untitled 102 3.png]]

So the [README.md](http://README.md) says that no passwords in producction, but whats up with dev?

Adn the i list the commits

![[/Untitled 103 3.png|Untitled 103 3.png]]

![[/Untitled 104 3.png|Untitled 104 3.png]]

Done, password found

USED BRANCHES

  

## Level 30→31

We have another repo here

![[/Untitled 105 3.png|Untitled 105 3.png]]

In this case we only have this

![[/Untitled 106 3.png|Untitled 106 3.png]]

And there is two branches with names master and HEAD

![[/Untitled 107 3.png|Untitled 107 3.png]]

In this case maybe I can use tags

> In Git, a tag basically serves as a signed branch that does not change, i.e., it always remains unchanged. It is simply an arbitrary string that points to a specific Commit. You can say that a tag is a name that you can use to mark a specific point in the history of a repository.

![[/Untitled 108 3.png|Untitled 108 3.png]]

So checking this is the password for bandit31

USED TAGS

  

## Level 31→32

Another repo

![[/Untitled 109 3.png|Untitled 109 3.png]]

This time I have to push content to a repo

![[/Untitled 110 3.png|Untitled 110 3.png]]

In this case i cant push the file

![[/Untitled 111 3.png|Untitled 111 3.png]]

So ill try to force

![[/Untitled 112 3.png|Untitled 112 3.png]]

It was added to local repo, so now i have to create a commit

![[/Untitled 113 3.png|Untitled 113 3.png]]

Done, th password was given

USED PUSH COMMITS AND ADD

  

## Level 32→33

This level is another escape

After login in this is what we have

![[/Untitled 114 3.png|Untitled 114 3.png]]

So, let’s see what is a UPPERCASE SHELL

This shell basicaly converts everythin to UPPERCASE

![[/Untitled 115 3.png|Untitled 115 3.png]]

Looking for information about the user i got

![[/Untitled 116 3.png|Untitled 116 3.png]]

---

Okay, for this one i have to use help.

In this case one way to do it, is to play with arguments

> In Bash you can use arguments from the command line, which are sent to the scripts as variables. These would be represented as follows:
> 
> [$0]: Represents the name of the script that was invoked from the terminal.
> 
> [$1]: Is the first argument from the command line.
> 
> [$2]: Is the second argument from the command line and so on.
> 
> [$#]: Contains the number of arguments that are received from the command line.
> 
> [$*]: Contains all the arguments that are received from the command line, stored all in the same variable.

So, basicaly using this arguments and the Linux varaibles i can break out of the UPEPRCASE shell

![[/Untitled 117 3.png|Untitled 117 3.png]]

  

## Level 33→34

Currently there is no more levels

![[/Untitled 118 3.png|Untitled 118 3.png]]