For this example I’ll use the python binary

![[Untitled 550.png|Untitled 550.png]]

Now if I want to make a ls -l without writing the absolute path

I can use the concatenation of commands and using **XARGS**

![[Untitled 1 16.png|Untitled 1 16.png]]

In this case the binary has the permissions **755**

Now I’ll asing the suid permission

![[Untitled 2 13.png|Untitled 2 13.png]]

It can be asigned in that way or in numeric way like **4**755 being **4** the SUID permission

  

Now what is the risk here?

  

Let’s supose that im an atacker that is navigating a compromised system

I’ll use **find** command

![[Untitled 3 13.png|Untitled 3 13.png]]

I see that the python binary has SUID permissions

So what we have to have in mind here is that the binary belongs to root and having SUID asigned, this would let execute the binary temporarily as priviliged user being a non-priviliged user

  

![[Untitled 4 13.png|Untitled 4 13.png]]

Now we have a bash as Root

  

SGID its pretty much the same

  

[[Reading permissions]]