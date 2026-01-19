# Network Protocol Headers (TCP, UDP, IP)

This document contains Mermaid diagrams illustrating the structure and field sizes
of **TCP**, **UDP**, and **IPv4** headers.  
All diagrams are compatible with **GitHub Markdown Mermaid rendering**.

---

## TCP Header

**Minimum size:** 20 bytes (160 bits)  
**Maximum size:** 60 bytes (with options)

```mermaid
graph TD
    subgraph TCP_Header["TCP Header"]
        A1["Source Port 16 bits"]:::field
        A2["Destination Port 16 bits"]:::field
        B1["Sequence Number 32 bits"]:::field
        C1["Acknowledgment Number 32 bits"]:::field
        D1["Data Offset 4 bits | Reserved 3 bits | Flags 9 bits"]:::field
        E1["Window Size 16 bits"]:::field
        E2["Checksum 16 bits"]:::field
        F1["Urgent Pointer 16 bits"]:::field
        F2["Options + Padding 0–40 bytes"]:::field
    end

    classDef field fill:#d9f0ff,stroke:#0074D9,stroke-width:1px;
```
## UDP Header
``` mermaid
graph TD
    subgraph UDP_Header["UDP Header"]
        A1["Source Port 16 bits"]:::field
        A2["Destination Port 16 bits"]:::field
        B1["Length 16 bits"]:::field
        B2["Checksum 16 bits"]:::field
    end

    classDef field fill:#d9f0ff,stroke:#0074D9,stroke-width:1px;
```

## IPv4 Header
``` mermaid
graph TD
    subgraph IPv4_Header["IPv4 Header"]
        A1["Version 4 bits | IHL 4 bits | DSCP 6 bits | ECN 2 bits"]:::field
        B1["Total Length 16 bits"]:::field
        C1["Identification 16 bits"]:::field
        D1["Flags 3 bits | Fragment Offset 13 bits"]:::field
        E1["Time To Live 8 bits"]:::field
        F1["Protocol 8 bits"]:::field
        G1["Header Checksum 16 bits"]:::field
        H1["Source IP Address 32 bits"]:::field
        I1["Destination IP Address 32 bits"]:::field
        J1["Options + Padding 0–40 bytes"]:::field
    end

    classDef field fill:#d9f0ff,stroke:#0074D9,stroke-width:1px;

```
