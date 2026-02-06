
# Multi-Router Infrastructure Assessment & Asset Discovery

##  Project Overview

This project simulates the role of a Network Engineer tasked with mapping an undocumented production IP data network. Using a combination of manual enumeration and automated scanning tools, I reconstructed the physical and logical topology of a multi-router environment. This assessment identifies all active hosts, subnets, and routing paths to ensure comprehensive network visibility and eliminate security "blind spots."

## Lab Environment & Topology
```javascript
 nmap -O -oX "C:\\Users\\Administrator\\Desktop\\networkReport.xml" 172.20,21,22,30,31,40.0.0/24 ```
The environment consisted of a Windows-based workstation and a backbone network of three core routing devices:

-   **Workstation:** Windows Server 2019
    
-   **Router 1:** Linux Ubuntu 16 running the Quagga Routing Suite
    
-   **Router 2:** Linux Ubuntu 16 running the Quagga Routing Suite
    
-   **Firewall:** pfSense 2.4 (FreeBSD-based)
    

### Network Topology Diagram

> **[ TOPOLOGY ]** > _This diagram represents the reconstructed network architecture, showing the connection points between the internal Quagga routers and the pfSense edge firewall._

----------

## Phase 1: Local Enumeration and Gateway Discovery

The first objective was to establish a "baseline" by identifying the local host configuration and the path to the default gateway.

1.  **IP Configuration:** Executed `ipconfig` to identify the workstation's IP address (`172.30.0.2`) and default gateway (`172.30.0.1`) on the Student interface.
    
2.  **ARP Cache Analysis:** Used the `arp -a` utility to map the gateway's Layer 2 (MAC) address to its Layer 3 (IP) address.
    
3.  **Connectivity Testing:** Verified communication with the gateway using the `ping` (ICMP) utility.
    

> **[ SCREENSHOT: vWorkstation IP Configuration]** > _Displays the initial 'ipconfig' output used to identify the local segment and the 172.30.0.1 gateway._

> **[ SCREENSHOT: vWorkstation ARP Cache]** > _Shows the mapping of the default gateway’s IP to its hardware address, confirming local segment connectivity._

----------

## Phase 2: Core Router Interrogation (Quagga)

With the gateway identified, I utilized remote management protocols to gain shell access to the core routers and document internal routing paths.

1.  **Remote Access:** Used **PuTTY** to establish a **Telnet** session (Port 23) to Router 1 and an **SSHv2** session (Port 22) to Router 2.
    
2.  **Shell Elevation:** Accessed the integrated Quagga console shell using the `sudo vtysh` command.
    
3.  **Interface Documentation:** Executed `show interface` to record hardware addresses and CIDR notation subnets for all active physical links (e.g., ens192).
    
4.  **Routing Table Analysis:** Used `show ip route` to identify "directly connected" networks and "via" hops to adjacent routers.
    

> **[ SCREENSHOT: Router1 Console Shell]** > _Explanation: Verification of successful authentication and access to the Quagga VTY shell environment._

> **[ SCREENSHOT: IP Routes for Router 1 & Router 2]** > _Explanation: These captures display the routing tables, highlighting the specific paths (K/R codes) used to reach neighboring subnets._

----------

## Phase 3: Edge Security and Firewall Assessment (pfSense)

The final segment of the manual discovery involved the **pfSense** device, which serves as the gateway to the outside world (WAN).

1.  **Web GUI Access:** Logged into the pfSense dashboard via a web browser at `172.21.0.2`.
    
2.  **Interface Mapping:** Documented the WAN uplink on the `vmx3` adapter and internal LAN segments.
    
3.  **Diagnostic Export:** Extracted the firewall's ARP table and routing table to finalize the logical topology map.
    

> **[ SCREENSHOT: pfSense IP Routes & ARP Table]** > _Demonstrates the configuration of the network edge, showing the WAN interface and current firewall session states._

----------

## Phase 4: Automated Asset Discovery and Reporting

To replace manual methods with more efficient security auditing, I utilized **Nmap/Zenmap** for automated host discovery.

1.  **Ping Scan:** Conducted a scan on the `172.30.0.0/24` subnet to identify all active IP hosts, including those not identified in initial manual tests.
    
2.  **Asset Inventory:** Compiled a formal inventory matching Hostnames to IP and MAC addresses to eliminate "security blind spots."
    
3.  **Professional Deliverable:** Generated an XML-based Nmap report and converted it into a "prettified" HTML format for stakeholder review.
    

> **[ SCREENSHOT: Completed IT Asset Inventory]** > _A finalized spreadsheet documenting every discovered host across the organization's backbone segments._

> **[ SCREENSHOT: Nmap HTML Report]** > _The final technical deliverable showing a high-level summary of active services and security findings for management._

----------

## Skills Demonstrated

-   **Infrastructure Reconnaissance:** Mapping undocumented multi-vendor environments.
    
-   **Routing & Switching:** Analyzing IP routing tables, ARP caches, and CIDR notation.
    
-   **Network Security Tools:** Proficient use of Nmap/Zenmap, pfSense, and PuTTY (SSH/Telnet).
    
-   **Technical Documentation:** Translating raw command-line data into professional asset reports and topology maps.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTAyODc0MTQ0OCwxMjYzOTEyODE3XX0=
-->