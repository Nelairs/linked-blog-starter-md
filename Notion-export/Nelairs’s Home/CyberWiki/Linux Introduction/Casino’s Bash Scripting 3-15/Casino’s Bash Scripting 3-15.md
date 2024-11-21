  

  

I started by creating the directory for this project and its main script

![[Untitled 564.png|Untitled 564.png]]

  

The first thing to do is the parameters for initializing the script

![[Untitled 1 29.png|Untitled 1 29.png]]

These parameters define the amount of money -m and the technique -t

---

  

Arrays for the labrouchere technique

  

So to declare an array, the best practice is to do it as `declare -a myArray=(n1 n2 n3 n4)`

![[Untitled 2 22.png|Untitled 2 22.png]]

This so we can operate later with this array

  

If I want to show the elements in the array Ill use `${myArray[@]}`

![[Untitled 3 22.png|Untitled 3 22.png]]

While if i want to know how many elements are in the array Ill use `${\#myArray[@]}`

![[Untitled 4 22.png|Untitled 4 22.png]]

Now I can explicitly show an element wit a given position `${myArray[0]}` or`${myArrar[-1]}`

This second -1 is alusive to the last position

![[Untitled 5 15.png|Untitled 5 15.png]]

Now for the Labrouchere, there is a time when we need to add the numbers from the first and last position, so to do that

![[Untitled 6 13.png|Untitled 6 13.png]]

When I win i need to add the reward to last position, so `myArray+=<number>`

![[Untitled 7 13.png|Untitled 7 13.png]]

To remove elements we use `unset` in the case of Labrouchere we need to strip the first and last element.

![[Untitled 8 12.png|Untitled 8 12.png]]

Using `unset` generates problems after in the positions, to fix this we need to “format” the array like `myArray=(${myArray[@]})`

![[Untitled 9 10.png|Untitled 9 10.png]]