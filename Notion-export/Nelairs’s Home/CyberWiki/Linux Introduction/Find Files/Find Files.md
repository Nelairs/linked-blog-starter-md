When you compromise a machine, in some ocassions we see that we are in a certain group and would be interested in perform some searches

![[Untitled 555.png|Untitled 555.png]]

In this example, imagine that i’d like to search a file called ‘passwd’, by default this file is in the path /etc/passwd.

![[Untitled 1 20.png|Untitled 1 20.png]]

Ill redirect all th errors to /dev/null because root user is not used and may not have permissions to enter some directories.

![[Untitled 2 15.png|Untitled 2 15.png]]

In addtition, we can pipe this to the command xargs to list the permissions

![[Untitled 3 15.png|Untitled 3 15.png]]

As seen, the binarie found in /usr/bin/passwd has SUID permissions

How do we find files with SUID permissions? This is a common technique done in PenTesting because dependeing on the binarie, you cna exploit that and Escalate Priviligies

![[Untitled 4 15.png|Untitled 4 15.png]]

Now you can find with different types of parameters

`find / -group n3l4irs 2>/dev/null` this finds all that belongs to the group given in the parameter

`find / -group -type d 2>/dev/null` in this case im filtering by only directories, the f is used for files

  

Now imagine that we want to look for files that have write permissions and belong to the user root

`find / -user root -writable 2>/dev/null`

`find / -user root -executable 2>/dev/null`

---

## How to find with names

`find / -name dex\* 2>/dev/null`