---
title: Security, Malware, Firewall, IDS, and other
author: Richard Hernandez
date: 2021-12-08 14:10:00 +0800
categories: [Notes, Class]
tags: [firewall, IDS, Malware, virus]
---


## Chapter 8 Intrusion Dection

#### Types:
* Cybercriminals: hackers, cracker, organized groups for money, identity theft, credentials, etc
* Activist: Motivated social polictica issue, DoS attacks, theft, negative publicity.
* State sponsored organization: hackers sponsored by government for (APT)
* Others: other motivations
* Apprentice: use toolkits
* Journeyman: Modify and extend attack  toolkits

> Examples of Intrusion: defacing webserver, copying database, sniffing to capture credentials,  posing, etc

#### Intruder behavior

* **Target Acquisition and Information gathering:**  public,technical, network exploration
* **Intial Access**: remote vulnerabilities, guess passwords, malware isntallation
* **Privilage Escalation**: local root access
* **Information Gathering or System Exploit**: scan files, transafer data outside
* **Maintaining Access**: installation backdoor
* **Covering Tracks**: desible logs, hide files

**Security Intrusion** event when intruder gets access or attemps.

### Intrusion Detection System(IDS)

#### 3 logical Componets:

1. Sensors: data collection
2. Analyser: out if intrusion
3. User Interface: output, control

#### Location of IDS:

1. Host-Based IDS(HIDS): single host
2. Network-Based IDS(NIDS): network segments, tarffic
3. Distrubuted or hybrid IDS: combines both, central analyser, multiple sensors

#### Threads types: 

1. Masquerader: false identity
2. Misfeasor: legit user, not authorized
3. Clandestine user: covering action by delteting logs
4. Port Scans: inforamtion gathering for TCP connections
5. DoS attacks
6. Malware Attacks
7. ARP spoofing
8. DNS cashe poisoning: changing DNS cache


#### IDS Componets: 
* IDS manager: compiles data to detect intrusion , determination based on site policies. Detects and sound Alarm

#### IDS Requirements

1. Run continually
2. Fault tolerant
3. Resist subversion
4. Impose a minimun overhead
5. Configure accourding to policies
6. Scale
7. Degradation Services
9. Allow Dynamic re-configuration

#### Analysis Approach

* **Anomaly Detection**: whether behavior is legit or intruder. Done by collecting data of legit user. Classification Approach for Detection:

1. Statistical: analysis of univariate, multivariate, or time-series model.
2. Knowledge base: by observed behavior accourding to set of rules
3. Machine Learning: automatic based on traning data

* **Signature/Heuristic Detection**: Malicious data patterns or attack rules and compared to current behavior. Approaches:

1. Match large colletion of know patterns agasint traffic
2. Signatures need to be large to decrease false alarms rate, still detect large fraction of malicious data.
3. Anti-virus, network scanning proxies or NIDS.

* **Rule-based Heuristic Identification**: 

1. Use rules identify know exploits
2. rules used are specific
3. Rules for Identify suspicious behavior


#### Base-Rate Fallacy
> base-rate fallacy: probability of some conditional event is assessed without considering the "base rate" of that event
IDS needs to: most intrusion with few false alarms.
* to few intrusion -> false security
* to many false alarms -> waste of time

### Host-Based Intrusion Detection (HIDS)

* Monitor activity 
* Halt attacks before damage is done
* Detect internal and external Intrusion

#### Sensors of HIDS:

1. System call traces
2. Audit(log): user activity
3. File integrity checksums: (difficult to monitor files, and need generate checksum for every file)
4. Registry access

### Network-based Intrusion Detection (NIDS)

* Monitor traffic selected points
* Traffic packet examination in real time
* Examine Protocol Activities
* Analysis of traffic patterns by sensors or both

#### Sensors in NIDS

They can be deployed in two mode:
1. Inline: traffic most pass through the sensor
2. Passive: monitos a copy of the traffic

### Stateful Protocol Analysis(SPA)

* Subset of **anomaly detection** compared observed vs predetermine universal vendor supplied profiles. Key disadvantage is the high resources usage it requires.

### IETF Intrusion Dectection Working Group

* Define data formats and exchange procedures for sharing information.

### Honeypots

* System is filed with fabricated data with no value to user.

1. Divert atacker from critical systems
2. Collect information about attacker activity
3. Encourage attacker to stay longer.

#### Low Interaction Honeypots:

* Emulate an IT service or system, but doesn't execute full version.
* Less realistic Target
* Sufficient as componet of IDS to warn of attack

#### High Interaction Honeypots:

* Real system, full OS with services and app
* Realistic target
* Use more resources 
* If compromised can be use to attack the system

#### Honeypots Locations

* Outside the external Firewall: tracks attemps to connect to IP within scope fo network. Reduces the alerts issued by firewall and internal IDS. Disadvantages little ability to trap internal attacker, mainly if firewall blocks traffci both directions.
* Network of externally avaible services(email, web) called DMS(`Demilitarized zone`) other systems are secure by the activity generated by the Honeypot. Disadvantage DMZ not fully accessible, firewall blocks traffic from DMZ and unneed services. Solution Opening up firewall but increase risk of attack.
* Fully internal, which can catch internal attacks and Detects misconfigured firewalls. Disadvantage if compromised can attack internal system. Also configuring firewall to allow traffic to honeypot potentially increasing risk, and configuration mistakes.

### Snort

Open source, configurable, protable host-or-network based IDS.Lightweight. Perform real time package analysis can capture, also content matching:

* Easy to deploy
* Efficient operation
* Easy configure

Intallation consist of 4 logical components:

1. Packet decoder
2. Detection engine
3. Logger 
4. ? **Search later**


## Chapter 9 Firewalls and Intrustion Prevention Systems


### Firewalls
**Firewall**: prevent access of unauthorized electronics to networks.

#### Design Goals: 

1. All traffic must pass thorugh it, physically blocking all access
2. Authorized traffic define by local policy.
3. Imune to penetration.

#### Firewall Policies

Filter incoming and outgoing traffic based on set of rules. types of traffic, address ranges, protocols, application, and content type

#### Policy Action:

1. Accept: allow pass
2. Dropped: not allow, no alarm
3. Rejected: not allow, sound alarm

#### Approuches for firewall

* Blacklist: all package allowed except from one on list
* Whitelist: (default) all packages blocked except the one on list

#### Firewall Filter Characteristics

1. IP address and protocol values
2. Apllication Protocol
3. User identity
4. Network activity

#### Firewall Capabilities and Limits
1. Single choke point
2. location for monitoring security related events
3. IPsec, tunnel mode to implement VPN
4. Can't protect agasint attack that bypass firewall
5. May not protect agaisnt internal attacks
6. Can't guard agaisnt misconfigured wireless LAN
7. Inffected devices can infect the network

#### Firewall Type: 

1. Packet filter(**Stateless**): matches  a set of rules.
2. **Statefull** filters: records all connection, and determine, new,existing, etc
3. Application-proxy: inspect content of traffic, blocking what consider inappropiate
4. Circuit Level proxy

#### Stateless 

* Advantages of Packet Filtering: simplicity, transaparent to user, very fast.
* Weakness of Packet Filtering:
1. Can't prevent application specific vulnerability
2. Logging functionality is limited
3. No support Advance user authentification schemes
4. vulnerable to attacks and exploits of TCPI/IP
5. Improper config lead to breaches

**Attacks:**

1. IP address spoofing. Counter measure-> discard packet with inside source address if arrive external
2. Source routing attack: Countermeasure -> is to discard all packets using this option
3. Tiny Fragments attacks: Create small packets and force TCP header into different packet fragments. Passing by the firewall rules. Countermeasure -> enforcing rule first fragment  of packet must contain predefined minimun amount of the transaport header.

#### Statefull Inspection Firewall

Entry for each established connection, allowing incoming traffic for those packets that fit profile, of one of the entries.

#### Stateful Firewall

Tables containing  data on each active connection, ip, ports, and sequence #. It does similar to packet filter firewall but records TCP connection.

#### Application-Level Gateway

Acts as a relay of application-level traffic, User need to athentificate himself. Disadvantages is addtional proccessing overhead on each connection.

#### SOCKS Circuit-Level Gateway

Client-Server applications in TCP/UDP use services of network firewall. Consist of the following components.
1. SOCKS server
2. SOCKS client library
3. SOCKS-ified version, re-linking or re-compilation using SOCKS library to encapsulate the the library.

#### Bastion Host

Firewall Administrator as critical strong point in security. Plataform for application or circuit gateway. Characteristics are:
1. SSecure version of OS
2. Essential installed only
3. Additional Authentification
4. Support from subset of protocols
5. Configure for specific host
6. Eac proxy loggs all traffic and duration of connection
7. Proxy modeule is very small
8. Proxy is independent of others
9. Proxy requires no disk access, besides inital config
10. Proxy runs as non-root user

#### Host-Based Firewall

Module comes with the OS. Advantages include:

1. Custom Filtering rules
2. Protection independent from topology
3. Additonal layer protection

> Personal firewall: deny unauthorized remote access, monitor outgoing traffic to detect and block worms, and malware.


### Intrusion Prevention Systems IPS (IDPS)

IPS blocks traffics but uses detection algorithms from IDS to determine when to do so. It can be Host- , Netowrk-, or hybrid.

#### Host-Based IPS (HIPS)

* Signature: focus on content of the application network traffic, sequence, pattern, sys calls, etc
* Anomaly: IPS checks **behavior** pattern that indicate malware Ex:
1. Modification of sys resources.
2. Privilege escalation exploits
3. Buffer-overflow
4. Access contact list email
5. Directory traversal

> A prudent approach is to use HIPS as one of elemnt in the defense.

#### Network-Based IPS (NIPS)

* Can modify or discard TCP connections, may provide flow data protection:

1. Pattern matching
2. Stateful matching
3. Protocol anomaly
4. Traffic anomaly
5. Statistical anomaly

#### Digital Inmune System

Comprhensive defense  agasint behavior casused by malware.Its success depends on the ability of the malware analysis to detect new and innovative malware strains.

#### Snort Inline

modified version of **Snort**, enhancing it and providing functionality similar to a IPS. Three new rules are added:
1. Drop: drop based on rule and logs
2. Reject: rejected, logged, and error returned
3. Sdrop: rejected but no logged



## Chapter 6 - Malicious Sofware

* Malware: A program intent of compromising confifentiallity, intergrity  or availability.
* Adavance Persistent Threat: cybercrime targeted to bussines or political targets.
* Adware: ads that integrated into sofware
* AttackKit: tools for generating malware. (Zeus crimeware, Angler)
* Auto-rooter: Break into new machines remotely
* Backdoor: access to unauthorized system bypasssing security(Maintenance Hook)
* Drive-by-Download: code that eploit browser vulnerability to attack
* Exploits: code specific for single or multiple vulnerabilities.
* Flooders: generate large volume of data, DoS
* Keyloggers: record credentials, keystrokes
* Logic Bombs: Stays dormant until triggerd by event or condition.
* Macro virus: scripting code attached to documents, can replicate
* Mobile Code: 
* Root kit: tools after attacke is in system.
* Spammer programs: large volume of email
* Spyware: collecs information from several sources
* Trojan Horse: Appears useful but execute payload in back
* Virus: When executed replicates it self to other executables
* Worm: runs independent, can propagate a complete version to other host.
* Zombie,bot: activate when lunching an attack on other machines.

#### Malware Classification Categories

1. How it propagates
2. Actions and Payload
3. Aditionally
* Need a host program
* Indpendent, self-contained
* Don't replicate

**Propagation Mechanism**:

1. Infection of existent content
2. Exploit sofware vulnerabilities(worms)
3. Social Engineering attacks.

**Payload actions once in target**

1. Corruption
2. Theft service/make system zombie
3. Theft of data (keylogger) 
4. Stealthing/hiding

#### Attack Sources

1. Politically motivated
2. Criminals
3. Organized Crime
4. Organization(selling services)
5. National Goverment Agencies

#### APTs Characteristics:

* Well resouced
* State sponsored
* Advance: dev custom malware
* Persisten: maximize order of success
* Threats: increase level of thread
* Aim: theft, intelectual properties, security,physical disruption
* Techniques used: social enginerring, spear-pishing, drive-by-download, etc
* Intent: to infect

#### Virus Characteristics

* Modifies and includes a copy of the virus
* Replicate into other content
* Easly spread through networks

Phases of Virus:

1. Dormat: is idle
2. Triggering: condition or event 
3. Propagation: copy of itself, replicates
4. Execution: Payload or action

Macro virus: they attack to documents or other media files:

* Plataform independent
* Easly spread, in traditional file systems
* easier to write or modify.

#### Virus Classification:

* **By target:** 
1. Boot sector infector
2. File infector
3. Micro virus
4. Multipartite virus: multiple way infecting

* **By concealment**
1. Encrypted
2. Stealth
3. Polymorphic: mutates apparence
4. Metamorphic: mutates and rewrite himself.  apparence, and behavior

#### Worms 

**Worm Replication:**

* Mail, instant msg
* File sharing
* Remote execution: execute himself in other system
* Remote file access or transfer capability
* Remote login capability

**Worm Target Discovery**

* Scanning (Fingerprint): search system
* Random: Probes random IP
* Hit List: list of potential targets
* Topological: based on information on host
* Local subnet: own local network

> Morris Worm: 1988 UNIX systems. Exploit UNIX finger protocol, sending emails. First worm

> WannaCry: Ransomware,  scan networks local and random to jump other systems. Encrypts files and ask for ransom to decrypt. UK researcher activated a kill switch. 

**State of art Worm tech**

* Multiplataform
* Multi-exploit
* Ultrafast-spreading
* Polymorphic: evade detection
* Transport vheicales
* Zero-day Attack: vulnerability not jet discovered

#### Mobile Code

Included when running cross-site scripting, interactive and dynamic websites, download from untrusted sofware. Target smartphones, delte data. Also CommWarrior replicates using Bluetooth.

#### Drive by Download

* Exploits browser vulnerabilities and plugins
* Doesn't actively propagates as regular worms
* Spreads when visiting websites

**Watering Hole Attacks**:
* Highly targeted
* has information on websites victim visit
* Then wait for them to visit
* Infect, and takes no action other visitor

#### Malvertising

* Placing malware without comprimising website.
* targeted websites
* Dynamically generated reduce detection
* Stay for as little as few hours

#### Clickjacking

* Lead user to belive they are typing in real UI(redress attack) but there is invisible interface stealing the data.
* Colecting users clicks
* Adjust browser setting 
* Place button under legit button
* routing clicks to another site.

#### Social Engineering

* Spam 
* Trojan Horse
* Mobile phone trojans

#### Payload System Corruption

1. Chernobyl virus: over write first megabyte with zeros.
2. Klez: Mass maling, can stop, delte antivirus programs, casue files to become empty
3. Ransomware: Encrypts data and ask for ransom. 


#### System Curruption

1. Real-world Damage: cause physical, like changing BIOS like Chernobyl or Stuxnet target industrial
2. Logic Bomb: embedded code in malware

#### Information Theft

1. Keylogger
2. Spyware

#### Phising 
1. Mascarading as trusted source
2. Spear-phising: targeted

#### Stealthing Rookit

* gives root access to attacker
* Persistent: every time it boots
* Memory based: cant survive reboot
* User mode: intercept API calls, returns modified results
* Kernel Mode: Intercept native API, removel itself from kernel list
* VM: install it self in VM
* External Mode: direct access to hardware, BIOS, etc

#### Malware Counter Measure

Prevention

* Policy
* Awareness 
* vulnerability mitigation
* Threat mitigation

Mitigation:

* Detection
* identification
* Removal

#### Anti-virus Generation 

1. Simple Scanner
2. Heuristic Scanners
3. Activity Traps(identify by action)
4. Full-featured

#### Sanbox Analysis

Run malware isolated to analyse the beahavior and to better detect in furute

#### Host-based behavior-blocking Software

It blocks potentially malicious actions before  they have a chance to happen

#### Perimeter Scanning Approuches

* Anti-virus
* traffic analysis
* ingress and egress monitoring(wierd behaviors)



## Chapter 10 - Buffer Overflow

Can occur in Stack, Heap, Data Section of proccess.

**Consequences**:

* Corruption
* Transfer control
* Memory Access violation
* Execution unknown code

#### Attacks 

1. Identify vulnerability
2. How is the buffer been stored

**Fuzzing tool** for identification of vulnerable problems

#### Stack Smashing

* Morris Worm exploits this vulnerability, vulnerable code like gets() fro C language. 
* Uses return address to execute shellcode

#### Shellcode

* Normally used to trasnfer control of the CLI to the attacker with high privilages.
* Required good assambly skills
* Metasploit helps create shell code.

#### Stack Overflow Variants

* Targeted: system utility, Program library code(if not static linked), Network Deamon
* Shellcode Functionality: flush firewall, listening services, remote shell, privilage escalation, backdoor

#### Buffer Overflow Defenses

1. **Compile-Time:** Harden new programs
 * Use modern language: notion of type, not vulnerable BO, higher cost resources, limit usefullness, example creating drivers
 * Safe Coding Techniques: inspect code, rewrite unsafe code, audited existing code base. 
 * Language Exntension/Safe Libraries: Handeling memory allocation, safer varients of functions
 * Stack protection: GCC compiler extension(add dummy functions),check for corruption of stack
2. **Run-Time**: detect and abort attacks, in exsisting programs
 * Executable Address Space Protection: memory non executable
 * Address Space Randomization
 * Guard Page: Flagg MMU as illigal address, any attemp access abort


#### Attack: Replacement Stack Frame:

Replace stack frame with a dummy stack frame.
> Defense: randomization address, stack protection, non executable stack


#### Return System Call 

* Use retunr addres to execute shellcode
> Defense: randomization address, stack protection, non executable stack

#### Heap Overflow

* Located in the Heap, dynamic data structures
* No return address
* No easy to transafer control

> Defense: Making heap non executable, Randomize address allocation

#### Global Data Overflow

* Attack buffer global data
* Vulnerable adjesent proccess
* Overwrite function pointer to later call

> defense: Non executable, move function pointers, guard pages



## Chapter 11 - Sofware Security

#### Security flaws

5 Security sofware flaws:

1. Unvalidated Input
2. Cross-Site Scripting
3. Buffer overflow
4. Injection Flaws
5. Improper error handling

NIST Reccommendation:

1. Stop vulnerabilities before they occur
2. Find vulnerabilities before they can be exploited
3. Reduce impact of vulnerabilities by resilient architectures

#### Sofware Security, Quality and Reliability:

1. Quality and Reliability: focus eliminate as many bugs possible. concern is not how many bugs but how often they are triggered.
2. Security : Probability distribution for targeting bugs that result in failure and can be exploited. Triger by input that differ dramatically from what is usually expected.

#### Defensive Programming

Never assume anything, check all assumptions and handle any possible error states.

#### Handling Program Inputs

* Common
* Input is any source data from outside.
* Identify all data sources
* validate size and type

#### Interpretation of program Input

* binary or text, binary depends on encoding
* Check are character types with care
* Fail to validate result in vurnerability

**Injection attacks** is by handeling input wrong way, attacker execute code.(scripting languages more often)

#### Cross Site Attack Sctipting (XXS) 

Input provided by one user is outputed by another user. Script in HTML Code, perform security checks and restrics data access to pages originating from the same site.

#### Validating Input Syntax

Compare against wanted string, compare input to dangerous values or accepting only know safe data.

#### Alternate Encodings

#### Input Fuzzing 

Tetsing Technique for tetsing input to program.
* Handle abnormal inputs
* Template to genrate of know problems inputs

#### Writing  Safe Program Code

1. Correct algorithms implementation
2. correct machine instruction for algorithm
3. Valid manipulation of data

#### Operating System Interaction

1. Programs Execute under OS control(enviroment, variables. shares resource)
2. System has concepts of multiple users(Lelvel Access, permission, shared,etc )

**Enviroment Variable**: string value inherityed by each proccess.

#### Vulnerable Compiled Programs

* Vulnerable to PATH manupulation
* Dynamically linked may be vulnerable to manupulation of PATH

#### Preventing Race Conditions

* Use Synchronization mechanism to lock the shared file.
* `Lockfile` to cooperater in sharing a file

## Operating System Security


