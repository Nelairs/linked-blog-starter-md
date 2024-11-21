Let’s make some examples for CRON and understanding its expresions

> Some resources  
>   
> [https://crontab.guru/](https://crontab.guru/#*_*_*_*_*)  
>   
> [https://www.site24x7.com/es/tools/crontab/cron-generator.html](https://www.site24x7.com/es/tools/crontab/cron-generator.html)

  
  

![[Untitled 559.png|Untitled 559.png]]

So, the basics of CRON is that it executes jobs at a determinated time, this time can be configured where the asterix are. Let’s take a deeper look for the configurations of time.

  

We have 4 common character in all the positions, the are

- * Any value
- , List separator
- - Range separator
- / Pass value

Then every positions has its own parameter for configuration, let’s start with minutes

### Minutes

Besides this 4 common values we have the valid values which are from 0-59 (minutes in hour)

- Examples
    
    The most common one **Every minute**
    
    ![[Untitled 1 24.png|Untitled 1 24.png]]
    
    ---
    
    Then, using only a number we have
    
    ![[Untitled 2 19.png|Untitled 2 19.png]]
    
    This means that every hour at 1 minute this will exec
    
    ---
    
    if we use the slash / we can define minutes intervals
    
    ![[Untitled 3 19.png|Untitled 3 19.png]]
    

---

### Hours

Besides the 4 common values there are also the valid values which are from 0-23 (hours in day)

- Examples
    
    Here we use the hours space, so if we just put a number we got
    
    ![[Untitled 4 19.png|Untitled 4 19.png]]
    
    This because we only specify a number in the hours space
    
    ---
    
    Using the minutes we have a specific hour in the day
    
    ![[Untitled 5 14.png|Untitled 5 14.png]]
    
    ---
    
    Here using slash
    
    ![[Untitled 6 12.png|Untitled 6 12.png]]
    
    ![[Untitled 7 12.png|Untitled 7 12.png]]
    

---

### Days in a month

Here we have 5 more values which are

- L Last day of the month
- W Closer Workable Day
- LW Last Closer Workable Day
- ? No specific value
- 1-31 Permitted values (days in a month)

- Examples

---

### Months

Here we have 2 more values

- 1-12 Permitted values (months in a year)
- JAN-DEC Permitted values (months in a year)

- Examples

---

### Days in a week

Here we have another 5 values

- L Saturday
- # n day
- ? No specific value
- 0-6 Permitted values (days in week)
- SUN-SAT Permitted values (days in week)

- Examples

---