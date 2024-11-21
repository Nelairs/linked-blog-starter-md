This room covers an incident handling with Splunk. An incident from a security perspective is “Any event or action, that has a negative consequence on the security of a user/computer of an organization”. Below are few of the events that would negatively affect the enviroment when they ocurred:

  

- Crashing the system
- Execution of an unwanted program
- Access to sensitive infomation from an unauthorized user
- A website being defaced by the attacker
- The use of USB devices when there is a restriction in usage is against the company’s policy

  

### Learning Objective

- Learn how to leverage OSINT sites during an investigation
- How to map Attacker’s activitiesto Cyber Kill Chain Phases
- How to utilize effective Splunk searches to investigate logs
- Understand the importance of host-centric and network-centric log sources

  

### Room Prerequisites

Before going through this room, it is expected that the participants will have a basic understanding of Splunk.

- Splunk overview and basic navigation
- Important Splunk Queries
- Know how to use different functions/values to craft a search query
- How to look for interesting fields

---

## **Incident Handling Life Cycle**

As an Incident Handler / SOC Analyst, we would aim to know the attackers' tactics, techniques, and procedures. Then we can stop/defend/prevent against the attack in a better way. The Incident Handling process is divided into four different phases. Let's briefly go through each phase before jumping into the incident, which we will be going through in this exercise.

### **1. Preparation**

The preparation phase covers the readiness of an organization against an attack. That means documenting the requirements, defining the policies, incorporating the security controls to monitor like EDR / SIEM / IDS / IPS, etc. It also includes hiring/training the staff.

### **2. Detection and Analysis**

The detection phase covers everything related to detecting an incident and the analysis process of the incident. This phase covers getting alerts from the security controls like SIEM/EDR investigating the alert to find the root cause. This phase also covers hunting for the unknown threat within the organization.

### **3. Containment, Eradication, and Recovery**

This phase covers the actions needed to prevent the incident from spreading and securing the network. It involves steps taken to avoid an attack from spreading into the network, isolating the infected host, clearing the network from the infection traces, and gaining control back from the attack.

### 4. Post-Incident Activity / Lessons Learnt

This phase includes identifying the loopholes in the organization's security posture, which led to an intrusion, and improving so that the attack does not happen next time. The steps involve identifying weaknesses that led to the attack, adding detection rules so that similar breach does not happen again, and most importantly, training the staff if required.

---

## Incident Handling: Scenario

In this exercise, we will investigate a cyber attarck in which the attacker defaced an organization’s website.

This organization has Splunk as a SIEM solution setup. Our task as a Security Analyst would be to investigate this cyber attack and map the attacker’s activities into all 7 of the Cyber Kill Chain Phases.

It is important to note that we dont need to follow the sequence of the cyber kill chain during the investigation. One finding in one phase will lead to another finding that may have mapped into some other phase.

### Cyber Kill Chain

We will follow the Cyber kill Chain Model and map the attacker's activity in each phase during this Investigation.

When required, we will also utilize Open Source Intelligence (OSINT) and other findings to fill the gaps in the kill chain. It is not necessary to follow this sequence of the phases while investigating.

  

![[/Untitled 547.png|Untitled 547.png]]

- Reconnaissance
- Weaponization
- Delivery
- Exploitation
- Installation
- Command & Control
- Actions on Objectives

---

### THM Scenario

A Big corporate organization **Wayne Enterprises** has recently faced a cyber-attack where the attackers broke into their network, found their way to their web server, and have successfully defaced their website **http://www.imreallynotbatman.com**. Their website is now showing the trademark of the attackers with the message **YOUR SITE HAS BEEN DEFACED** as shown below.

![[/Untitled 1 13.png|Untitled 1 13.png]]

They have requested "**US**" to join them as a **Security Analyst** and help them investigate this cyber attack and find the root cause and all the attackers' activities within their network.

  

The good thing is, that they have Splunk already in place, so we have got all the event logs related to the attacker's activities captured. We need to explore the records and find how the attack got into their network and what actions they performed.

This Investigation comes under the `Detection and Analysis phase.`

  

### Splunk

During our investigation, we will be using `Splunk` as our SIEM solution. Logs are being ingested from webserver/firewall/Suricata/Sysmon etc. In the data summary tab, we can explore the log sources showing visibility into both network-centric and host-centric activities. To get the complete picture of the hosts and log sources being monitored in Wayne Enterprise, please click on the **Data summary** and navigate the available tabs to get the information.

[![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/dc5ec747ef4b0f3eada2aac0bb1fa0ed.gif)](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/dc5ec747ef4b0f3eada2aac0bb1fa0ed.gif)

**Interesting log Sources**

Some of the interesting log sources that will help us in our Investigation are:

|   |   |
|---|---|
|**Log Sources**|**Details**|
|**wineventlog**|It contains Windows Event logs|
|**winRegistry**|It contains the logs related to registry creation / modification / deletion etc.|
|**XmlWinEventLog**|It contains the sysmon event logs. It is a very important log source from an investigation point of view.|
|**fortigate_utm**|It contains Fortinet Firewall logs|
|**iis**|It contains IIS web server logs|
|**Nessus:scan**|It contains the results from the Nessus vulnerability scanner.|
|**Suricata**|It contains the details of the alerts from the Suricata IDS.   This log source shows which alert was triggered and what caused the alert to get triggered— a very important log source for the Investigation.|
|**stream:http**|It contains the network flow related to http traffic.|
|**stream:** **DNS**|It contains the network flow related to DNS traffic.|
|**stream:icmp**|It contains the network flow related to icmp traffic.|

---

### Reconnaissance Phase

Reconnaissance is an attempt to discover and collect information about a target. It could be knowledge about the system in use, the web app, employees or location, etc.

  

We will start our analysis by examining any reconnaissance attempt against the webserver `imreallynotbatman.com`. From an analyst perspective, where do we first need to look? If we look at the available log sources, we will find some log sources covering the network traffic, which means all the inbound communication towards our web server will be logged into the log source that contains the web traffic. Let's start by searching for the domain in the search head and see which log source includes the traces of our domain.

![[/Untitled 2 11.png|Untitled 2 11.png]]

Here we have searched for the term `imreallynotbatman.com` in the index `botsv1`. In the sourcetype field, we saw that the following log sources contain the traces of this search term.

- Suricata
- stream:http
- fortigate_utm
- iis

  

From the name of these log sources, it is clear what each log source may contain. Every analyst may have a different approach to investigating a scenario. Our first task is to identify the IP address attempting to perform reconnaissance activity on our web server. It would be obvious to look at the web traffic coming into the network. We can start looking into any of the logs mentioned above sources.

Let us begin looking at the log source **stream:http**, which contains the http traffic logs, and examine the `src_ip` field from the left panel. **Src_ip** field contains the source IP address it finds in the logs.

**Search Query:**

```Plain
index=botsv1 imreallynotbatman.com sourcetype=stream:http
```

![[/Untitled 3 11.png|Untitled 3 11.png]]

So far, we have found two IPs in the src_ip field `40.80.148.42` and `23.22.63.114`. The first IP seems to contain a high percentage of the logs as compared to the other IP, which could be the answer. If you want to confirm further, click on each IP one by one, it will be added into the search query, and look at the logs, and you will find the answer.

[![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/6e4de3d85d3322f76b20c71ea020d0b4.gif)](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/6e4de3d85d3322f76b20c71ea020d0b4.gif)

### Validate that the IP is scanning

So what do we need to do to validate the scanning attempt? Simple, dig further into the weblogs. Let us narrow down the result, look into the `suricata` logs, and see if any rule is triggered on this communication.

![[/Untitled 4 11.png|Untitled 4 11.png]]

### Exploitation Phase