# Rack Plan & Inventory

This document provides a clear, reproducible plan of the 12U wall rack plus a complete inventory with roles, power, and networking notes.

---

## Rack Elevation (Front View)

> Top-to-bottom. "U" positions are approximate based on current layout.

```
U12 ┌────────────────────────────────────────────────────────┐
    │  24-Port Patch Panel (Cat6)                            │
U11 ├────────────────────────────────────────────────────────┤
    │  Cable Brush / Cable Manager                           │
U10 ├────────────────────────────────────────────────────────┤
    │  1U Fan Shelf (exhaust)                                │
U09 ├────────────────────────────────────────────────────────┤
    │  Cisco WS-C2960S-24TS-L (Managed Switch)               │
U08 ├────────────────────────────────────────────────────────┤
    │                                                        │
U07 ├────────────────────────────────────────────────────────┤
    │  Protectli FW4C, Bosgame Mini PC, Raspberry Pi (shelf) │
U06 ├────────────────────────────────────────────────────────┤
    │  Spare shelf                                           │
U05 ├────────────────────────────────────────────────────────┤
    │                                                        │
U04 ├────────────────────────────────────────────────────────┤
    │  Front-mount PDU / Power Bar                           │
U03 ├────────────────────────────────────────────────────────┤
    │                                                        │
U02 ├────────────────────────────────────────────────────────┤
    │  Dell OptiPlex PC | SATA Dock (8TB HDD + 2TB HDD)      │
U01 └────────────────────────────────────────────────────────┘
```

---

## Cable & Power Notes

- **Patch panel** terminates all ethernet runs from devices, including those external from the rack; short ~0.3m patch leads to the switch for clean dressing.
- **Trunk** from Protectli LAN → Switch Gi1/0/1 carries VLANs **10,20,30,40,99** (native **10**).
- **WAN** from ISP/home router to Protectli **WAN** via Patch Port 1.
- **TP-Link EAP653** (AX3000 Wi-Fi 6) connected to **Patch Port 8 → Gi1/0/7**, powered by **passive PoE+ injector**.
- **PDU** powers switch, Protectli, Bosgame, SATA dock, fan shelf; all adapters labeled.
- **Airflow:** Fan shelf set to exhaust; leave gap if thermals increase.

---

## Rack & Network Inventory

| Item | Make / Model | Role | Mount / U | Power | Network | Notes |
|------|---------------|------|-----------|--------|----------|-------|
| Patch Panel | 24-port Cat6 | Front termination | U12 | — | — | Top of rack |
| Cable Manager | Brush panel | Cable hygiene | U11 | — | — | Below patch panel |
| Fan Shelf | 1U | Cooling / airflow | U10 | IEC to PDU | — | Exhaust out |
| Managed Switch | Cisco WS-C2960S-24TS-L | L2 switching, VLANs | U09 | IEC C13 to PDU | 24×1G (no PoE) | Trunk on Gi1/0/1 to pfSense |
| Firewall | Protectli FW4C | pfSense CE router/firewall | U07 (shelf) | 12V DC to PDU | 4×1G (igc0–igc3) | WAN→Home router; LAN trunk→Switch |
| Bosgame Mini PC | Ryzen 7 5825U / 32GB / 1TB NVMe | Lab server / Proxmox | U07 (shelf) | 19V DC to PDU | 1×1G | VLAN 40 (Lab) |
| Raspberry Pi 5 | 8GB | Lab node / controllers | U07 (shelf) | USB-C to PDU | 1×1G | VLAN 40 (Lab) |
| Spare Shelf | — | Future expansion | U06 | — | — | Currently empty |
| PDU | Front-mount PDU | Power distribution | U04 | Mains | — | Feeds all gear |
| Dell OptiPlex | — | File storage / SIEM node | U02 (with dock) | Mains | 1×1G | VLAN 10 (Trusted) |
| SATA Dock | 2-bay USB (8TB + 2TB HDD) | Bulk storage | U02 (with Dell) | 12V DC to PDU | USB→Bosgame | Media/backup |
| TP-Link EAP653 | AX3000 Dual-Band Wi-Fi 6 AP | Wireless IoT network | External | Passive PoE+ injector | VLAN 20 | Managed by Omada Controller |
| ISP Router | *(Home router)* | Internet uplink | External | Mains | 4×LAN | Provides DHCP to pfSense WAN |

---

## Patch Panel → Switch Mapping

| Patch Port | Connected Device | Switch Port | VLAN | Notes |
|-------------|------------------|--------------|-------|-------|
| 1 | WAN (ISP Router) | — | WAN | Internet uplink |
| 2 | Protectli LAN | Gi1/0/1 | Trunk | VLANs 10,20,30,40,99 |
| 3 | Bosgame Mini PC | Gi1/0/2 | VLAN 40 | Lab server |
| 4 | Raspberry Pi 5 | Gi1/0/3 | VLAN 40 | Lab node |
| 5 | Dell OptiPlex | Gi1/0/4 | VLAN 10 | Trusted |
| 6 | Desktop PC | Gi1/0/5 | VLAN 10 | Admin desktop |
| 7 | ThinkPad (Kali) | Gi1/0/6 | VLAN 40 | Pentesting laptop |
| 8 | TP-Link EAP653 | Gi1/0/7 | VLAN 20 | Wi-Fi AP (IoT network) |

---

## Quick Logical Overview

```
[ISP Router] → [Protectli WAN]
[Protectli LAN] → [Cisco Gi1/0/1 Trunk]
          ↓
   VLAN10 → Trusted PCs & NAS
   VLAN20 → IoT + EAP653 Wi-Fi
   VLAN40 → Lab (Bosgame, Pi, Kali)
   VLAN99 → Management (Switch)
```