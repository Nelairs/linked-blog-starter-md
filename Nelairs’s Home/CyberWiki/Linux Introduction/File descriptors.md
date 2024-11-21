Now this is not that commonly used, but is good to know as general linux culture

If we execute the following command

`~$exec 3<> file`

This creates a new descriptor (in this case 3) associated with file named ‘file’ the operator <> opens the file both in read and write without truncating

This means that this works in append mode, so everything that you send to the file, is apended and not overwritten.

  

If I want to write the file I use **>&3** as for pushing the output of a command for example.  
  

`~$ whoami >&3`  
This does not prints anything but if I see whats in the file  

`~$ cat file`

`n3l4irs`

But I can also read the file using the descriptor like

`n3l4irs@localhost:~$ cat <&3 n3l4irs`

  

Now I can close a file descriptor like `exec 3>&-` this closes the file descriptor but this doesnt means that the content of the file is removed but the descriptor cannot be used anymore unless is opened again