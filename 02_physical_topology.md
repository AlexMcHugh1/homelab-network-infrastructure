# Physical Topology & Patch Panel Mapping

This document records the physical cabling layout between the patch panel, Protectli firewall, and Cisco switch.

## Patch Panel → Switch Mapping

| Patch Port | Connected Device | Switch Port | VLAN    | Notes                |
|------------|------------------|-------------|---------|----------------------|
| 1          | WAN (ISP Router) | —           | WAN     | Internet uplink      |
| 2          | Protectli LAN    | Gi1/0/1     | Trunk   | VLANs 10,20,30,40,99 |
| 3          | Bosgame Mini PC  | Gi1/0/2     | VLAN 40 | Lab node             |
| 4          | Raspberry Pi 5   | Gi1/0/3     | VLAN 20 | IoT device           |
| 5          | Dell OptiPlex    | Gi1/0/4     | VLAN 10 | File storage / NAS node|
| 6          | Desktop PC       | Gi1/0/5     | VLAN 10 | Admin desktop        |
| 7          | ThinkPad (Kali)  | Gi1/0/6     | VLAN 40 | Pentesting laptop    |

---

## Logical Overview

```
[ISP Router] → [Protectli WAN]
[Protectli LAN] → [Cisco Gi1/0/1 Trunk]
```

The firewall connects to the home router for internet access, and the switch distributes VLANs to the lab devices.
