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
## To discover the target VM: an initial command "netdiscover" was used 

1. TCP Connect Scan (-sT)
- Default scan for non‑privileged users
- Full handshake is established
- System scanning is logged; easily detectable

2. TCP Syn Scan (-sS)
- Default scan for privileged users
- A connection is never fully established. (the connection is immediately disconnected as soon as results are recorded)
- System scanning target is logged; not as easily detectable (Tools such as wireshark can make such scans detectable)

**Main Difference between -sT and -sS Scan is as follows:**
![WhatsApp Image 2026-03-01 at 6 59 30 AM](https://github.com/user-attachments/assets/643f4e82-d4c0-4fa1-9184-e860ba7d6988)

3. Decoy Scan (-D)
- Main purpose of this scan is used to confuse the target by hiding the real scanning system among multiple decoy systems (Instead of sending traffic from a single machine, Nmap can generate traffic from several specified or randomly generated decoy addresses)
- The real scanning system will still be included in the list of systems that will attemAggressive Scan (-A)pt to scan the target as it is the only system capable of receiving responses from the target
- Can be combined by different kinds of scans
- Detectable by advance IDS/IPS syetms due to traffic patterns
  
4. Serice Version Detection (-sV)
- Used to find the service version of applications running on open ports. (Helpful as it helps us know if the application is vulnerable to already known attacks related to the version that is detected)
- Uses banner grabbing
  
5. OS Detection Scan (-O)
- It is used to identify the operating system running on the target. (Nmap analyzes response traffic and fingerprints the target to guess its OS.)
- **Special note:** A targets OS can also be roughly guessed by its response to ICMP packets (simply ping the target in command prompt). By checking its TTL value we can come to know if the target is Linux/Unix system (TTL value is around 60) or Windows (TTL value is around 120, double of 60).
  
6. Aggressive Scan (-A)
- This type of scan combines different scan types discussed above such as: Service Detection (-sV), OS Detection (-O) along with script scanning for a combined output of all different scans.
- This type of scan is painfully obvious.
  
7. UDP Scan (-sU)
- This type of scan is used to sccan UDP ports.
- slower than TCP scans as unlike TCP, UDP does not establish a connection hence response traffic is not always guarenteed

8. Ping Scan (-sn)
- This type of scan is used to know if a a host is alive and responsive.
- Does not scan ports of the target
- Good for scanning for alive targets in a network
- Detectable 

9. No Host Discovery Scan (-Pn) 
- This type of scan assumes all targets in a network are alive and hence skips checking whether a target is alive.
- Directly starts scanning ports
- Useful to scan targets which block ICMP or host discovery probes through firewall

## Background information regarding firewall evasion: 
Certain scans are designed to observe how a target system or firewall handles unusual TCP packets. These techniques may help gather additional information about ports that initially appear as filtered during standard scans.
The following scans attempt to bypass basic firewall filtering by sending packets that do not follow the convention. If these scans successfully bypass firewall filtering and produce results, it is important to carefully analyze the reasoning behind the reported port state.
Some ports may appear "open" or "open|filtered" due to a lack of response. This may not always indicate that the service is accessible, as the system or firewall may simply be dropping packets.

**Why Silence Matters When Scanning Filtered Ports**
Understanding silence or lack of response is important when interpreting scan results.
According to TCP behaviour, closed ports typically respond with a "RST" packet when probed. However, ports running certain services or protected by firewall rules may drop unexpected packets instead of responding.
As a result, no response may indicate an open port, filtered port, or packet filtering behaviour, and should be analyzed carefully before drawing conclusions.

**Firewall Evasion Scans** 
10. FIN Scan (-sF)
- In this type of scan Nmap sends probes with the "FIN" flag set to observe how the target responds. (As FIN flag is only expected from hosts that have previously established a connection; unexpected packets with FIN flags can sometimes bypass the firewall)

11. NULL Scan (-sN)
- In this type of scan Nmap sends probes with the "NULL" flag set to observe how the target responds. (As Firewall always expects packets to have some sort of flag set in its header, having no flags can also sometimes bypass the firewall)
  
12. Xmas Scan (-sX)
- This kind of scan is known as Xmas Scan due to its use of unexpected trio of TCP flags set (URG, PSH and FIN). As these packets contain 3 distinct flags rather than a single flag in its header, it sometimes is able to evade the firewall. 
