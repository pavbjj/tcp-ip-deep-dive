# Network Protocol Headers (TCP, UDP, IP)

This document contains Mermaid diagrams illustrating the structure and field sizes
of **TCP**, **UDP**, and **IPv4** headers.  
All diagrams are compatible with **GitHub Markdown Mermaid rendering**.

---

## TCP Header

**Minimum size:** 20 bytes (160 bits)  
**Maximum size:** 60 bytes (with options)

```mermaid
flowchart TB
    subgraph TCP["TCP Header (Minimum 20 bytes / 160 bits)"]
        A["Source Port (16 bits)"]
        B["Destination Port (16 bits)"]
        C["Sequence Number (32 bits)"]
        D["Acknowledgment Number (32 bits)"]
        E["Data Offset (4) | Reserved (3) | Flags (9)"]
        F["Window Size (16 bits)"]
        G["Checksum (16 bits)"]
        H["Urgent Pointer (16 bits)"]
        I["Options + Padding (0–40 bytes, optional)"]
    end

    A --> B --> C --> D --> E --> F --> G --> H --> I

## UDP Header
flowchart TB
    subgraph UDP["UDP Header (8 bytes / 64 bits)"]
        A["Source Port (16 bits)"]
        B["Destination Port (16 bits)"]
        C["Length (16 bits)"]
        D["Checksum (16 bits)"]
    end

    A --> B --> C --> D
## IPv4 Header
flowchart TB
    subgraph IP["IPv4 Header (Minimum 20 bytes / 160 bits)"]
        A["Version (4) | IHL (4) | DSCP (6) | ECN (2)"]
        B["Total Length (16 bits)"]
        C["Identification (16 bits)"]
        D["Flags (3) | Fragment Offset (13)"]
        E["TTL (8 bits)"]
        F["Protocol (8 bits)"]
        G["Header Checksum (16 bits)"]
        H["Source IP Address (32 bits)"]
        I["Destination IP Address (32 bits)"]
        J["Options + Padding (0–40 bytes, optional)"]
    end

    A --> B --> C --> D --> E --> F --> G --> H --> I --> J
