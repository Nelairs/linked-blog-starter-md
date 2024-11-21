> [!info] Vim Cheat Sheet  
>  
> [https://vim.rtorr.com/](https://vim.rtorr.com/)  

So vim its a text editor, i has multiple funcitions and at first can difficult to use but when you learn the shortcuts and how to move easily its a quick editor

  

Now for example, when you open a new file `vim test.txt` and try to write you will see that its not allowed.

  

For start typing you need to press I as **Insert  
  
**

![[Untitled 557.png|Untitled 557.png]]

And now you can begin typing

![[Untitled 1 22.png|Untitled 1 22.png]]

To UNDO something as Ctrl + Z would do, you have to use Alt + U

To move you can use ARROW KEYS or H J K L Keys, Using HOME and END you move the start or the end of the line respectively

To move word by word use W and using a number then press W you n words

To delete a word you position on the start and press D then W also you can use a number prevuous to this to delete as many words as you like

To delete a line use D then D

  

In VISUAL mode pressing V, you can select a string and pressing Y you copy the selection, and to PASTE you P

  

### MACROS

Now the macros are created to automatize some task, so in this case lets make an example deleting the users in /etc/passwd file

![[Untitled 2 17.png|Untitled 2 17.png]]

To delete you’ll use ‘d’ then ‘j’ to go down one line and ‘dot’ to repeat the action delete

But in this case let’s a MACRO

To record a macro youll press ‘q’ then (for example) ‘a’ and youll start recording a macro

![[Untitled 3 17.png|Untitled 3 17.png]]

Now every command you make will be recorded, so lets record the command to delete and go down one line

This would be like, start recording macro ‘q’ → ‘a’ then record the strokes ‘d’ → ‘w’ → ‘j’ and stop recording ‘q’

Now to execute it use `31 @ a` to execute it 31 times

![[Untitled 4 17.png|Untitled 4 17.png]]

![[Untitled 5 12.png|Untitled 5 12.png]]