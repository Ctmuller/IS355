# IS355: Tutorial 4 - Routing, Traceroute, and IP Geolocation

## Task 1: View Routing Table

### PowerShell Commands Executed

First, identify the index (`InterfaceIndex`) of the primary physical adapter:

![Get-NetRoute -Interfaceindex](IM/GetNetRoute)

| DestinationPrefix | NextHop | RouteMetric | Detailed Description |
| :--- | :--- | :--- | :--- |
| **`0.0.0.0/0`** | `172.16.144.1` | `0` | **Default Gateway Route:** Matches any IP destination outside the local network. All outbound Internet/external traffic is forwarded to the default gateway address `172.16.144.1`. |
| **`172.16.144.0/22`** | `0.0.0.0` (On-Link) | `256` | **Local Subnet Route:** Specifies that all destination IP addresses within the `/22` subnet (`172.16.144.1` – `172.16.147.254`) reside on the directly connected local network link and do not require router forwarding. |
| **`172.16.146.211/32`** | `0.0.0.0` (On-Link) | `256` | **Primary Host Interface Route:** Represents the exact IPv4 address assigned to this computer's primary network card (`/32` host mask). Traffic sent to this address is handled internally by the local network stack. |
| **`172.16.147.255/32`** | `0.0.0.0` (On-Link) | `256` | **Subnet Directed Broadcast Route:** Directs local broadcast packets across the entire `/22` subnet segment. |
| **`169.254.169.x/32`** | `172.16.144.1` | `15` | **Virtual Environment Service Routes:** Specific link-local endpoint routes (e.g., `169.254.169.254`) used by the Apporto cloud infrastructure to communicate with internal metadata services via the gateway. |
| **`224.0.0.0/4`** | `0.0.0.0` (On-Link) | `256` | **Multicast Address Range:** Reserved for IPv4 multicast communication (`224.0.0.0` through `239.255.255.255`) across local subscribing devices. |
| **`255.255.255.255/32`** | `0.0.0.0` (On-Link) | `256` | **Limited Broadcast Route:** Directs local broadcast messages to all nodes on the immediate physical network link. |



