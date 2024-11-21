Walkthroughs by me as I made the machines.

## Starting Point

### Responder - Very Easy

Add the host, to /etc/hosts

SMB Relay

Using responder to see for events

[https://book.hacktricks.xyz/windows-hardening/ntlm/places-to-steal-ntlm-creds](https://book.hacktricks.xyz/windows-hardening/ntlm/places-to-steal-ntlm-creds)

[https://osandamalith.com/2017/03/24/places-of-interest-in-stealing-netntlm-hashes/](https://osandamalith.com/2017/03/24/places-of-interest-in-stealing-netntlm-hashes/)

Once we launch the responder server

![[Untitled 538.png|Untitled 538.png]]

Then we use the LFI on the web application to try and get a resource, change it to RFI

![[Untitled 1 4.png|Untitled 1 4.png]]

![[Untitled 2 4.png|Untitled 2 4.png]]

![[Untitled 3 4.png|Untitled 3 4.png]]

Since Powershell is not installed by default in Linux, I have to use `evil-winrm`

DESPUES DE ARREGLAR MIL COSAS CON OPENSSL

![[Untitled 4 4.png|Untitled 4 4.png]]

  

### Three - Very Easy

We have two open TCP ports, 22a and 80

In the HTTP webpage I found a domain (thetoppers.htb) contact mail, I added it to the /etc/hosts file

Using gobuster i started a further search

  

---