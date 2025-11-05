# Cisco WS-C2960S Configuration

---

## VLAN Definitions
```bash
vlan 10
 name Trusted
vlan 20
 name IoT
vlan 30
 name Guest
vlan 40
 name Lab
vlan 99
 name Mgmt
exit
```

---

## Trunk to pfSense
```bash
interface gigabitEthernet1/0/1
 description Uplink_to_pfSense
 switchport mode trunk
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 10,20,30,40,99
 switchport trunk native vlan 10
 spanning-tree portfast trunk
exit
```

---

## Access Ports
```bash
interface gi1/0/2
 description Bosgame_Lab_Server
 switchport mode access
 switchport access vlan 40
 spanning-tree portfast
exit

interface gi1/0/3
 description RaspberryPi_Lab_Node
 switchport mode access
 switchport access vlan 40
 spanning-tree portfast
exit

interface gi1/0/4
 description Dell_OptiPlex_Trusted
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
exit

interface gi1/0/5
 description DesktopPC_Trusted
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
exit

interface gi1/0/6
 description ThinkPad_Kali_Lab
 switchport mode access
 switchport access vlan 40
 spanning-tree portfast
exit

interface gi1/0/7
 description TPLink_EAP653_IoT_AP
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
exit
```

---

## Management Interface
```bash
interface vlan 99
 ip address 192.168.99.2 255.255.255.0
 no shutdown
ip default-gateway 192.168.99.1
end
write memory
```