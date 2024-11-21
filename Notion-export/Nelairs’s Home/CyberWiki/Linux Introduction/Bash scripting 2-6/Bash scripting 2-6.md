In this next class i added some new things, the most important one is a function for checking if there is updates in the data base

![[Untitled 561.png|Untitled 561.png]]

So what it basically does, is that download a new file with curl, and using md5sum i compare the hashes from the two files.

> Hashing is **the process of transforming any given key or a string of characters into another value**. This is usually represented by a shorter, fixed-length value or key that represents and makes it easier to find or employ the original string.

  

This also made me add the option (flag) -u in the getopts menu

![[Untitled 1 26.png|Untitled 1 26.png]]