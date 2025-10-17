# pfSense VLAN and Firewall Configuration

## Interfaces
- WAN: DHCP (connected to home router)
- LAN (igc1): VLAN trunk to switch

### VLAN Interfaces
| VLAN | Tag |  Interface  |        IP       |   DHCP   | Notes              |
|------|-----|-------------|-----------------|----------|--------------------|
| 10   | 10  | LAN_Trusted | 192.168.10.1/24 | Enabled  | Default admin VLAN |
| 20   | 20  | LAN_IoT     | 192.168.20.1/24 | Enabled  | Internet-only      |
| 30   | 30  | LAN_Guest   | 192.168.30.1/24 | Enabled  | Internet-only      |
| 40   | 40  | LAN_Lab     | 192.168.40.1/24 | Enabled  | For Proxmox host + SIEM VMs|
| 99   | 99  | LAN_Mgmt    | 192.168.99.1/24 | Disabled | Static mgmt VLAN   |
