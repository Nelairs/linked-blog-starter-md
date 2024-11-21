The easiest way to see bash is as we are excuting commands from the console, but automatized

  

Let’s get started.

In the /Desktop I’ll create a file called `script.sh`

![[/Untitled 556.png|Untitled 556.png]]

As seen this file has no execution permissions

![[/Untitled 1 21.png|Untitled 1 21.png]]

In this case I assigned to everyone execution permissions.

  

So now, lets create the script perse

`~$ vi script.sh`

```Bash
#!/bin/bash

echo "Hola! Esto es una prueba"
```

This would print whats inside echo command

![[/Untitled 2 16.png|Untitled 2 16.png]]

Now let’s make a practical example.

I want to make a script to report my host IPv4

  

Using `~$ ip a` I can print all my interfaces and its IPs

![[/Untitled 3 16.png|Untitled 3 16.png]]

I know that my physical interface is the one named **‘ens33’**

So I’ll do something like `~$ ip a | grep ens33`

![[/Untitled 4 16.png|Untitled 4 16.png]]

Now the IP is on the second line so to do this there is multiple command to use  
  

> THERE IS ALWAYS ANOTHER WAY IN LINUX

  

So using `~$ ip a | grep ens33 | tail -n 1` or `~$ ip a | grep ens33 | awk 'NR==2'`

![[/Untitled 5 11.png|Untitled 5 11.png]]

Now the IP is on the second argument so i have to extract it, and again theres is multiple ways of doing this

`~$ ip a | grep ens33 | tail -n 1 | awk '{print $2}'` is one of the ways

![[/Untitled 6 10.png|Untitled 6 10.png]]

Now i have to remove the CIDR part of the IP

`~$ ip a | grep ens33 | tail -n 1 | awk '{print $2}'| awk '{print $1}' FS='/'`

`~$ ip a | grep ens33 | tail -n 1 | awk '{print $2}'| cut -d '/' -f 1`

`~$ ip a | grep ens33 | tail -n 1 | awk '{print $2}'| tr '/' ' ' | awk '{print $1}'`

![[/Untitled 7 10.png|Untitled 7 10.png]]

Now we have the IP that I wanted, but doing this commands one by one can be tedious, so for that we make a **SCRIPT**

  

```Bash
#!/bin/bash

echo "[*] Esta es tu direccion IP privada -> $(ip a | grep ens33 | tail -n 1 | awk '{print $2}'| awk '{print $1}' FS='/')"
```

![[/Untitled 8 10.png|Untitled 8 10.png]]

We dont allways need to give execution permissions, giving permission serves to execute the script as if were a binary.  
  

We can always execute the scripts with the command bash first, and no t having execution permissions.

![[/Untitled 9 8.png|Untitled 9 8.png]]