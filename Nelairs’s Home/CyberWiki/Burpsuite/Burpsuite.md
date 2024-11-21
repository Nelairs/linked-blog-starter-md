- Exclude domains
    
    > [!info] Burpsuite: just passthrough firefox detect portal  
    > When I enable Burpsuite's Proxy I continiously get http GET requests for firefox's detectportal as seen in the following image:  
    > [https://security.stackexchange.com/questions/187069/burpsuite-just-passthrough-firefox-detect-portal](https://security.stackexchange.com/questions/187069/burpsuite-just-passthrough-firefox-detect-portal)  
    
    > [!info]  
    >  
    > [https://github.com/leobloonz/burpsuite-exclude-scope-list/blob/main/burp_exclude_scope.txt](https://github.com/leobloonz/burpsuite-exclude-scope-list/blob/main/burp_exclude_scope.txt)  
    
    ---
    
    Abrir Burp rapido y disown  
      
    
    ![[Untitled 545.png|Untitled 545.png]]
    
    ---
    
      
    
    > Target --> Scope --> Use advanced scope control --> exclude from scope --> Load ...
    
    > Project Options --> Connection --> Out-OF-Scope-Request --> Drop all out-of-scope request
    

FoxyProxy Extension Configuration

Install foxyproxy extension

Click on the extension and then options

![[Untitled 1 11.png|Untitled 1 11.png]]

Click Add

![[Untitled 2 9.png|Untitled 2 9.png]]

Click on the "Add" button and fill in the following values:

- Title: `Burp` (or anything else you prefer)
- Proxy IP: `127.0.0.1`
- Port: `8080`

![[Untitled 3 9.png|Untitled 3 9.png]]

Now click "Save"

---

When you click on the FoxyProxy icon at the top of the screen, you will see that that there is a configuration available for Burp:

![[Untitled 4 9.png|Untitled 4 9.png]]