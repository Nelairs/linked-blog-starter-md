  

## OPERATORS AND FLUX REDIRECT

  

If i want to execute two commands in a line (also called **one liner**) we can concatenate with a SEMICOLON ;

==`n3l4irs@localhost:~$ whoami ; ls n3l4irs Desktop Documents Downloads Music Pictures Public Templates Videos`==

  

As we can see the commands execute one after the other without looking for the state of the first one like using ampersands

==`n3l4irs@localhost:~$ whoami && ls n3l4irs Desktop Documents Downloads Music Pictures Public Templates Videos`==

  

At first looks the same, but it actually waits for the first command to send an succesful status code

  

==`n3l4irs@localhost:~$ whoam && ls command not found: whoam`==

  

As we see the second command is not executed, because the first one gives a err status code

  

Now, to see the status code I can use

==`~$echo $?`==

0 for a success status

or it could be

127 or 1 for error code

  

Now we can use the OR operand as | | this executes the second command if the first one fails

==`n3l4irs@localhost:~$ whoam && ls Desktop Documents Downloads Music Pictures Public Templates Videos`==

This executes the LS because the whoami was mispelled

  

Now if we are making a BASH SCRIPT maybe we dont want to see all the errors so we can redirect it.

The std_err is referenced as 2

So if we make ==`~$whoam 2>/dev/null`== the errors will be redirected to /dev/null directorie, this directorie you can see it as a _black hole_ where everithing gets deleted or faded

  

If we execute a command with ==`cat /etc/hosts > /dev/null`== we are executing the **cat** command but the output is redirected to /dev/null so we dont see nothing

  

Now if i execute ==`cat /etc/hotss > /dev/null`== (see that i made a typo) the output would be an error message, because the std_out is th only redirected

Now we can fix this be _transforming_ the std_err to std_out —> ==`cat /etc/hotss > /dev/null 2>&1`==

Now this is a nasty way of doing it, a better way would be ==`cat /etc/hotss &>/dev/null`==

  

When you execute a program or a command like for example wireshark, you are executing like ==`wireshark`== and this will open a window or a gui for the program

Now we can execute it like ==`wireshark &>/dev/null`== so we hide the std_err and std_out of the program (in this case wireshark). This opens the software and hides the outputs

Another thing to have in mind is that when you open a software the terminal is the parent proccess of the child proccess wireshark, this force the terminal to stay opened so the software stays open

Now we can background the proccess to keep using the terminal

==`~$wireshark &>/dev/null &`== and this will print the proccess number as ==`[1] 30698`== , now this dont make the terminal completely free, because now we can use it, but we cant close it. To do this we can _disown_ the terminal like

==`~$wireshark &>/dev/null & disown`==