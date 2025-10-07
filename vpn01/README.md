vpn01 (WireGuard)

This directory documents the VPN server (WireGuard) side of the AAA lab setup.


───────────────────────────────────────────────

1️⃣ Overview

───────────────────────────────────────────────
The vpn01 node acts as the WireGuard VPN server. It provides secure connectivity 
for remote clients (e.g., Windows clients) and integrates with the FreeRADIUS 
server (aaa01) for authentication and accounting.

The setup was tested with successful start and stop events recorded in the 
FreeRADIUS accounting logs (/var/log/freeradius/radacct/).


───────────────────────────────────────────────

2️⃣ Folder Structure

───────────────────────────────────────────────
configs/
 ├── wg0.conf.example          → sanitized WireGuard server configuration
 ├── iptables.rules            → optional UDP/ICMP forwarding rules
 └── sysctl-ipforward.txt      → reminder to enable IPv4 forwarding

screenshots/
 ├── ss_vpn01_udp_port_51820.png       → WireGuard UDP listener verification
 ├── ss_wg0_interface_active.png       → wg0 interface showing peer connection
 ├── ss_wg_service_status.png          → wg-quick@wg0 service active and enabled
 ├── ss_copy_wg0_conf_success.png      → scp transfer or config setup confirmation
 └── ss_vpn01_ping_client_success.png  → (optional) connectivity validation screenshot

───────────────────────────────────────────────

3️⃣ Configuration Instructions

───────────────────────────────────────────────
• Copy the sanitized WireGuard config to the system path:
  sudo cp configs/wg0.conf.example /etc/wireguard/wg0.conf

• Edit wg0.conf to include real keys:
  - Replace:  PrivateKey = <REDACTED>            # server’s private key
  - Replace:  PublicKey  = <CLIENT_PUBLIC_KEY>  # client’s public key

• Apply system configurations:
  sudo iptables-restore < configs/iptables.rules
  echo 'net.ipv4.ip_forward=1' | sudo tee -a /etc/sysctl.conf
  sudo sysctl -p


───────────────────────────────────────────────

4️⃣ Service Control

───────────────────────────────────────────────
• Start WireGuard:
  sudo systemctl start wg-quick@wg0

• Enable at boot:
  sudo systemctl enable wg-quick@wg0

• Check service status:
  sudo systemctl status wg-quick@wg0

• Verify listening port:
  sudo ss -ulpn | grep 51820


───────────────────────────────────────────────

5️⃣ Verification and Results

───────────────────────────────────────────────
• WireGuard interface wg0 shows as active with a valid peer.
• UDP port 51820 is open for IPv4/IPv6 connections.
• Accounting logs on aaa01 confirmed Start/Stop records for “testuser”.
• End-to-end VPN authentication and accounting were successful.


───────────────────────────────────────────────

6️⃣ Author & Notes

───────────────────────────────────────────────
Author: Ibrahim Shadi  
Date: October 2025  
Part of the AAA Lab (FreeRADIUS + WireGuard Integration)  
Repository: https://github.com/IbrahimShadi/aaa-lab-freeradius-wireguard
