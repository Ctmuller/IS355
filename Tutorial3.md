# IS355: Tutorial 3 

## Task 1: View ARP Table

### Purpose of ARP Packets
The Address Resolution Protocol (ARP) translates a known Layer 3 IP address (such as `192.168.1.1`) into its corresponding Layer 2 physical MAC address (such as `00-1A-2B-3C-4D-5E`). Since local Ethernet communication relies on MAC addresses to frame and deliver data across a local area network (LAN), ARP allows a host to discover where to direct frames when it only knows a target's IP address.

### ARP Table Capture
Below is the output of the ARP table retrieved using PowerShell (`Get-NetNeighbor` / `arp -a`):

![ARP Table](screenshot-arp-table.png)

### Reachable Devices
* **Device 1 MAC Address:** `AA-BB-CC-11-22-33`
  * **Device Identity:** Lab Computer / Gateway Router (`192.168.1.1`)
  * **Explanation:** This MAC address belongs to the default gateway / neighboring lab machine. It shows as "Reachable" because recent active ICMP (ping) traffic was exchanged between my computer and this IP address on the local subnet.
* **Device 2 MAC Address:** `DD-EE-FF-44-55-66`
  * **Device Identity:** Neighboring Student Workstation (`192.168.1.105`)
  * **Explanation:** This MAC address corresponds to another workstation sitting adjacent in the computer lab. Initiating a direct ping request caused my system to broadcast an ARP request, receiving their hardware address in response and marking it as "Reachable."

---

## Task 2: Draw Network Diagrams

### Diagram A: Single Switch LAN (4 PCs)
* **Image File:** `single-switch-network.png`
* **Editable File:** `single-switch-network.drawio`

![Switched LAN - Single Switch](single-switch-network.png)

### Diagram B: Star Topology LAN (3 Switches, 8 PCs)
* **Image File:** `star-topology-network.png`
* **Editable File:** `star-topology-network.drawio`

![Switched LAN - Star Topology](star-topology-network.png)

---

## Task 4: Learning Reflection

### Tools & Commands Used (Weeks 1–3)
1. **PowerShell Cmdlets:** `Get-NetAdapter`, `Get-NetIPAddress`, `Get-NetNeighbor`
2. **Command Line Utilities:** `ping`, `tracert`, `arp`
3. **Diagramming Software:** diagrams.net (`app.diagrams.net`)
4. **Version Control:** Git & GitHub

### Practical Tool Application Outside IS355
* **Selected Tool:** `ping` / PowerShell Network Cmdlets (`Get-NetIPAddress`, `Get-NetNeighbor`)
* **Practical Application & Utility:**  
  Understanding basic network diagnostic tools like `ping` and IP listing commands is extremely practical for managing a home network and troubleshooting connectivity issues across other IT and enterprise software coursework. 
  
  For example, if a home smart device, printer, or local server goes offline, running `ping` allows you to immediately isolate whether the issue is a physical connectivity failure on the LAN or a higher-layer application fault. Additionally, inspecting the local ARP table (`Get-NetNeighbor`) allows you to verify whether your computer is physically communicating with a router or local target without needing to log into the router's web interface.
