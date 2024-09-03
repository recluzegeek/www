---
title: Cyber Hackathon Workshop Experience — Day 1
author: recluzegeek
pubDatetime: 2024-09-3T18:11:06.130Z
description: Personal experience about attending first ever cyber workshop in person.
tags:
    - write-ups
    - cyber
    - hackathon
slug: cyber-hackathon-workshop-experience-day-1
featured: true
draft: false
---

[IGNITE](https://ignite.org.pk/) in collaboration with Ministry of I.T has been organizing Cyberhackathon across major cities of Pakistan to attract youth towards untapped jobs of cybersecurity for past 4 years. Before the actual hackathon, organizers arrange workshops across cities to give the participants chance to brush up their cyber skills and gain knowledge from the industry experts. I had the chance to attend one of the workshops today & I'll be documenting my experience in this writeup.

## Table of Content

## Distinguished Speakers

- [Ahsan Khurshid](https://www.linkedin.com/in/ahsankhurshid/) — Team Lead - Security Assessments & Forensics @ Trillium Information Security Systems
- Salman
- [Ali Zain](https://www.linkedin.com/in/syedalizain033/) — Project Manager for Cyber Security @ Trillium Information Security Systems

## Material Covered

Mr. Ahsan gave the overview of Cyber Security Space — how threats are evolving, state sponsored targeted incidents of stuxnet and lazarus. Discussed the benefits of adoption of pro-active over the traditional approaches.

Mr. Salman gave us overview about Networks & Securing them — OSI Models, TCP/IP models, IPv4/IPv6, IP subnetting, IPS/IDS and different types of WLAN Network attacks.

Mr. Ali Zain gave us more of hand on practice about network analysis tools — `wireshark`, `nmap`, `nikto` and `burpsuite`. We played 6 CTFs and were explained the

### Cyber Security Overview

In this section we were briefed about brief history, attacks classification, CIA triads, Mitre ATT&CK and CTFs. Mr. Ahsan was kind enough to walkthrough us with the overview of the ever evolving cyberspace & took few quizes on [Mentimeter](https://menti.com) to check our understanding and at the same time keep us engaging.

#### Common Cyber Security Threats

Discussed the potential of cyber space in the upcoming era, subfields of cyber security. The rise of threat intelligence, adoption of proactive approaches over traditional active approaches, mindset development and then skill development were discussed. We talked about types of attacks, which are commodity attacks happening 90% of the time, targeted attacks having 9.9% share and 0.1% belongs to state sponsored targeted attacks. We discussed the cyber-security in view of geopolitical scenarios such as **stuxnet and lazarous incidents** and the need of the cyber-skilled personals to defend the national interests.

#### CIA Triads

CIA triad is made up of Confidentiality, Integrity, Availability and are said to be the pillars of cyber space along with the new additions of non-repudiation and authorization. Below is brief explanation of each of the terms:

- **Confidentiality:** Confidentiality refers to data protection from unauthorized access.
- **Integrity:** Data should not been tempered or altered with by unauthorized access. MITM attack may violate this principle.
- **Availability:** Data remain accessible to the intended users. DDoS attack may violate the availability of the CIA triad.
- **Authorization:** Resource utilization should be authorized, only authorized persons should have access to resource. Open access to WLAN connections in an event is a violation of authorization.
- **Non-Repudiation:** Hold accountable persons that performed the action. In case of a breach forensic, non-repudiation principle may help forensics to pin down the source or location from where the attack originate. Logging activities is one of the ways to achieve non-repudiation.

#### Mitre ATT&CK

I'm not a beginner in cyber space and have been following it for about 4 years, but the fact that I was unfamiliar with such goldmine struck me hard internally. [Mitre ATT&CK](https://attack.mitre.org/) which stands for Adversarial Tactics, Techniques and Common Knowledge is a database of adversary tactics and techniques based on real-world observations. Quoting the website,

> The ATT&CK knowledge base is used as a foundation for the development of specific threat models and methodologies in the private sector, in government, and in the cybersecurity product and service community.

#### Cryptography & Cryptanalysis

Several cryptographic algorithms such as Ceaser cipher, Motor cipher, ROT13, Vigenère cipher and several others were discussed. We talked about the difference b/w hash functions (irreversible & fixed length output) and encryption (reversible & output length varies of the input), types of encryption (symmetric & asymmetric). We saw a practical demo of ciphers, hash functions when input certain string and understood their functionality well.

Impacts of Quantum computing leaps, threats of Quantum computing to the current encryption algorithms, rise of Quantum Encryption & Cryptography were talked in details.

#### CTFs — Intro & Getting Started with

The concept of CTF wasn't new to me, I've practiced some CTFs online and found them to be difficult considering my knowledge back then and left considering it was only for those with deep dive computer and networks knowledge.

Capture the flag or CTFs events are **gamified competitive hacking events** that are based on different challenges or aspects of information security. We practiced 6 CTFs with Mr. Ali Zain with `wireshark` and it was totally fun exploring things :)

Hack the Box, TryHackMe and several other providers exists online that provides machines or challenges to the persons looking to improve their skills.

### Networks Refresher & Network Security

In this section, Mr. Salman gave us walkthrough with Networks Fundamentals, Network Threats and implementing measures for Networks Security.

#### Fundamentals

Quick refresher to the network fundamentals such as IPv4/IPv6, OSI model, TCP/IP model, NAT, Subnetting were given by the speaker. Transition of IPv4 to IPv6, why we the need of OSI model arose when the TCP/IP model existed & the role of NAT in accommodating the internet devices within the IPv4 was discussed.

| OSI Model Layer      | TCP/IP Model Layer   | Description                                                                 |
|----------------------|----------------------|-----------------------------------------------------------------------------|
| Application Layer     | Application Layer    | Interfaces directly with end-user applications and provides network services. |
| Presentation Layer    | -                    | Formats and encrypts data for the application layer.                       |
| Session Layer         | -                    | Manages sessions between applications, establishing and terminating connections. |
| Transport Layer       | Transport Layer      | Ensures reliable data transfer, error checking, and flow control.          |
| Network Layer         | Internet Layer       | Handles routing of packets across networks using logical addressing (IP).   |
| Data Link Layer       | Link Layer           | Responsible for node-to-node data transfer and error detection.             |
| Physical Layer        | Network Interface Layer | Transmits raw bits over a physical medium, defining hardware specifications. |

#### Firewalls, IDS and IPS

Firewalls are used to monitor, filter and control the incoming and outgoing internet traffic based on predetermined rule. Different types of firewalls such as NGFW, Perimeter firewall, Software and Hardware based firewalls were discussed.

Intrusion detection systems (IDS) and intrusion prevention systems (IPS) constantly watches the network, identifying possible incidents and logging information about them, stopping the incidents, and reporting them to security administrators.

#### WLAN Vulnerabilities & Attacks

Speaker discussed various WLAN vulnerabilities such as usage of weak encryption protocols (WEP, WPA2, WPA2), poorly configured hardware and default settings that lead to exploitation and unauthorized access.

### Practice with Network Analysis Tools

In the previous sections, we were given theoretical background knowledge of the cyber and the related domains for better understanding the whole picture. In this section, we were guided by Mr. Ali Zain in playing 6 CTFs and this was the fun part of the day for me.

- **`wireshark` — Experienced 6 CTFs:**
Introduced about the `wireshark` — network packets capturing tool. We learnt how to navigate around it, applying filters for different protocols such as **HTTP, TLS, FTP, SMB**, exporting or restructuring files from the packets to the original file. We played 6 CTFs with `wireshark` and gained very value able experience with it. With CTFs, we gain better understanding how to make better use of it for our specific need, analyzing the captured packets, checking for file metadata for hints.

- **`nmap`:** Played with `nmap`, scanned a website to check for the open ports, get operating system information of the server/site. We learned about the `-p`, `-A` flags for specifying ports and getting OS information using `nmap`.

    Discussed the use of `man` for reading software manual and `grep` command to filter out the required information. Applied this knowledge with **linux pipes `|`** and filtered out some information from the `nmap --help` command

- **`nikto`:** is a web vulnerability scanner, scans the website for known or commonly found web vulnerabilities, looks for outdated software versions, misconfigurations and reports them back to the user.

- **`burpsuite`:** acts as an HTTP/S interceptor. It comes with pre-configured chromium browser to **redirect the unencrypted browser traffic to `burpsuite`**, so the user can modify the request according to the needs and forward it. Also intercepts the response, and it can also be modified before sending back to the browser. Comes with features such as repeater (sending same request multiple times with different query parameters for testing), scoping (filtering the traffic view) and HTTP history. Analyzed traffic sent to [Google](https://google.com) server, modified the request headers and verified in the chromium developer tools to verify the request header has been modified.

## Conclusion

The first day of the workshop was engaging and full of knowledge. I appreciate the efforts and hardwork of [IGNITE](https://ignite.org.pk/) and the speakers in arranging and giving lectures in the workshop. Theoretical portion took the majority of the time today, since we all need basic refresher into what we're going to learn, its significance, but the ground work has been done and I'm hoping to do more challenges on the day 2 of the workshop.
