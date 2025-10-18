# Homelab Firewall & Switch Network

This repository documents the configuration and topology of a self-contained lab network built around a **Protectli FW4C** running **pfSense CE** and a **Cisco WS-C2960S-24TS-L** managed switch.  
The lab network is fully segmented from the home Wi-Fi network and used for firewall testing, VLAN design, and cybersecurity practice.

---

## Lab Photo

![Home Lab Photograph](home-lab-photograph.JPG)

---

## Hardware Overview

| Device | Role | Notes |
|---------|------|-------|
| Protectli FW4C | pfSense firewall & router | WAN from home router, LAN trunk to switch |
| Cisco WS-C2960S-24TS-L | Managed Layer 2 switch | VLAN trunk & segmentation |
| Bosgame Mini PC | Lab server | VLAN 40 (Lab) |
| Raspberry Pi 5 | Sandbox node | VLAN 20 (IoT) |
| Dell OptiPlex | Test workstation | VLAN 10 (Trusted) |
| Desktop PC | Admin desktop | VLAN 10 (Trusted) |
| ThinkPad (Kali) | Pentesting laptop | VLAN 40 (Lab) |
| Patch Panel (24-Port) | Physical cable management | WAN + LAN organized by port |

---

## High-Level Topology

```
[Home Router/Wi-Fi]
       │
   (Patch Port 1)
       │
[Protectli FW4C - WAN]
[Protectli FW4C - LAN]
       │
   (Patch Port 2)
       │
[Cisco Switch Gi1/0/1 - Trunk]
       │
 ┌────────────┬──────────────┬──────────────┐
 │ VLAN10     │ VLAN40       │ VLAN99       │
 │ Trusted PCs│ Lab Devices  │ Switch Mgmt  │
 └────────────┴──────────────┴──────────────┘
```

---

## VLAN Summary

| VLAN | Name | Purpose | Subnet | DHCP Range | Notes |
|------|------|----------|---------|-------------|-------|
| 10 | Trusted | Admin + Workstations | 192.168.10.0/24 | .50–.199 | Native VLAN |
| 20 | IoT | Smart/isolated devices | 192.168.20.0/24 | .50–.199 | Internet only |
| 30 | Guest | External/temporary | 192.168.30.0/24 | .50–.199 | Internet only |
| 40 | Lab | Servers + testing | 192.168.40.0/24 | .50–.199 | For VMs, Pis, etc. |
| 99 | Mgmt | Infrastructure management | 192.168.99.0/24 | Static only | Switch management IP |

---

## Documentation

| File | Description |
|------|--------------|
| [`01_rack_plan_and_inventory.md`](docs/01_rack_plan_and_inventory.md) | Rack elevation and full inventory |
| [`02_physical_topology.md`](docs/02_physical_topology.md) | Rack wiring, patch panel, and switch mapping |
| [`03_network_design.md`](docs/03_network_design.md) | VLAN plan, subnets, and IP addressing |
| [`04_switch_configuration.md`](docs/04_switch_configuration.md) | Cisco WS-C2960S trunk + access port config |
| [`05_pfsense_configuration.md`](docs/05_pfsense_configuration.md) | pfSense VLANs, DHCP, and firewall setup |
| [`06_testing_and_verification.md`](docs/06_testing_and_verification.md) | Ping, isolation, and connectivity tests |

---

## Network Isolation Summary

- Lab VLANs (20, 30, 40, 99) **cannot reach** the home network or each other.  
- Trusted VLAN (10) can manage pfSense and the switch (VLAN99).  
- All VLANs have NAT outbound via pfSense → Home router → Internet.  
- Wi-Fi devices remain on the home router subnet (e.g. 192.168.0.0/24) and are **not routed** through pfSense.

---

## Future Plans

- Automate VLAN + firewall provisioning using **Ansible**
- Deploy internal DNS and Pi-hole container on Bosgame mini PC
- Add OpenVPN or WireGuard remote access for lab testing
- Document configuration backups and syslog setup

