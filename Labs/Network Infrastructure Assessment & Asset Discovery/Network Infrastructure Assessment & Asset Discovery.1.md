

# Multi-Router Infrastructure Assessment & Asset Discovery

>Nye Wade
>02/05/26
>Spring 26'

##  Project Overview

This project simulates the role of a Network Engineer tasked with mapping an undocumented production IP data network. Using a combination of manual enumeration and automated scanning tools, I reconstructed the physical and logical topology of a multi-router environment. This assessment identifies all active hosts, subnets, and routing paths to ensure comprehensive network visibility and eliminate security "blind spots."

# Lab Environment & Topology

The environment consisted of a Windows-based workstation and a backbone network of three core routing devices:

-   **Workstation:** Windows Server 2019
    
-   **Router 1:** Linux Ubuntu 16 running the Quagga Routing Suite
    
-   **Router 2:** Linux Ubuntu 16 running the Quagga Routing Suite
    
-   **Firewall:** pfSense 2.4 (FreeBSD-based)
    

## Network Topology Diagram

<img width="580" height="263" alt="Screenshot 2026-02-05 at 11 58 55 PM" src="https://github.com/user-attachments/assets/51221e44-631b-4271-8e31-7df7ec40d68d" />

> _This diagram represents the reconstructed network architecture, showing the connection points between the internal Quagga routers and the pfSense edge firewall._

----------

# Phase 1: Local Enumeration and Gateway Discovery

The first objective was to establish a "baseline" by identifying the local host configuration and the path to the default gateway.

1.  **IP Configuration:** Executed `ipconfig` to identify the workstation's IP address (`172.30.0.2`) and default gateway (`172.30.0.1`) on the Student interface.
    
2.  **ARP Cache Analysis:** Used the `arp -a` utility to map the gateway's Layer 2 (MAC) address to its Layer 3 (IP) address.
    
3.  **Connectivity Testing:** Verified communication with the gateway using the `ping` (ICMP) utility.
    
## _Workstation IpConfig._
> ![Assessing Network Infrastructure Lab View](https://github.com/user-attachments/assets/db77feef-2f67-4f34-ae90-50c0f7ef783f)
> _Displays the initial 'ipconfig' output used to identify the local segment and the 172.30.0.1 gateway._
_Workstation ARP._
> ![Assessing Network Infrastructure Lab View (1)](https://github.com/user-attachments/assets/fa0b85c2-5661-49d9-bca2-79a4ee30ce41)
> _Shows the mapping of the default gateway’s IP to its hardware address, confirming local segment connectivity._

----------

# Phase 2: Core Router Interrogation (Quagga)

With the gateway identified, I utilized remote management protocols to gain shell access to the core routers and document internal routing paths.

1.  **Remote Access:** Used **PuTTY** to establish a **Telnet** session (Port 23) to Router 1 and an **SSHv2** session (Port 22) to Router 2.
    
2.  **Shell Elevation:** Accessed the integrated Quagga console shell using the `sudo vtysh` command.
    
3.  **Interface Documentation:** Executed `show interface` to record hardware addresses and CIDR notation subnets for all active physical links (e.g., ens192).
    
4.  **Routing Table Analysis:** Used `show ip route` to identify "directly connected" networks and "via" hops to adjacent routers.
    
## _Router 1: Console Shell._
> ![Assessing Network Infrastructure Lab View (2)](https://github.com/user-attachments/assets/efa998fd-f936-4fd6-ae32-09994d30a5ab)
>  _Explanation: Verification of successful authentication and access to the Quagga VTY shell environment._

## _Router 1: Routing Table._
> ![Assessing Network Infrastructure Lab View (3)](https://github.com/user-attachments/assets/7a432806-b5af-4185-befc-97dbfa8b3af7)
> 
## _Router 1: ARP Table._
> ![Assessing Network Infrastructure Lab View (4)](https://github.com/user-attachments/assets/7533d9d7-7bf9-4618-b40d-5fa308041724)
> 
## _Router 2: Routing Table._
> ![Assessing Network Infrastructure Lab View (5)](https://github.com/user-attachments/assets/7ebd4d2c-0dae-47be-bc08-8d9055f1c95d)
> 
## _Router 2: ARP Table._
> ![Assessing Network Infrastructure Lab View (6)](https://github.com/user-attachments/assets/959776f2-8a77-4d5b-bd25-1841153bdc3e)
> 
> _Explanation: These captures display the routing tables, highlighting the specific paths (K/R codes) used to reach neighboring subnets._

----------

# Phase 3: Edge Security and Firewall Assessment (pfSense)

The final segment of the manual discovery involved the **pfSense** device, which serves as the gateway to the outside world (WAN).

1.  **Web GUI Access:** Logged into the pfSense dashboard via a web browser at `172.21.0.2`.
    
2.  **Interface Mapping:** Documented the WAN uplink on the `vmx3` adapter and internal LAN segments.
    
3.  **Diagnostic Export:** Extracted the firewall's ARP table and routing table to finalize the logical topology map.
    

## _pfSense: Routing Table._
> ![Assessing Network Infrastructure Lab View (8)](https://github.com/user-attachments/assets/2d7b715f-f6bd-436b-8fd9-0985f5140311)
> 
## _pfSense: ARP Table._
> ![Assessing Network Infrastructure Lab View (9)](https://github.com/user-attachments/assets/24c8d432-e2b8-4481-834b-17080377e596)
> 
>  _Demonstrates the configuration of the network edge, showing the WAN interface and current firewall session states._

----------

# Phase 4: Automated Asset Discovery and Reporting

To replace manual methods with more efficient security auditing, I utilized **Nmap/Zenmap** for automated host discovery.

1.  **Ping Scan:** Conducted a scan on the `172.30.0.0/24` subnet to identify all active IP hosts, including those not identified in initial manual tests.
    
2.  **Asset Inventory:** Compiled a formal inventory matching Hostnames to IP and MAC addresses to eliminate "security blind spots."
    
3.  **Professional Deliverable:** Generated an XML-based Nmap report and converted it into a "prettified" HTML format for stakeholder review.
    

![Network Infrastructure Assessment Jones   Bartlett (3)](https://github.com/user-attachments/assets/54ad24c3-1460-4a57-b9b9-e85eb0753ffc)
> _A finalized spreadsheet documenting every discovered host across the organization's backbone segments._

## _Converted Nmap Output into an HTML Report._ 
> ```javascript
> "nmap -O -oX C:\\Users\\Administrator\\Desktop\\networkReport.xml"
> 172.20,21,22,30,31,40.0.0/24
 
> _The command used in Nmap to generate the XML output file._


> ![Converted Nmap Output to HTML](https://github.com/user-attachments/assets/4d5dfa02-66ad-4fe3-b03f-b1e7e5b8208f)
> _The final technical deliverable showing a high-level summary of active services and security findings for management._

----------

# Skills Demonstrated

-   **Infrastructure Reconnaissance:** Mapping undocumented multi-vendor environments.
    
-   **Routing & Switching:** Analyzing IP routing tables, ARP caches, and CIDR notation.
    
-   **Network Security Tools:** Proficient use of Nmap/Zenmap, pfSense, and PuTTY (SSH/Telnet).
    
-   **Technical Documentation:** Translating raw command-line data into professional asset reports and topology maps.


<!--stackedit_data:
eyJoaXN0b3J5IjpbMjgxMDI3OTA4LDE1MjIyODY3NzcsMTI2Mz
kxMjgxN119
-->