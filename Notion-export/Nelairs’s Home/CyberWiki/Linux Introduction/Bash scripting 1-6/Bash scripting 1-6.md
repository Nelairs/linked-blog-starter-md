So, let’s get started.

![[Untitled 560.png|Untitled 560.png]]

I will start by creating the base htbmachines directory in /opt

In this case for efficiency, i downloaded the data from the page

![[Untitled 1 25.png|Untitled 1 25.png]]

Using `pip install jsbeautifier` i got js-beatify so i can get a more clear vision of the js code

  

I’ll begin by creating the options or flags with a `while getopts`

To code the a flag I just have to put the letter and using colon (:) I tell the program if it needs arguments or not.

![[Untitled 2 20.png|Untitled 2 20.png]]

This is the base program, where it iterates between the two parameters, machineName and helpPanel.

![[Untitled 3 20.png|Untitled 3 20.png]]

The variable is used as a flag to see which parameter was used inside the code.

![[Untitled 4 20.png|Untitled 4 20.png]]