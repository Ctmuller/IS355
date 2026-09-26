# Tutorial 1: System Information with Get-ComputerInfo

This tutorial uses the PowerShell `Get-ComputerInfo` cmdlet to pull processor and memory details for the local machine, filtering the output with the `-Property` parameter.

## Task 1: Computer Processor

![Processor Info](IM/TrueProc.png)

The command `Get-ComputerInfo -Property "*Processors*"` filters the output to only properties containing "Processors". The results show the machine has **1 physical processor** — an Intel(R) Xeon(R) Platinum 8259CL CPU @ 2.50GHz — which exposes **8 logical processors** to the operating system, meaning the OS can schedule work across 8 threads simultaneously.
### Summary Table
| Metric | Value |
| :--- | :--- |
| **Physical Processors (`CsNumberOfProcessors`)** | 1 |
| **Logical Processors (`CsNumberOfLogicalProcessors`)** | 8 |
| **CPU Model (`CsProcessors`)** | Intel(R) Xeon(R) Platinum 8259CL CPU @ 2.50GHz |

### Technical Analysis & Insights
- **Multi-Threading / SMT:** The presence of 8 logical processors on a single physical CPU indicates hyper-threading/simultaneous multithreading capabilities, allowing the OS scheduler to manage up to 8 threads concurrently.
- **Environment Detection:** The Intel Xeon Platinum processor model is standard across AWS EC2 cloud instances (such as t3/c5 memory-optimized instances), confirming this lab runs within a virtualized cloud environment.

## Task 2: Computer Memory

![Memory Info](memory.png)

The command `Get-ComputerInfo -Property "*memory*"` filters the output to properties containing "memory", providing a breakdown of the system's RAM and virtual memory:

- **CsTotalPhysicalMemory / CsPhysicallyInstalledMemory:** Total physical RAM installed (~32 GB).
- **OsTotalVisibleMemorySize / OsFreePhysicalMemory:** RAM visible to the operating system and currently available (~27 GB free).
- **OsTotalVirtualMemorySize / OsFreeVirtualMemory / OsInUseVirtualMemory:** Virtual memory metrics combining physical RAM and pagefile allocation (total, free, and in-use).
- **OsMaxProcessMemorySize:** Maximum addressable memory space for a single 64-bit user-mode process (128 TB).

### Memory Breakdown & Byte Conversions
- **Total Installed RAM (`CsTotalPhysicalMemory`):** 33,983,848,448 Bytes ≈ **31.65 GB**
- **Free Physical Memory (`OsFreePhysicalMemory`):** 27,241,120 KB ≈ **25.98 GB** free (~82% available capacity)
- **64-Bit Architecture Limit:** The 128 TB limit on `OsMaxProcessMemorySize` confirms the system is running a standard modern 64-bit Windows OS kernel.
