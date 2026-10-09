# Network Engineer Portfolio 

# Lab 1: OSI Model Breakdown via Wireshark (HTTP Packet Capture) 

## **Objective** 

To deepen my understanding of the OSI Model by capturing and analysing a real HTTP packet using Wireshark. This hands-on approach allows me to visualize the abstract concepts I learned in theory. 

## **Packet Capture Context** 

- **Capture Interface** : WiFi 

- **Website Accessed** : http://neverssl.com 

- **Filter Used in Wireshark** : http 

- 1) Opened Wireshark and began a live capture on the active Wi-Fi interface. 

- 2) Visited http:// neverssl.com in a web browser to generate HTTP traffic. 

- 3) Stopped the capture and filtered for packets using port **80** (HTTP). 

- 4) Analysed a single HTTP GET packet and reviewed protocol breakdown across OSI layers. 

## **Captured Packet Breakdown** 

Below is a breakdown of a single HTTP packet and how it maps to each OSI layer: 

|**Wireshark**<br>**Layer**|**OSI Layer**|**Description**|
|---|---|---|
|Frame|Layer 1 –<br>Physical|**Represents raw transmission of bits over the network**<br>**interface**<br>Frame 22331: 501 bytes on wire (4008 bits), 501 bytes captured<br>(4008 bits) on interface \Device\NPF_{DE51CB05-44BD-4934-<br>A0E1-E74DA3A6A843}, id 0|
|||**Contains MAC addresses used for local network delivery**|
|Ethernet II|Layer 2 –<br>Data Link|Ethernet II, Src: Intel_fe:d0:21 (5c:e4:2a:fe:d0:21), Dst:<br>SkyUk_60:dc:01 (b4:ba:9d:60:dc:01)|
|||**Handles logical addressing and routing via IP addresses**|
|Internet<br>Protocol Version<br>4|<br>Layer 3 –<br>Network|Internet Protocol Version 6, Src:<br>2a06:5904:1a06:c000:7c70:4253:fdc4:691d, Dst:<br>2600:1f13:37c:1400:ba21:7165:5fc7:736e|



|**Wireshark**<br>**Layer**|**OSI Layer**|**Description**|
|---|---|---|
|||**Manages end-to-end delivery, error checking, and flow**<br>**control (via TCP)**|
|Transmission<br>Control Protocol|Layer 4 –<br>Transport|Transmission Control Protocol, Src Port: 59164, Dst Port: 80,<br>Seq: 1, Ack: 1, Len: 427|
|Hypertext<br>Transfer<br>Protocol|Layer 7 –<br>Application|**Contains the actual data (e.g., an HTTP GET request)**<br>**requested by the user**<br>Hypertext Transfer Protocol|



## **Reflection** 

This lab helped me _see_ how each OSI layer plays a part in delivering data from a server to my browser. It was especially rewarding to: 

- See real HTTP headers (Layer 7) in plaintext 

- Understand how each lower layer works together to ensure smooth delivery 

- Recognize how abstract concepts (like "transport" or "routing") show up in real data 

I now feel more confident about the OSI Model, especially Layers 2–4, which I used to find difficult to differentiate. I still need to revisit Session (Layer 5) and Presentation (Layer 6), which were not clearly visible in this packet — I plan to look at HTTPS traffic next to explore TLS handshakes and encryption. 

# Lab 1 Reflection – Technical Breakdown 

What happens when you visit a website? 

From the moment you type www.youtube.com and press Enter what actually happens? 

- 1) User Input and DNS Resolution - **Layer 7 Application** 

The browser checks the DNC cache – this is to check for the IP address of youtube.com. If it is not found, it sends a DNS query (UDP port 53) to resolve youtube.com into an IP address (moves up the DNS chain from the device, to the router, to the external DNS servers. 

OSI Model Explanation: DNS is an application-layer protocol. User interacts with browser here. 

- 2) TCP Handshake (3-Way) - **Layer 4 Transport** 

Client sends SYN, server replies ACK, client responds with ACK. This establishes a reliable connection  - usually to port 443 for HTTPS 

OSI Model Explanation: TCP lives here — handles reliable communication, SYN/SYNACK/ACK exchange. 

## 3) TLS Handshake (if HTTPS) - **Layer 5-6 Session/Presentation** 

Secure keys are exchanged and encrypted session begins. This is to ensure that no one can intercept the network. 

OSI Model Explanation: TLS negotiates secure session (Session) and encryption formatting (Presentation) 

- 4) HTTP Request sent – **Lauer 7 Application** 

A GET request is sent over TCP, via port 443 or 80 for HTTP (This is the “send me the page part” – client asks the server for the page) 

OSI Model Explanation: HTTP is an application-layer protocol. The browser sends the GET request. 

- 5) Server responds – **Layer 3-2-1** 

Sends back HTML, CSS, JS, JS, images, video metadata etc. 

Packets travel back through the OSI layers, hitting each relevant protocol 

OSI Model Explanation: Packets travel back via Network (IP), Data Link (MAC/Ethernet), Physical (bits) 

- 6) Browser renders the page - **Layer 7 Application** 

Data is interpreted and displayed; you see the YouTube homepage fully loaded 

OSI Model Explanation: Browser interprets and displays the content — pure Application Layer 

# Lab 2 – VLSM Lab 

## **Scenario: Departmental Subnetting** 

You are assigned the 192.168.10.0/24 network. 

You must divide it using **Variable Length Subnet Masking** to meet the following requirements: 

## **Department Required Usable Hosts** 

HR 60 Engineering 30 Sales 14 Management 2 

For each department, determine: 

- Network address 

- Subnet mask (CIDR and dotted decimal) 

- First and last usable IP 

- Broadcast address 

**HR:** 2^6 = 64 6 host bits – 32-6= /26 (CIDR) Subnet Mask 255.255.255.192 Total IPs = 64 Useable Ips = 62 **Engineering:** 2^5 = 32 – 5 host bits – 32-5= /27 

**Sales:** 2^4= 16 – 4 host bits – 32-4= /28 

**Management:** 2^2=4 – 2 host bits – 32-2= /30 

|**Department**|**Network**<br>**Address**|**CIDR**<br>**Subnet Mask**|**First Usable IP**|**Last Usable IP**|**Broadcast Address**|
|---|---|---|---|---|---|
|HR|192.168.10.0|/26 255.255.255.192|192.168.10.1|192.168.10.62|192.168.10.63|
|Engineering|192.168.10.64|/27 255.255.255.240|192.168.10.65|192.168.10.94|192.168.10.95|
|Sales|192.168.10.96|/28 255.255.255.248|192.168.10.97|192.168.10.110|192.168.10.111|
|Management|192.168.10.112|/30 255.255.255.252|192.168.10.113|192.168.10.114|192.168.10.115|



# Lab 2 Reflection – Technical Breakdown 

## **What is Subnetting?** 

Subnetting is the process of dividing a larger IP network into smaller, more manageable segments called subnets. This allows network administrators to organize devices efficiently, improve security, and reduce unnecessary broadcast traffic. Each subnet operates as a distinct network, even though they all come from a single original range. 

## **Why is Subnetting Important?** 

In modern networks, IP addresses are a limited resource. Subnetting helps: 

- Prevent wasted IPs by allocating only what’s needed 

- Segment traffic for performance and security 

- Simplify troubleshooting by creating logical groups of devices 

- Support growth by structuring the network into scalable blocks 

## Classful vs Classless Addressing 

In the past, IP addresses were grouped by classes (A, B, C), with fixed subnet masks. This method wasted a lot of addresses and lacked flexibility. 

Modern networks use CIDR (Classless Inter-Domain Routing), which allows subnetting based on how many addresses you actually need. Instead of using a fixed class, CIDR uses slash notation (e.g. /26) to show how many bits are used for the network portion of the address. 

Subnetting in Practice (CIDR & Usable Hosts) 

For example, a /26 subnet has: 

- 64 total IP addresses 

- 62 usable host addresses (2 are reserved for network and broadcast) 

- Subnet mask: 255.255.255.192 

By choosing the right CIDR, you avoid over-allocating addresses and gain much tighter control of your network structure. 

## Example Subnet Plan (VLSM) 

Given the network 192.168.10.0/24 and the following department needs: 

Department Usable IPs CIDR Subnet Mask Range 

|HR|60|/26|255.255.255.192 192.168.10.1 – .62|
|---|---|---|---|
|Engineering|30|/27|255.255.255.224 192.168.10.65 – .94|
|Sales|14|/28|255.255.255.240 192.168.10.97 – .110|
|Management|2|/30|255.255.255.252 192.168.10.113 – .114|



This method is called VLSM (Variable Length Subnet Masking) — it lets you assign subnets of different sizes based on the exact number of hosts needed. It’s more efficient than using equalsize subnets for everyone. 

## **Summary** 

Subnetting is a core networking skill that transforms how networks are designed, scaled, and secured. With CIDR and VLSM, you gain precision, efficiency, and full control over your IP space. 

# Lab 3 – Subnetting and IP Addressing 

## **Lab Write-up: Troubleshooting and Resolving IP Conflicts** 

## **Objective:** 

To simulate an IP conflict scenario by assigning the same IP address to two devices on the same network and resolving the conflict using SSH for remote management. 

## **Steps Taken:** 

1. **Initial Setup:** 

   - Connected **PC0** and **PC1** to the same network (via a Cisco 2960 switch) with the same **static IP address** (192.168.1.10) and **subnet mask** (255.255.255.0). 

   - Ensured both PCs were configured with the same IP in the same subnet to intentionally create an **IP conflict** . 

2. **Identifying the IP Conflict:** 

   - Ran **ping tests** from both PCs to the **switch** (IP: 192.168.1.1). 

   - Observed **packet loss** (1 packet lost out of 4) when pinging from **PC1** , while **PC0** was able to ping the switch successfully. 

   - Conclusion: The IP conflict between PC0 and PC1 was causing network instability, with only one device responding at times. 

3. **Accessing the Switch via SSH:** 

   - Used the **console cable** to access the Cisco 2960 switch via SSH, having configured the switch's IP address (192.168.1.10) and ensured SSH was enabled earlier. 

- Confirmed the switch’s **MAC address table** using the command: 

bash 

show mac address-table 

This showed that both PCs were associated with the same IP address (192.168.1.10), further confirming the IP conflict. 

4. **Resolving the IP Conflict:** 

   - Changed the **IP address of PC1** from 192.168.1.10 to 192.168.1.11 to ensure both devices had **unique IP addresses** . 

   - Verified connectivity from **both PCs** : 

      - Pinging the switch from both **PC0** (192.168.1.10) and **PC1** (192.168.1.11) resulted in **no packet loss** . 

      - Both devices successfully communicated with the switch without network issues. 

5. **Confirming the Fix and Testing:** 

   - Re-ran the **ping test** from both PCs to ensure stable connectivity. 

   - No packet loss occurred, confirming the IP conflict was fully resolved. 

   - Saved the switch configuration with: 

bash 

write memory 

## **Outcome:** 

- Successfully identified and resolved the **IP conflict** between PC0 and PC1 by ensuring each device had a unique IP address. 

- Demonstrated the use of **SSH** for remote management of the switch to monitor the network and troubleshoot issues. 

- The network became stable with **no packet loss** , confirming that the conflict was the source of the issue. 

Lab 4 – VLANs and Trunking with Cisco 2960 Home Lab 

## **Objective:** 

To create VLANs, assign access ports, configure trunking between switches, and verify MAC address learning and VLAN isolation using real Cisco 2960 hardware. 

**Purpose:** The purpose of creating a VLAN is to logically segment a network into isolated broadcast domains, improving security, traffic management, and network efficiency without needing separate physical switches. 

**Context:** In a school, VLANs are useful because they can separate network traffic—for example, isolating student devices, teacher systems, admin offices, and guest Wi-Fi—so each group has its own secure, controlled environment without interference or access to each other’s resources. 

## **<u>Set Hostnames</u>** 

Renamed to Switch1 

_Bash_ 

Switch> enable Switch _# configure terminal_ 

Switch(config) _# hostname Switch1_ 

## **Create VLANs** 

Create VLANs 10 (STAFF) and VLAN 20 (STUDENTS): 

Keynote: 

Here I was creating a VLAN 10 and 20 both switches. While doing this with the first Switch. I was using the correct interface name – the switch is case sensitive. 

Used the above command to find the correct names of the Interfaces. 

## **<u>Assign Access Ports</u>** 

Assigned GigabitEthernet1/0/24 to VLAN 10 as access port 

## **<u>Configure Trunk Port</u>** 

Configured trunk on GigabitEthernet1/0/1 

Keynote: 

Here I attempted to assign an access port. However, my Cisco 2960 Switches do not support switchport trunk encapsulation dot1q as it only supports  dot1q, as seen above. 

So I left out that line. Actual input: 

_bash_ 

configure terminal 

interface GigabitEthernet1/0/24 

switchport mode trunk 

Output which PuTTY displayed after 

Had to press Enter a few times before I could access the Switch1 to _show interface trunk_ . 

The Enter input showed the active and working interfaces. Here is a quick summary: 

## **Active and Working Interfaces** 

- **GigabitEthernet1/0/1** and **GigabitEthernet1/0/24** 

   - Status: **Up** 

   - Line protocol: **Up** 

   - Traffic is being received and transmitted (packets in/out). 

   - No errors reported. 

   - These interfaces are **live and connected** . 

## **Down or Not Connected Interfaces** 

- **Vlan1** 

   - Status: **Up** 

   - Line protocol: **Up** 

   - Indicates that the VLAN interface is operational. Often used for management access. 

- **FastEthernet0** 

   - Status: **Down** 

   - Line protocol: **Down** 

   - Not connected or administratively shut down. Not passing any traffic. 

- 

## **GigabitEthernet1/0/2 and GigabitEthernet1/0/3** 

- Status: **Down (notconnect)** 

- Line protocol: **Down** 

- No devices connected to these ports, or cables might be unplugged. 

## **Config/Actions Taken** 

- GigabitEthernet1/0/24 was configured as a **trunk port** (switchport mode trunk), then brought **up** . 

- show interfaces and show int vlan1 were used to verify status and traffic stats. 

## **Key Insights** 

- Your trunk port (G1/0/24) is **functioning correctly** and is up. 

- Some interfaces are **down simply due to nothing being plugged in** . 

- You’re not seeing any errors or drops on the active ports — this is a good sign. 

# Lab 4 Continued 

## **Configured Switch 2** 

- Renamed to Switch2 

- Created VLANs 10 and 20 to match 

- Assigned GigabitEthernet1/0/2 to VLAN 20 

- Configured trunk on GigabitEthernet1/0/1 

## **PC IP Assignment** 

- PC0 (VLAN 10): 192.168.10.10 /24 

- PC1 (VLAN 20): 192.168.20.10 /24 

- IPs assigned manually via Windows adapter settings 

## **Test: Ping** 

- Ping from PC0 to PC1: **Failed** (expected due to VLAN isolation) 

- Ping from PC0 to another MAC in VLAN 20 (via trunk): **Successful** 

- Ping from PC1 to VLAN 20 (itself): **Successful** 

## **Verified VLANs for both Switches** 

## **Verified Trunks for both Switches** 

## **MAC Address Table** 

This shows dynamic MAC entries associated with their ports/VLANs. As you can see there are a different number of MAC addresses across both Switches so I will verify to see if VLAN 20 is configured correctly on Switch 2. 

Confirmation: Verification successful. 

- Ran `show mac address-table` on both switches 

- **Switch1:** 25 MAC addresses 

 **Switch2:** 23 MAC addresses 

  Verified dynamic learning on: 

   - `Gi1/0/24` (access ports) 

   - `Gi1/0/1` (trunk port) 

- Differences explained by traffic patterns and MAC aging 

Reasoning for why there are different numbers on the mac address-tables between switches 

# Lab 5: Lab Name: Basic Switch & PC Connectivity 

## **Tool Used: Cisco Packet Tracer** 

## **Objective** : 

To demonstrate an understanding of switch setup, interface configuration, and PC-to-PC communication within a local area network (LAN). 

## **Steps Taken** : 

1. Deployed two 2960 switches and connected them using a crossover cable. 

2. Accessed the CLI on each switch and entered configuration mode to assign hostnames and prepare interfaces. 

3. Added two PCs and connected them to each switch using straight-through cables. 

4. Assigned IP addresses manually to the PCs in the same subnet (e.g., 192.168.10.1 and 192.168.10.2). 

5. Verified connectivity using the ping command. Successful replies confirmed proper setup. 

6. Explored tracert to understand packet flow (optional). 

## **Outcome** : 

- Devices on the same network were able to communicate. 

- Gained hands-on experience with CLI navigation, basic switch commands, and packet testing. 

## Today I: 

- 1) Connected two PCs (end devices) with a copper straight-through cable to a switch each PC0 > FastEthernet0/1 on Switch 0 PC1 > FastEthernet0/24 on Switch 1 

Used a copper crossover cable to connect the two Switches together Connect0 > FastEthernet0/24 to Switch1 > FastEthernet 0/24 2) Configured Switch 0 and Switch 1 enable configure terminal vlan 10 name Sales exit interface fastethernet0/1 switchport mode access switchport access vlan 10 exit interface fastethernet0/24 switchport mode trunk exit end write memory 3) Assigned Ips to PCs PC0 → Desktop → IP Configuration IP: 192.168.10.1 Subnet: 255.255.255.0 PC1 → Desktop → IP Configuration IP: 192.168.10.2 Subnet: 255.255.255.0 4) Tested connectivity Clicked on PC0 > Desktop > Command Prompt Run: 

Ping 192.168.10.2 

Self-Reflection: I initially had no clue what I was doing, but by breaking each step down, I successfully completed my first Packet Tracer lab and understood the logic behind basic network connectivity. This has made VLANs and switch configuration much less “intimidating.” 

# **Lab 6** : MAC Address Table Lab Using Cisco Packet Tracer 

## **Objective** : 

To demonstrate the learning behaviour of a switch and how MAC address tables are populated during device communication. 

## **Steps Taken** : 

1. Connected two PCs to a switch using straight-through Ethernet cables. 

2. Assigned static IPs: 

   - PC0: 192.168.1.1 /24 

   - PC1: 192.168.1.2 /24 

3. Verified connectivity using ping. 

4. Cleared the MAC address table (clear mac address-table dynamic). 

5. Re-ran ping to trigger learning. 

6. Used show mac address-table to verify MACs and ports were dynamically learned. 

## **Outcome** : 

The switch successfully learned the MAC addresses of the connected devices and recorded them in its MAC address table. This confirms that Layer 2 switching works as expected. 

## **Reflection** : 

Before this lab, I understood MAC addresses in theory. Now, I’ve seen them dynamically populate the switch's table in real-time, making the concept click. This exercise helped me connect command-line interaction to actual network behaviour. 

# Lab 7: Understanding Physical Network Infrastructure 

## **1. What I Set Up:** 

I simulated a basic network topology in Packet Tracer, connecting end devices to switches and interconnecting the switches to reflect a real-world rack-and-patch-panel setup. 

## **2. What I Learned:** 

- The difference between copper straight-through and crossover cables. 

- The importance of proper cable selection and port configuration. 

- How racks and patch panels relate conceptually to how devices are wired in a data center. 

## **3. Real-World Insight:** 

In actual physical setups, cables are routed through patch panels and cable trays. Though not shown in Packet Tracer, understanding these helps visualize how cleaner, organized networking environments are structured. 

## **4. Reflection:** 

This lab helped bridge the gap between virtual setups and physical infrastructure. I’m starting to appreciate the hands-on, practical aspect of networking more and can now picture what setting up a small office network might look like. 

# Lab 8 Portfolio Entry – Troubleshooting & Monitoring 

## **Focus Area** 

Network troubleshooting using ping, tracert, logs, baselining, and jitter observation. 

## **Lab Summary** 

**Objective** : Observe how network behavior changes under different conditions using ping and traceroute. 

## **Tools Used** : 

- Windows Command Prompt 

- ping, tracert commands 

- Google DNS (8.8.8.8) as target 

- 

## **Part 1: Normal Conditions** 

- **Command** : ping 8.8.8.8 -n 10 

- **Results** : All packets received, round-trip times consistent (e.g. 7ms–9ms), 0% packet loss. 

- **Insight** : This established the **baseline performance** of the network — stable and jitterfree. 

## **Part 2: Disrupted Conditions** 

- **Command** : ping 8.8.8.8 -n 10 

- **Results** : 

   - 7x General failure. 

   - 3x Request timed out. 

   - 100% packet loss. 

**Insight** : 

- **General failure** suggests the issue was local (adapter disabled, DNS broken, gateway misconfigured). 

- **Request timed out** shows packets left the device but didn’t receive a response, indicating **path disruption** . 

- This shows how **jitter** and **loss** can signal specific layers of failure (local vs external). 

## _Reflection_ 

At first, I was uncertain about what jitter or request timeouts meant. By comparing a clean connection to a failed one, I now clearly understand how ping helps **troubleshoot connectivity** and identify **which part of the network is failing** . This hands-on use of the ping command helped tie together concepts from the first 5 weeks — from layers to routing to switching. 

I reviewed BitLocker settings on my Windows host and noted how it encrypts the entire drive. I also explored recovery key storage options, which are crucial for secure but recoverable encryption.” 

Simulate MFA on a Test Account 

Situation: Needed to secure a Google test account. 

Task: Set up MFA and test cross-device login. 

Action: Created a new account, enabled 2-step verification via Authenticator, tested sign-in from Ubuntu VM. 

Result: Successfully secured login; MFA prompt appeared as expected, showing added protection. 

“In a lab, I configured a test Microsoft account with MFA. I demonstrated logging in from a secondary device, where I had to approve the sign-in via an authenticator app. This reinforced the value of MFA in preventing unauthorized access even if a password is compromised.” 

I simulated managing a printer by adding a virtual PDF printer and sending a print job from a secondary environment. This taught me about print queue monitoring and device management without needing physical hardware. 

With my current device I configured Windows Hello PIN sign-in on my host system and verified successful login. I could then explain how Hello provides faster, more secure authentication compared to traditional passwords. 

## Testing reachability across two LANs 

I built a Packet Tracer lab titled _Testing Reachability Across Two LANs_ . The goal was to configure two subnets — one /24 and one /17 — and use a router to allow communication between them. I validated connectivity with pings, explained the ARP process, and documented the results. 

The first ping from PC0 to PC2 failed due to the PC on LAN1 trying to communicate to something (PC2) through its gateway. I understand that doing a ping for the first time triggers ARP 

