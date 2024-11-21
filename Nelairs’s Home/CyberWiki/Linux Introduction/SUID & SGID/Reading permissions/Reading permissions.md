So if I create a file in (for example) `~/Desktop/` named **file.txt**

`~$ touch file.txt`

`n3l4irs@localhost:~/Desktop$ ls file.txt`

  
If i  
**list** the directory i see that there it is, but what if I **list** a directory with -l flag?  
  

n3l4irs@localhost:~/Desktop$ ls -l  
  
==-rw-r--r-- 1== ==n3l4irs n3l4irs== ==0== ==May 16 10:30== ==file.txt==

  
As seen, now we have more information like the  
==name of the file,== ==the creation date,== ==the size of the file,== ==the propietary and group to whom belongs== and ==last the permissions.==

What is important for us in this case is the permissions, so lets talk about them.  
  
The permissions of a file are denoted by  

==-====rw-====r--====r--==

The ==first letter== that indicates the type of file, which could be - dash(any file created by any program), Directory(mkdir), l Symbolic Link, s Socket(nc -U), etc.

The ==first threes== which indicates the permissions for the **USER** that owns the file.

The ==second threes== which indicates the permissions for the **GROUP** that owns the file.

The ==third threes== which indicates the permissions for the **OTHERS** that dont belong to group.

  

Now th permissions are readed like, **READ, WRITE, EXECUTION.** And to manage them we can use `chmod` and two forms of representation, one in arythmetic mode and another in number mode.

  

Arythmetic, is using the letter and “add” or “substract” permissions as  
  
`~$ chmod g+rwx` o `~$ chmod g-rwx` in the first case I add (+) permisisons and in the second i substract (-) permission to group that owns the file.  
The permissions can be concatenated with commas like  
`~$ chmod u+rwx,g+rx,o-rwx`

  

There is also the a which is used to give same permissions to all `~$ chmod a+rw`

And equal operator that is used to delete previous permissions and give the new ones

`~$ chmod o=r`

---

### Adding users

  

Now if we create a new user like this

`~$ mkdir /home/pepe`

`~$ useradd pepe -s /bin/bash -d /home/pepe`

  

I’ll see something like

`~$ cat /etc/passwd | grep pepe`

![[Untitled 573.png|Untitled 573.png]]

`~$ ll /home/`

![[Untitled 1 37.png|Untitled 1 37.png]]

Now the directory does not belong to the user **pepe,** to do this I’ll use **chown**

`~$ chown pepe:pepe pepe/`

![[Untitled 2 30.png|Untitled 2 30.png]]

As seen, now the directory

---

  

### Sticky Bit

  

This is a common missconfiguration because sometimes we assing permissions to a file or directories without paying attention to the ACTUAL directorie we are working

  

If we create a directory, assing it to the user and group ‘pepe’ and make the permissions 777

![[Untitled 3 29.png|Untitled 3 29.png]]

Now I have a directory called ‘pruebaspepito’ that belongs to user ‘pepe’ and to the group ‘pepe’

The permissions in this directory are RWX for everyone (never do this).

  

Now if we to user ‘pepe’ and enter the directory ‘pruebaspepito’

Ill create a file with some text inside

![[Untitled 4 29.png|Untitled 4 29.png]]

Now the file has other permissions, the group and others can only read the file.

So if become OTHER user and enter this directory (because others have permission to navigate the directory) can i modify the ‘file.txt’ file? I should not have permissions, right?

![[Untitled 5 19.png|Untitled 5 19.png]]

Right, now ill try to delete this file, nothing should happen because i cant modify this ‘file.txt’

![[Untitled 6 17.png|Untitled 6 17.png]]

Oops, Its gone, why its that? Well that its because in the ‘pruebaspepito’ directory OTHERS have all the permissions. So we have to pay attention to the actual container folder

  

Now how do we prevent this?

![[Untitled 7 17.png|Untitled 7 17.png]]

This uses the sticky bit, so the permissions doesnt override the ones inside the container folder

![[Untitled 8 15.png|Untitled 8 15.png]]

  

### LSATTR y CHATTR

---

I’ll make a copy of the **/etc/hosts** to the actual directory.

Now using **LSATTR** I can list the attributes of the file beyond the traditional file permissions and ownership.

As seen the attributes show a C that stands for compression.

![[Untitled 9 13.png|Untitled 9 13.png]]

  
Now with the command  
**CHATTR** i’ll add the immutable attribute (i)

![[Untitled 10 10.png|Untitled 10 10.png]]

This stands that the file cannot be modified, renamed, or deleted.

> [!important]  
> Some of the common attributes that can be displayed by lsattr include:i (immutable): The file cannot be modified, renamed, or deleted.a (append-only): Data can only be appended to the file; existing data cannot be modified.d (no dump): The file or directory will be excluded from system backup programs.s (secure deletion): When a file is deleted, its contents are overwritten with zeroes.c (compressed): The file is compressed on disk.