# IS355: Tutorial 4 - Routing, Traceroute, and IP Geolocation

## Task 1: View Routing Table

### PowerShell Commands Executed

First, identify the index (`InterfaceIndex`) of the primary physical adapter:

```powershell
Get-NetAdapter -Physical


! [Get-NetRoute -Interfaceindex](IM/Get-NetRoute -Interfaceindex.png)
