# Physical Topology & Patch Panel Mapping

This document records the physical cabling layout between the patch panel, Protectli firewall, and Cisco switch.

---

## Patch Panel → Switch Mapping

| Patch Port | Connected Device    | Switch Port | VLAN | Notes                                  |
|-----------:|---------------------|------------:|------|----------------------------------------|
| 1          | WAN (ISP Router)    | —           | WAN  | Internet uplink                        |
| 2          | Protectli LAN       | Gi1/0/1     | Trunk| VLANs 10,20,30,40,99                   |
| 3          | Bosgame Mini PC     | Gi1/0/2     | 40   | Proxmox / Lab server                   |
| 4          | Raspberry Pi 5      | Gi1/0/3     | 40   | Lab node / controllers                 |
| 5          | Dell OptiPlex       | Gi1/0/4     | 10   | Trusted workstation / storage / SIEM   |
| 6          | Desktop PC          | Gi1/0/5     | 10   | Admin desktop (Trusted)                |
| 7          | ThinkPad (Kali)     | Gi1/0/6     | 40   | Pentesting laptop (Lab)                |
| 8          | TP-Link EAP653 AP   | Gi1/0/7     | 20   | Wi-Fi 6 IoT AP (powered by 12V adapter)|

---

## Logical Overview

```
[ISP Router] → [Protectli FW4C WAN]
[Protectli FW4C LAN] → [Cisco Gi1/0/1 Trunk]

Gi1/0/2 ← Patch 3 → Bosgame (VLAN 40 - Lab)
Gi1/0/3 ← Patch 4 → Raspberry Pi 5 (VLAN 40 - Lab)
Gi1/0/4 ← Patch 5 → Dell OptiPlex (VLAN 10 - Trusted)
Gi1/0/5 ← Patch 6 → Desktop PC (VLAN 10 - Trusted)
Gi1/0/6 ← Patch 7 → ThinkPad (Kali, VLAN 40 - Lab)
Gi1/0/7 ← Patch 8 → TP-Link EAP653 (VLAN 20 - IoT Wi-Fi)
```

The firewall connects to the home router for internet access, and the switch distributes VLANs to the lab devices.

The **TP-Link EAP653** provides wireless coverage for **VLAN 20 (IoT)**.  
It is cabled via **Patch Port 8 → Switch Gi1/0/7**, assigned to **VLAN 20**, and powered by a **12 V DC adapter (not PoE)**.