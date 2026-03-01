# -network-reconnaissance-and-service-enumeration-using-nmap
This repository documents a hands-on network reconnaissance lab focused on scanning a target system to gather information and build a reconnaissance report using Nmap.


**Recon:** short for reconnaissance, is the first step when planning and structuring a successful attack.
It refers to the process of gathering as much information about a target as possible.
This process can be both passive (collecting information about the target without directly interacting with it) and active (collecting information about the target by directly interacting with it).
An example of active reconnaissance includes using various tools to gather information about a target, such as its operating system, open ports, and the services running on those ports.
For the purpose of this document, the tool used to perform reconnaissance will be Nmap.

**What is Nmap:** Nmap is a network scanning and reconnaissance tool that is widely used by penetration testers and security professionals to discover hosts, identify open ports (including the services running on them), enumerate services, and explore various ways to assess or penetrate targets.

##Lab Setup: To demonstrate Nmap and its usability during reconnaissance activities, both Kali Linux and Metasploitable were configured to simulate a lab environment.

**Additional Information Before Conducting Scans:** 
-T Flag: 
The "-T" flag is used to control the speed of an Nmap scan. Nmap allows users to adjust the rate at which packets are sent in order to change the timing of scans. An important note to be aware of is that scan speed is generally inversely proportional to the accuracy and amount of information that can be gathered. Faster scans may produce less reliable or incomplete results, while slower scans tend to provide more accurate information.
The following table provides explanation for various levels of speed along with 
![WhatsApp Image 2026-03-01 at 6 32 07 AM](https://github.com/user-attachments/assets/896c16db-4cb8-425f-8fab-efe61940dcdb)

**Different Kinds of Port States:**
When scanning, Nmap can report a port to be in one of the following states:
- **Open** — The port is actively listening and accepting new connections. A service is running and responding on this port.
- **Closed** — The port is accessible, but no service is listening on it. The target system typically responds with a RST (Reset) packet.
- **Filtered** — The state of the port cannot be determined because a firewall or filtering device is blocking or dropping Nmap’s probes.

**Different kinds of Nmap Scans**
