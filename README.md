AAA Lab — FreeRADIUS + WireGuard Integration

───────────────────────────────────────────────

1️⃣ Project Overview

───────────────────────────────────────────────

This repository documents a functional lab environment integrating FreeRADIUS and WireGuard 
to demonstrate AAA principles (Authentication, Authorization, Accounting) in a VPN context.

The lab includes two virtual nodes:

• aaa01 — FreeRADIUS server (10.10.10.10)

• vpn01 — WireGuard VPN server (10.10.10.11)

Together they implement a secure, end-to-end AAA architecture for VPN users.

───────────────────────────────────────────────

2️⃣ Architecture Summary

───────────────────────────────────────────────

```
┌──────────────────────────┐       UDP/1812–1813       ┌──────────────────────────┐
│        vpn01 (WG)        │ <──────────────────────>  │     aaa01 (RADIUS)       │
│  WireGuard Server        │                           │  Authentication + Logging│
│  User connects via VPN   │                           │  FreeRADIUS 3.0 Service  │
└──────────────────────────┘                           └──────────────────────────┘
```

Client authentication requests originate from vpn01 to aaa01 over RADIUS UDP ports.
Accounting Start/Stop events are logged by aaa01 under /var/log/freeradius/radacct/.


───────────────────────────────────────────────

3️⃣ Folder Structure

───────────────────────────────────────────────
```
aaa-lab-freeradius-wireguard/
├── aaa01/                     → FreeRADIUS server configs & verification
│   ├── configs/
│   │   ├── clients.conf.example
│   │   └── users.example
│   ├── screenshots/
│   │   ├── ss_1813_listener.png
│   │   └── accounting_start_stop.png
│   └── README.md
│
├── vpn01/                     → WireGuard VPN server configs & screenshots
│   ├── configs/
│   │   └── wg0.conf.example
│   ├── screenshots/
│   │   ├── ss_vpn01_udp_port_51820.png
│   │   └── ss_wg_service_status.png
│   └── README.md
│
├── LICENSE
└── README.md  ← This file
```
───────────────────────────────────────────────

4️⃣ How to Use This Repository

───────────────────────────────────────────────

1. Clone the repository:
   ```
    git clone https://github.com/IbrahimShadi/aaa-lab-freeradius-wireguard.git
   ```
2. Review and copy the example configuration files:

   • aaa01/configs/*.example → /etc/freeradius/3.0/

   • vpn01/configs/*.example → /etc/wireguard/

3. Replace placeholder values (<SHARED_SECRET>, <REDACTED>, etc.) with real ones **on your host only.**
   

4. Start both services:
   
   - On aaa01:  `sudo systemctl restart freeradius`
     
   - On vpn01:  `sudo systemctl start wg-quick@wg0`

5. Validate:
   - RADIUS Authentication: `radtest testuser examplepass 10.10.10.10 0 <SHARED_SECRET>`
     
   - WireGuard Connectivity: `sudo ss -ulpn | grep 51820`

───────────────────────────────────────────────

5️⃣ Results

───────────────────────────────────────────────

✅ Authentication via FreeRADIUS confirmed (`Access-Accept`)  
✅ Authorization managed via users file  
✅ Accounting Start/Stop entries recorded  
✅ WireGuard peer communication established  

───────────────────────────────────────────────

6️⃣ Security Notes

───────────────────────────────────────────────

- Do not commit real credentials or keys.
- Use placeholders in public repositories.
- Rotate secrets if they were exposed.
- Keep `/etc/wireguard` and `/etc/freeradius` permissions strict (chmod 600).

───────────────────────────────────────────────

7️⃣ Author & Acknowledgment

───────────────────────────────────────────────

Author: Ibrahim Shadi   
Date: October 2025  

Part of the AAA Lab Project — FreeRADIUS + WireGuard Integration  
