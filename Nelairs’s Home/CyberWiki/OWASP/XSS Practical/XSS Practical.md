  

With the **reflected XSS** tab, I can Inject code for testing.

`alert('Hello')`

![[Untitled 571.png|Untitled 571.png]]

With `alert(windows.location.hostname)` i display the machines IP

![[Untitled 1 36.png|Untitled 1 36.png]]

Now with some Stored XSS i can inject some code, in this case HTML

![[Untitled 2 29.png|Untitled 2 29.png]]

Now for some JS ill try `<script>alert(document.cookies)</script>` to popup the cookies

![[Untitled 3 28.png|Untitled 3 28.png]]

Now ill try to deface the website

ill try `<script>document.querySelector("\#thm-title').textContent = "I am a hacker"</script>` this should change the above button that says **XSS Playground**

![[Untitled 4 28.png|Untitled 4 28.png]]