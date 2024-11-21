# Section 2 - Commands, How Linux works

[[Linux Basic commands]]

[[Flow control operator, BG processes]]

[[File descriptors]]

[[FIRST Q&A]]

[[SUID & SGID]]

[[Capabilities]]

[[System directory estructure]]

[[Update & Upgrade]]

[[TMUX (TO-DO)]]

[[Find Files]]

[[Bash Scripting Intro]]

[[Using and Configuring Kitty (TO-DO)]]

[[Vim editor usage]]

[[Bandit DONE]]

[[CRON]]

# Section 3 - Bash Scripting

### Project: Creating a search engine

What we gonna do?  
In this project we will create a search engine, this search engine will be inspired in  

[https://htbmachines.github.io/](https://htbmachines.github.io/)

This search engine lets us filter by keywords facing the machines resolved in HTB.  
It possible to filter by  
**Name, IP, OS, Certification alike, Dificulty and even specific techniques used.**

  

This would be great to have it in console, so this what this project is going to focus on, we are gonna create a tool that acts as search engine and represents all the data in a structured format so we can get what we want quickly

[[Bash scripting 1-6]]

[[Bash scripting 2-6]]

[[Bash Scripting 3-6]]

[[Bash Scripting 4-6]]

---

### Project: Defying casino’s roulette

![[/Untitled 21.png|Untitled 21.png]]

What we gonna do?

In this project, the objective is to prove how always the casino wons the money. I am going to demonstrate this using different techniques for the roulette.  
  
  
**Martingale Technique****:** A **martingale** is a class of betting strategies that originated from and were popular in 18th-century France. The simplest of these strategies was designed for a game in which the gambler wins the stake if a coin comes up heads and loses if it comes up tails. The strategy had the gambler double the bet after every loss, so that the first win would recover all previous losses plus win a profit equal to the original stake. Thus the strategy is an instantiation of the [St. Petersburg paradox](https://en.wikipedia.org/wiki/St._Petersburg_paradox).

**Reverse Labouchère****:** betting system is a positive progression version (increase bets following wins) of the original version of the [classic Labouchère system](https://www.gsimulator.net/labouchere).

Understanding the mechanisms of the [classic Labouchère method](https://www.gsimulator.net/labouchere) makes it easy to apply the reverse method. Just inverting the rules, baring the bets on the sequence following the losses, and adding the last bet played to the sequence following the winnings.

  

The **Reverse Labouchère betting system** requires great rigor in its application. It remains very simple to understand. Like the other systems with positive progression ([Paroli](https://www.gsimulator.net/reverse-martingale-paroli), [Reverse d’Alembert](https://www.gsimulator.net/contra-dalembert)), the Revrese Labouchère betting system reduces the risk of big losses.

  

So basically Ill try and make a script that apllies this techniques and so how we won a lot of money, right?

---

[[Casino’s Bash Scripting 3-15]]

[[Casino’s Bash Scripting]]