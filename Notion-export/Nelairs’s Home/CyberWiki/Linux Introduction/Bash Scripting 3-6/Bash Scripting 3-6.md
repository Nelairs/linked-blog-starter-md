In this class I am going to define the primary parameter to filter out the machines using the flag -m to search

  

So in this case i have to find by name , the easiest way would be using GREP

![[/Untitled 562.png|Untitled 562.png]]

As seen this is not acurate, so lets try with awk

  

With AWK I can specify a range

![[/Untitled 1 27.png|Untitled 1 27.png]]

With this range that goes from name: to resuelta: I can filter every machine, no matter if there is more or less content.

  

Now using grep i can filter the values i dont want

![[/Untitled 2 21.png|Untitled 2 21.png]]

And using tr I’ll delete the commas and double quotations

![[/Untitled 3 21.png|Untitled 3 21.png]]

So this is the final command and output

  

There was added a sed command at the end so I could delete the spaces at the beggining

![[/Untitled 4 21.png|Untitled 4 21.png]]

Finally, there was added a parameter -i to search by IP address, this search for the machine name and then looks th emachine by the name.