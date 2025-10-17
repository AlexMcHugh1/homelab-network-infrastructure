# Cisco WS-C2960S Configuration

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

## Access Ports
```bash
interface range gi1/0/2 , gi1/0/6
 description Lab_Devices
 switchport mode access
 switchport access vlan 40
 spanning-tree portfast
exit

interface gi1/0/3
 description IoT_Device
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
exit



interface range gi1/0/4 , gi1/0/5
 description Trusted_Devices
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
exit
```

## Management Interface
```bash
interface vlan 99
 ip address 192.168.99.2 255.255.255.0
 no shutdown
ip default-gateway 192.168.99.1
end
write memory
```
