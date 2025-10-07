# vpn01 (WireGuard)

This folder contains sanitized configs used in the AAA lab.

- `configs/wg0.conf.example` — server-side WireGuard config (sanitized)
  - Replace `PrivateKey = <REDACTED>` with your real server private key.
  - Replace `<CLIENT_PUBLIC_KEY>` with the Windows client public key.
- `iptables.rules` — UDP/51820 + ICMP rules (optional, if you plan to include)
- `sysctl-ipforward.txt` — note to enable IPv4 forwarding (`net.ipv4.ip_forward=1`)

Apply on a fresh server:

```bash
sudo cp configs/wg0.conf.example /etc/wireguard/wg0.conf
sudo systemctl enable --now wg-quick@wg0
sudo iptables-restore < iptables.rules   # if you use it
echo 'net.ipv4.ip_forward=1' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p 
