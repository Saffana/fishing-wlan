#Phishing-WLAN


## Introduction
Our goal is to get the victim's credentials while he is trying to connect to a website.
This project takes place in our network course. 
The aim of this project is to practice and learn how a particular aspect of a network works.

This project can be sum up by several steps :
- Scan the Network and choose a victim
- Setup a MITM Attack on the Local Area Network
    - ARP Spoofing
    - DNS Spoofing
    - steal SSL certificate
- Steal credentials

##1 - What we are doing

###1.1 - Global Process
The first step of this attack is to look for a target by scanning the network, and getting his network gateway. 

            (insérer capture du nmap)

Then, we can make the target's routing table think that we are its gateway via ARPSpoofing, and redirect all the DNS requests with some DNSSpoofing.
For the target, this is just a normal browsing over the internet.
For the attacker, he can see all the packets and get any information about the target.


####1.1.1 ARP Step
Before spoofing the target, we can lookup on his ARP table and see the relation between IP adress and MAC adress. 
With those informations, we can get the AP IP and MAC adress. 

            (cf. screen arp before spoofing)

We can now start the ARPSoofing, this will have the effect of sending continuously the same ARP packets (level 2 messages) to the target.
The point in doing this, is to force the target to think that the gateway we are sending in the ARP messages is his gateway.

If we open wireshark to sniff the network, we are able to see a lot of those ARP messages from the attacker.

Example of an ARP spoof message: 

<MAC_PIRATE> tell <MAC_TARGET> that <IP_GATEWAY> is at <MAC_PIRATE>


Then, if we look to the target's ARP table, we can see that he uses our gateway.
At this point, the target can't reach the internet since he is asking to our gateway.
To make him communicate throw the attacker's gateway, we need to activate the IP forwarding.
So now, the target can surf on the internet, the MITM is in palce, the hacker can see what is comming throw his connections.

###1.1.1.2 DNS Step
This step is necessary to exploit a MITM attack.
The purpose of the DNSSpoofing is to intercept the target's DNS requests to be able, if required, to redirect them.

###1.1.1.3 Serveur HTTP Step
Once the DNSSpoofing is working, our objectives is to redirect he target to our own webservice wichi 
Pour que la redirection DNS fonctionne il faut qu'un serveur http tourne sur la machine où  a été rediriger la victime


###1.2 - what we did to legally test security flaw
- Work on a personnal WLAN
- Every test was ran on our personnal machines

###1.3 - Counter measures to prevent this kind of attack
For the website owner :
 - not providing an HTTP version of his website
 - acquire a legitimate HTTPS certificate

For the internet user :
 - always browse on a secured network you trust (ie not on a public WI-FI)
 - always browse on a HTTPS website with trusted and verified certificate


##2 - Encountered problems

###2.1 - What they were
- HTTPS protocol, while in the MITM, we can 'read' everything as long as the target is on HTTP. If we ask for HTTPS website, all the packets are protected. 
- HTTPS with SSLStrip, Even with ssltrip, we still have problems to get credentials on specific websites (facebook.com and gmail.com for exemple). Those website, do not use http at all, so sslstrip does not have eny effects.

###2.2 - How we actually dealt with them
- HTTPS protocol, the solution to get readable informations with this protocol is to use SSLStrip.
- HTTPS with SSLStrip, we can't use it on facebook.com or gmail.com, we have to find an other website with less protection.
    
##3 - Differences with the Overview's Objectives

###3.1 - What are those differences (if any)?
- We wanted to get the credentials from gmail, but since this site is well protected against this sort of attack, we can't use it as our target.

###3.2 - Why did we not met our objectives.

###3.3 - What are the possible solutions to meet them.
- Find a website less protected and/or having http and https protocols.

##4 - What we wanted to learn with this project and what did we really learn
We wanted to learn how to use some basic tools provided natively with kali linux.

##5 - How did we share the code
We used Git (and especially GitHub) to share the code. (https://github.com/as3nds/fishing-WLAN)
We also used c9.io to work in a collaborative way.


## - Definitions
AP  : Acces Point, device necessary to connect client on a wireless network.
MITM : Man In The Middle, this attack is based on relaying secretly the communication between a client and the AP.
ARP Table : It's a database with all the relation between IP adress and MAC adress.

## - Used Programs :
- nmap : Scan the network in order to find a target
- arpspoof : Allows to usurp the target's gateway
- wireshark : Network analyzer, shows all the packets flowing on the network including protocols and many informations
- dnsspoof : Listen and intercept the target's DNS requests in order to apply a fake DNS table.
- sslstrip : This tool, used in a MITM attack, can change requests protocols from HTTPS to HTTP. Only works with website that have HTTP 'AND' HTTPS connection
    