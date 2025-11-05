# Testing & Verification

## Connectivity Tests
1. From VLAN10 (Trusted):
   - `ping 192.168.10.1` 
   - `ping 8.8.8.8` 
2. From VLAN20 (IoT / Wi-Fi via EAP653):
   - `ping 192.168.20.1` 
   - `ping 8.8.8.8` 
   - `ping 192.168.10.1` (should fail)
3. From VLAN40 (Lab):
   - `ping 192.168.40.1` 
   - `ping 8.8.8.8` 
   - `ping 192.168.10.1` (should fail)

---

## Isolation Tests
- VLAN20 (EAP653 Wi-Fi + IoT) isolated from VLAN10 and VLAN40.
- VLAN30 (Guest) has Internet-only access.
- VLAN10 (Trusted) can reach pfSense GUI and switch (192.168.99.2).