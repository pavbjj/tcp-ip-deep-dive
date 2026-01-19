# Network Protocol Headers (TCP, UDP, IP)

This document contains Mermaid diagrams illustrating the structure and field sizes
of **TCP**, **UDP**, and **IPv4** headers.  
All diagrams are compatible with **GitHub Markdown Mermaid rendering**.

---

## TCP Header

**Minimum size:** 20 bytes (160 bits)  
**Maximum size:** 60 bytes (with options)

```mermaid
graph TB
    style TCP_Header fill:none,stroke:none

    subgraph TCP_Header["TCP Header"]
        direction TB
        R1["| Source Port (16 bits) | Destination Port (16 bits) |"]
        R2["| Sequence Number (32 bits) |"]
        R3["| Acknowledgment Number (32 bits) |"]
        R4["| Data Offset (4) | Reserved (3) | Flags (9) |"]
        R5["| Window Size (16 bits) | Checksum (16 bits) |"]
        R6["| Urgent Pointer (16 bits) | Options + Padding (0–40 bytes) |"]
    end
```
## UDP Header
``` mermaid
graph TB
    style UDP_Header fill:none,stroke:none

    subgraph UDP_Header["UDP Header"]
        direction TB
        R1["| Source Port (16 bits) | Destination Port (16 bits) |"]
        R2["| Length (16 bits) | Checksum (16 bits) |"]
    end

```

## IPv4 Header
``` mermaid
graph TB
    style IPv4_Header fill:none,stroke:none

    subgraph IPv4_Header["IPv4 Header"]
        direction TB
        R1["| Version (4) | IHL (4) | DSCP (6) | ECN (2) |"]
        R2["| Total Length (16 bits) | Identification (16 bits) |"]
        R3["| Flags (3) | Fragment Offset (13) |"]
        R4["| Time To Live (8) | Protocol (8) | Header Checksum (16) |"]
        R5["| Source IP Address (32 bits) |"]
        R6["| Destination IP Address (32 bits) |"]
        R7["| Options + Padding (0–40 bytes) |"]
    end


```
