So what is capabilities? Let’s see an example with the Python3.9 binary

![[Untitled 551.png|Untitled 551.png]]

As I can see the binary belongs to user and group **root,** but there is not SUID neither SGID asigned

So if I enter an interactive terminal and try to change my user ID and become root I should not be able to.

![[Untitled 1 17.png|Untitled 1 17.png]]

Well as seen, I became root

![[Untitled 2 14.png|Untitled 2 14.png]]

Why is that? How is possible? Well there is a tool that can help

![[Untitled 3 14.png|Untitled 3 14.png]]

getcap is for listing capabilities of the system

So if we list from the system root directory we have this output  
  

![[Untitled 4 14.png|Untitled 4 14.png]]

  

Here is something interesting, in the python3.9 binary, there is a capability called **cap_seuid=ep** this tells me that is posible to change or set the User ID, so that is why we can become **ROOT.  
  
  
**Now this is not set by default, in this case was set for the PoC

---

Now there is a resource that is well used in Pentesting, this is GTFOBins that list binaries and its vulnerabilities and exploits for Unix systems.

![[Untitled 5 10.png|Untitled 5 10.png]]

On a counterpart there is LOLBAS which is the same but with windows binaries.

![[Untitled 6 9.png|Untitled 6 9.png]]

For example, in Python, if there is a Capabilitie, lets look what we can do

![[Untitled 7 9.png|Untitled 7 9.png]]

Now here, in a one liner we have a PrivEsc

![[Untitled 8 9.png|Untitled 8 9.png]]

This will spawn a shell as root