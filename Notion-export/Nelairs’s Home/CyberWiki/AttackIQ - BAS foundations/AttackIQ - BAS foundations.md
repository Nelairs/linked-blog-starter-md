> [!info]  
>  
> [https://www.academy.attackiq.com/lessons/welcome-to-foundations-of-breach-attack-simulation](https://www.academy.attackiq.com/lessons/welcome-to-foundations-of-breach-attack-simulation)  

  

### **A new skill in a rapidly expanding field of security**

The need for breach and attack simulation tools and expertise is growing, and in this course, we are going to introduce you to the concept of Breach and Attack Simulation and Threat Informed Defense.

We’re also going to talk about different things you should consider when selecting a BAS tool and then we will cover some of the major use cases for BAS.

### **Hands-on labs**

I know for me, one of the best parts of training is getting my hands dirty in whatever it is I just learned. Throughout this course, you are going to use our AttackIQ Cyber Range along with the AttackIQ platform to perform some labs and hopefully put some of this new knowledge into practice.

---

## **Introduction to Threat-Informed Defense.**

In this lesson, we will take a look at what a threat-informed defense is. This includes looking at the main elements of threat-informed defense and discussing cyber threat intelligence analysis, defensive engagement of the threat, and focused sharing and collaboration. We will end this lesson with a quiz to test your knowledge.

  

- **Threat-Informed Defense Overview**
    
    ![[Untitled.png]]
    
    A threat-informed defense is a proactive approach to cybersecurity that utilizes three elements to provide an evolving feedback loop to your security team.
    
    Those elements are:
    
    - Cyber threat intelligence analysis
    - Defensive engagement of the threat
    - Focused sharing and collaboration
- **Cyber Threat Intelligence Analysis**
    
    Threat intelligence analysis is taking existing intelligence data like, malware hashes, or domain names and applying human intelligence to harden cyber defenses and improve ways to anticipate, prevent, detect, and respond to cyber-attacks.
    
    **MITRE CRITS**
    
    Let’s look at CRITS as an example of what goes into cyber threat intelligence analysis. CRITS is a tool developed by MITRE and stands for Collaborative Research Into Threats. It’s open-source and freely available [here](https://crits.github.io/). CRITS does a handful of things that assist with intelligence analysis such as:
    
    - Collecting and archiving attack artifacts
    - Associating artifacts with stages of the cyber attack lifecycle
    - Conducting malware reverse engineering
    - Tracking environmental influences
    - Connecting all of this together to shape and prioritize defenses and react to incidents
    
    CRITS itself is outside of the scope of this course, but it gives us a good illustration of some of the features of cyber threat intelligence.
    
- **Defensive Engagement of The Threat**
    
    Defensive engagement of the threat takes what you’ve discovered from intelligence analysis and allows you to look for indicators of a pending, active, or successful cyber attack. Breach and attack simulation tools fit in well here because we can take the behavioral models uncovered during intel analysis and use BAS to automate testing and reporting on what those behavior patterns look like in our enterprise.
    
    These simulation results can feed back into your threat intelligence analysis and into the next element we’re going to talk about, which is focused sharing and collaboration.
    
- **Focused Sharing and Collaboration**
    
    By sharing threat actor TTPs through standards such as STIX and TAXII the security community benefits together, or if you are part of a large organization with different security groups information shared between groups in a standard format can help your enterprise build a threat informed defense.
    
    Groups like MITRE’s Center for Threat Informed Defense (CTID) bring together sophisticated security teams from leading organizations around the world to expand the global understanding of adversary behaviors by creating focus, collaboration, and coordination to accelerate innovation in threat-informed defense, building on the MITRE ATT&CK framework.
    

---

## **Introduction to Breach & Attack Simulation Tools**

![[Untitled 1.png]]

Now that we’ve talked about the methodology of a threat-informed defense we can begin to talk about Breach and Attack Simulation as a way to operationalize and take a lot of the manual work out of implementing a threat-informed defense.

The general idea between breach and attack simulation tools is similar:

- Organizations can choose attacker behaviors they want to see executed in their environment.
- Behaviors are executed by the BAS tool.
- Operators observe the response from security controls.

How these ideas are implemented and additional features provided vary from vendor to vendor. We’re going to talk about things to consider when you’re investigating BAS solutions in a little bit, but before we do that I want to talk about Why BAS has become important.

- **How Breach & Attack Simulation Helps**
    
    Before breach and attack simulation tools existed, there were still plenty of organizations implementing or at least partially implementing a threat-informed defense. This work was originally done through purple teaming activities where red teams and blue teams would work together to improve their security posture. Purple teams still exist and are beginning to become more popular, but BAS tools can be used to help with some deficiencies of a manual process.
    
    **Time/FTE**
    
    - Red Team members are generally highly skilled individuals whose time could be better spent innovating instead of running scripts and building reports.
    - Coordination and sharing of information between red teams and blue teams consumes time that could be spent implementing projects and defending the enterprise.
    
    **Documentation**
    
    - Documentation during manual efforts is often lacking because of the time commitment or lack of resources to document what was done, how it was done, when it was done, and by who.
    
    **Safety**
    
    - Without tight collaboration or understanding between red teams and blue teams on what exercises are run by who, against what assets, and when the idea of testing the security of your network begins to feel more like a liability than an asset.
    
    When we get into Breach and Attack Simulation use cases later in this course we will explore in more detail how BAS tools help alleviate at least some of these burdens.
    
- **Deployment Approaches**
    
    ![[Untitled 2.png]]
    
- **Testing Approaches**
    
    ![[Untitled 3.png]]
    
      
    
- **Transparency Approaches**
    
    ![[Untitled 4.png]]
    
- **Framework Alignment**
    
    Although there are a few frameworks you could lay on top of BAS testing tools, the most prevalent is the MITRE ATT&CK Framework. Along with many defensive tools, breach and attack simulation tools often align themselves with the MITRE ATT&CK Framework.
    
    This makes sense for organizations that are trying to find a way to match security controls to offensive tactics.
    
    MITRE has organized attacker techniques into multiple categories along the attack chain. On the MITRE ATT&CK website, you can drill into techniques under each category to get a better understanding of how a technique works, threat groups known to use the technique, how to mitigate and detect the technique, and references to articles on the technique.
    
    Some breach and attack simulation tools allow you to understand where your defensive gaps may lie in the context of MITRE ATT&CK.
    
    - If the tool aligns to ATT&CK, you should be able to design your test based on techniques that are used by known threat actors.
    
    If the tool doesn’t have direct MITRE ATT&CK alignment, you can use a freely available online tool like the [MITRE ATT&CK Navigator](https://mitre-attack.github.io/attack-navigator/enterprise/) to understand the attack patterns of known threat actors and then find tests within your BAS tool that align to those techniques.
    
    ![[Untitled 5.png]]
    

---

## **Introduction to the Range**

![[Untitled 6.png]]

- **AttackIQ Terminology**
    
    ### Assesments
    
    Used to match assets to scenarios you want to run against them. All reporting and scheduling for testing is contained within each assesment.
    
      
    
    ### Assets
    
    Any host or machine in the enviroment that has the attackIq agent installed. Serve as the source address for any testing done in the enviroment.
    
      
    
    ### Scenarios
    
    The individual unit tests you are running during emulation testing.
    
- **Use Cases Overview**
    
    ![[Untitled 7.png]]
    
      
    
- **Continuous Security Validation**
    
    Continuous security validation is the process of taking your existing individual security controls, creating unit tests for those controls, executing those tests, and analyzing the results
    
    For example:
    
    - You have a DLP solution or a Firewall, and you are using it to block a specific rule or action.
    - For every rule or action you create, you should also design a test for that rule.
    - If I’m blocking a specific domain or URL, I would create a test that tries to reach that domain or URL.
    
    If I’m blocking a specific text pattern in my DLP, I would create a test that would try and mimic that pattern and exfiltrate data.
    
    Let’s keep it simple and stick to the Firewall example with a blocked URL. We will call it www.blockme.com.
    
    Once my rule has been created and the policy has been pushed to block www.blockme.com on the firewall, I create a test using a BAS tool or even scripting to try to make a connection from my network through the firewall and out to www.blockme.com.
    
    Now I execute the test and make sure that the results come back that it could not connect.
    
    It’s important to remember to execute this sort of testing against all firewalls to validate that the policy you pushed was deployed correctly.
    
    Once we’ve validated that our test to www.blockme.com is actually being blocked, we need to schedule this test to occur regularly so that we can be certain that the rule we put in place continues to work as desired.
    
    ![[Untitled 8.png]]
    
    ---
    
    ### **Lab 1 – Continuous Security Validation**
    
    To start we search for a template in the “Template library”
    
    ![[Untitled 9.png]]
    
    The start a new assesment using the “Content Filtering” Template.
    
    ![[Untitled 10.png]]
    
    And select “Create New”
    
    ![[Untitled 11.png]]
    
    This is the main assesment page
    
    ![[Untitled 12.png]]
    
    In the Assets tab click edit, and add the assets for the assesment
    
    ![[Untitled 13.png]]
    
    Then we select the assets, and apply
    
    ![[Untitled 14.png]]
    
    Click continue
    
    ![[Untitled 15.png]]
    
    On the next page, which is On Demand page, click Run Now
    
    ![[Untitled 16.png]]
    
- **Personnel Testing**
    
    Another use case that is fairly common for BAS tools is the testing of your security team. Whether that team is internal, an MSSP, or a combination of both.
    
    By simulating an actual attack you can understand how your team responds. This can be useful in identifying gaps in policy, procedure, or training.
    
    When designing assessments for personnel testing, it’s important to consider a design that would emulate that of an attacker.
    
    Using a tool like the MITRE ATT&CK Navigator can allow you to see the TTPs known to be used by advanced threat groups. It’s also important to keep in mind what sort of IOCs or events you expect your security team to report on during an incident.
    
- **Security Tool Bake Off & PoC Testing**
    
    Alright, so you’ve identified your deficiencies while performing GAP Analysis. It’s time to put your plan into action and start selecting tools you will purchase to cover those gaps. Here’s the problem – you want to be as certain as possible that the tool you are about to spend a lot of money on actually follows through on the promises to fill those gaps.
    
    By taking a scientific approach that is measured and repeatable with each solution to be tested, you can make sure that you are choosing the best tool to meet your needs. BAS tools fit in well here because they allow you to take a lot of the manual process and documentation out of the equation.
    
    Here are some suggestions I’ve given security teams in the past:
    
    - Make sure your testing scope only includes tests that make sense for the solution you are evaluating. It doesn’t make sense to run credential theft testing against a network firewall solution and can skew results.
    - If possible, execute your testing in production to get the most accurate picture of how the product will perform in your environment
    - Use a control – For example: If you are testing endpoint solutions, make sure that one of the hosts you are testing does not have that endpoint solution installed. This allows you to see where there may already be some overlap in coverage or a false reading in your testing.
    
    Another side benefit of performing testing this way is that when you do choose a solution to purchase and implement, you will already have the test designed that will help to verify that your implementation is correct. You can also use this same test plan continuously with that security control to ensure that environmental changes to your enterprise do not affect how the security control operates.
    

---

- Purple Teaming
    
    Red Teams are expensive and highly specialized. They should be innovating, not playing gotcha! Blue Teams are overworked and spread too thinly. They should be hunting, not maintaining.
    
    Purple Teaming is an organizational concept by which red and blue functions occur simultaneously, continuously, tightly coupled, and with full knowledge of each other’s capabilities, limitations, and intent at any given time.
    
    Given reliable access to red capabilities, this methodology allows security teams to iteratively increase program maturity as a product of continuously clearing low-effort attacks from the board.
    
    Let’s take a look at the workflow of a purple team.
    
    1. Red Team executes iterative attacks against friendly cyberspace, tuned to replicate adversary capabilities and prevent irrecoverable disruption  
    2.  
    Stopped attacks generate reports of detection and mitigation details back to the Red Team  
    3.  
    Successful attacks generate reports of the attack method and exposure details back to the Blue Team.  
    4.  
    Red and Blue Teams jointly debrief all actions in coordination with IT Ops; mitigations emplaced, attack techniques refined, attack surface reduced  
    5.  
    Continuous testing and improvement refines detection capabilities and enables ever-more difficult scenario execution, which refines detection capabilities.
    
    ![[Untitled 17.png]]
    
- **How BAS Applies to Purple Teaming**
    
    ![[Untitled 18.png]]
    
    - Breach and Attack simulation tools can help with Red Team execution by providing a platform to make sure test procedures are safe, controlled, and documented.
    - Integrations with other defensive security tools like EDR, Firewalls, AV, and IDS/IPS can allow BAS tools to provide instant feedback in a centralized manner to the Red Team
    - Those same integrations can provide instant feedback and centralization for Blue Team members as well. Some BAS platforms will also provide mitigation information to the Blue Team as well.
    - During the joint debrief, data collected by the BAS tool can be analyzed by both Blue and Red team members. This data can be used as suggestions for both sides on the next piece, which is
    - Continuous testing and improvement. Breach and attack simulation tools allow you to begin automating many of the low-level tasks the red team is doing so that they can continue to innovate. Blue teams are also provided with a way to run those lower-level red team tasks themselves to validate that the measures taken to resolve red team discoveries are always working.

---

- **Quality Assurance**
    
      
    
    Quality Assurance testing can utilize BAS tools to help make sure security configuration on golden images or new server deployments is correct. Testing your golden image with a BAS tool can greatly decrease the risk of deploying new workstations with improper configuration.
    
    Here are a few things to keep in mind:
    
    - Design your tests to match the security controls you put on the host. This may include things like bypassing UAC, privilege escalation, registry modification, or credential theft.
    - Don’t just focus on security tool testing. Consider testing operating system policy and other native controls.
    - Utilizing a BAS tool with RBAC features can allow Desktop QA engineers to execute testing without having access to results for separation of duties.
    - Utilizing a BAS tool with an API can allow the process of testing to be baked into QA automation tools
    
    In a world where we are seeing more and more automated deployment of servers, it makes sense that security teams are becoming more and more involved with the quality assurance of these servers. Breach and Attack simulation tools can allow security teams and server deployment teams to feel confident in the configuration and setup of new assets.
    
    Some things to consider when using BAS in conjunction with server deployment;
    
    - Don’t forget about a threat informed defense – keep tests lightweight and fast by only testing what you’ve discovered from intel analysis.
    - Utilize a BAS tool with an API to automate the process and test rapidly
    - Using a BAS tool that integrates with your security stack can help security operations teams quickly pinpoint what failed if a test does not pass