# Testing & Verification

## Connectivity Tests
1. From any VLAN10 device:
   - Ping 192.168.10.1 
   - Ping 8.8.8.8 
2. From VLAN40 device:
   - Ping 192.168.40.1 
   - Ping 8.8.8.8 
   - Ping 192.168.10.1 (should fail)

## Isolation Tests
- IoT (VLAN20) cannot access 192.168.10.0/24 or 192.168.40.0/24.
- Guest (VLAN30) has internet only.
- Trusted (VLAN10) can access pfSense GUI and switch (192.168.99.2).
