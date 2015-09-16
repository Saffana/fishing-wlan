#Phishing-wlan


## Introduction
Our goal is to get the victim's credentials while he is trying to connect to a website
The project takes place in our network course. The aim of this project is to practice and learn how a particular aspect of a network works.

This project can be sum up by several steps :
- Scan the Network and choose a victim
- Setup a MITM Attack on the Local Area Network
    - ARP Spoofing
    - DNS Spoofing
    - steal SSL certificate
- Steal credentials

##1 - What we are doing

###1.1 - Global Process
The first step of this attack is to look for a target by scanning the network, and getting his network gateway. (insérer capture du nmap).
Then, we can make his him think that we are its gateway. At this point, the target can't reach the internet. This is not what we want, the MITM has to be as invisiable as possible.
So, the solution is to use ARPSpoofing, the aim is to deliver all the packets towards the real gateway by spoofing the targets ARP table.
For the target, he is just surfing on the internet.
For the attacker, he can see all the packets and eny information about the target.


####1.1.1 ARP Step
Before spoofing the target, we can lookup on his ARP table and see the relation between IP adress and MAC adress.
Avant de lancer l'attaque on peut regarder la table ARP  de la victime. La table arp fait une correspondance entre Adresse Ip et adresse MAC.
On peut donc y voire l'adresse IP de la box associé à son adresse MAC. Le but de l'arpspoof est d'envoyer une trame ARP (donc de niveau 2), 
et en réaliter inonder la victime de ces trames pour la forcer à mettre à jour sa gateway. Si on lance un wireshark sur la victime on voit un grand nombre de requete ARP pour la forcer à changer sa table
Ex ARP spoof : 
MAC_PIRATE tell MAC_TARGET that <IP_GATEWAY> is at MAC_PIRATE
Si on regarde à nouveau la table arp de la victime on s'appercoit que la gateway a changer de mac. On remarque aussi que cette adresse MAC match une autre IP : celle de l'attaquant.







###1.2 - what we did to legally test security flaw
- Work on a personnal wlan
- Every test was ran on our personnal machines

###1.3 - Counter measures to prevent this kind of attack
For the website owner :


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
We used Git (and especially GitHub) to share the code. (https://github.com/as3nds/fishing-wlan)
We also used c9.io to work in a collaborative way.


## - Definitions
AP  : Acces Point, device necessary to connect client on a wireless network.
MITM : Man In The Middle, this attack is based on relaying secretly the communication between a client and the AP.
ARP Table

## - Used Programs :
- nmap : Scan the network in order to find a target
- arpspoof : Allows to usurp the target's gateway
- wireshark : Network analyzer, shows all the packets flowing on the network including protocols and many informations
- dnsspoof : Listen and intercept the target's DNS requests in order to apply a fake DNS table.
- sslstrip : This tool, used in a MITM attack, can change requests protocols from HTTPS to HTTP. Only works with website that have HTTP 'AND' HTTPS connection
    