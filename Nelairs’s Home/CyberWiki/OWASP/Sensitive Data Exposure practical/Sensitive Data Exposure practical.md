In this challenge ill put everything i learn about Data Leak

  

**ALWAYS CHECK SOURCE CODE**

So i began on the main page of the webapp, inspecting the code i couldnt find anything suspicious so i advanced to login page

![[Untitled 568.png|Untitled 568.png]]

In the login page, inspecting the code i foun a note from dev, it appears that the DB is stored in the path **/assets**

![[Untitled 1 33.png|Untitled 1 33.png]]

The path is exposed indeed, and there it is, the .db file

![[Untitled 2 26.png|Untitled 2 26.png]]

I go and download the db file

![[Untitled 3 26.png|Untitled 3 26.png]]

Using `sqlite3 webapp.db`

![[Untitled 4 26.png|Untitled 4 26.png]]

Founded two admin acounts and their hashes, using **Crackstation**

![[Untitled 5 18.png|Untitled 5 18.png]]

6eea9b7ef19179a06954edd0f6c05ceb admin

ad0234829205b9033196ba818f7a872b bob

Success, i logon as admin

![[Untitled 6 16.png|Untitled 6 16.png]]

FLAG: THM{Yzc2YjdkMjE5NzVjMzNhOTE3NjdiMjdl}

![[Untitled 7 16.png|Untitled 7 16.png]]