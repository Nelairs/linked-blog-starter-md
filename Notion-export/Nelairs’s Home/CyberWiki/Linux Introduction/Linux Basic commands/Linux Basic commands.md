First of all, when you are using a terminal you are someone, so to know that you use

==`~$ whoami`==

==`n3l4irs`==

  

With the command id you can see to which group you belong

==`~$ id`==

==`uid=1000(n3l4irs) gid=1000(n3l4irs) groups=1000(n3l4irs),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),108(netdev),998(docker)`==

  

This is usefull in pentesting because there are some groups that can be risky leading to a potential PrivEsc

For example being on a **docker** or **lxd** there are potential ways to exploit these groups.

  

As we see, being in the **sudoers** group, this lets us elevate privileges to make actions as root, whitout actually using root acount.

==`~$ sudo su`==

==`[sudo] password for n3l4irs: root@your-pc-name:/home/n3l4irs#`==

Now if we use root and use id command

==`~$ id`==

==`uid=0(root) gid=0(root) groups=0(root)`==

  

Now this groups can be seen in the directory ==`/etc/groups`==

If we use **cat** we can print whats in that path

==`~$ cat /etc/group`==

- Answer
    
    ==`root:x:0: daemon:x:1: bin:x:2: sys:x:3: adm:x:4:syslog,n3l4irs tty:x:5: disk:x:6: lp:x:7: mail:x:8: news:x:9: uucp:x:10: man:x:12: proxy:x:13: kmem:x:15: dialout:x:20: fax:x:21: voice:x:22: cdrom:x:24:n3l4irs floppy:x:25:n3l4irs tape:x:26: sudo:x:27:n3l4irs audio:x:29:n3l4irs dip:x:30:n3l4irs www-data:x:33: backup:x:34: operator:x:37: list:x:38: irc:x:39: src:x:40: gnats:x:41: shadow:x:42: utmp:x:43: video:x:44:n3l4irs sasl:x:45: plugdev:x:46:n3l4irs staff:x:50: games:x:60: users:x:100: nogroup:x:65534: systemd-journal:x:101: systemd-timesync:x:102: systemd-network:x:103: systemd-resolve:x:104: input:x:105: crontab:x:106: syslog:x:107: netdev:x:108:n3l4irs mlocate:x:109: ssl-cert:x:110: uuidd:x:111: avahi-autoipd:x:112: bluetooth:x:113: rtkit:x:114: ssh:x:115: pulse:x:116: pulse-access:x:117: saned:x:118: n3l4irs:x:1000: docker:x:998:n3l4irs lxd:x:999:n3l4irs`==
    

## PATH

Using the command

==`~$ echo $PATH`==

==`/home/n3l4irs/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin`==

We can see the here are some directories that are searched when we ‘execute’ a command

For example, when you use `==whoami==` this command is executed from **/usr/bin/whoami** this makes easier the execution of commands.

There is also an exploit called **PATH HIJACKING** in PenTesting this basicaly makes the flow or result of a program works in another way other than the normal

  

Now if you want to make another command after for example a ==`cat /etc/group`== you can use **PIPES** and pass it to a ==`grep`==

==`~$ cat /etc/group | grep "floppy" floppy:x:25:n3l4irs`==

  

When you want to know the absolut path of a command you can use ==`which`==

==`~$ which whoami`==

==`/usr/bin/whoami`==

  

This is useful when PenTesting because we can see if a command exist in the server or endpoint we are breaching

A similar way is using ==`command -v whoami`==

  

This is normal in Linux, you have more than one way to acomplish something

  

==`ls`== is a command to list the content from a directorie

For example,

==`n3l4irs@localhost:~$ ls Desktop Documents Downloads Music Pictures Public Templates Videos`==

Now there are some flags that let you use more functions of a command, like

==`n3l4irs@localhost:~$ ls -l total 28 drwxr-xr-x 2 n3l4irs n3l4irs 4096 May 4 08:30 Desktop drwxr-xr-x 2 n3l4irs n3l4irs 4096 May 4 08:30 Documents drwxr-xr-x 2 n3l4irs n3l4irs 4096 May 4 08:30 Downloads drwxr-xr-x 2 n3l4irs n3l4irs 4096 May 4 08:30 Music drwxr-xr-x 2 n3l4irs n3l4irs 4096 May 4 08:30 Pictures drwxr-xr-x 2 n3l4irs n3l4irs 4096 May 4 08:30 Public drwxr-xr-x 2 n3l4irs n3l4irs 4096 May 4 08:30 Templates drwxr-xr-x 2 n3l4irs n3l4irs 4096 May 4 08:30 Videos`==

  

This leads me to the permissions, which are the letter in the first column of the last command

==`drwxr-xr-x`==

![[/Untitled 548.png|Untitled 548.png]]

As seen, the first dash shows the file type

Now the octects shows the permissions for what they are made, for example, the first octet being for the user (which is the owner of the file) is set for **read and write** permissions.

A good form of setting or remembering the permissions is in **binary** or knowing the value of each digit

  
  

![[/Untitled 1 14.png|Untitled 1 14.png]]