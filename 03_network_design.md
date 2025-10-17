# Network Design

## VLANs and Subnets

| VLAN | Name | Subnet | Gateway | DHCP Range |
|------|------|---------|----------|-------------|
| 10 | Trusted | 192.168.10.0/24 | 192.168.10.1 | 192.168.10.50–199 |
| 20 | IoT | 192.168.20.0/24 | 192.168.20.1 | 192.168.20.50–199 |
| 30 | Guest | 192.168.30.0/24 | 192.168.30.1 | 192.168.30.50–199 |
| 40 | Lab | 192.168.40.0/24 | 192.168.40.1 | 192.168.40.50–199 |
| 99 | Mgmt | 192.168.99.0/24 | 192.168.99.1 | Static only |

Note: VLAN 40 supports the Proxmox host (Bosgame) and virtualized SIEM workloads.
