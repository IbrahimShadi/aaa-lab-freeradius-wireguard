# 🧾 AAA Lab — Accounting Phase (FreeRADIUS + WireGuard)

## 🎯 Objective
In this phase, the goal was to verify that **FreeRADIUS correctly logs Accounting Start and Stop records** for VPN sessions initiated via the WireGuard server.

---

## 🖥️ Environment
| Component | Hostname | IP Address | Description |
|------------|-----------|-------------|--------------|
| FreeRADIUS | `aaa01` | `10.10.10.10` | RADIUS Server |
| VPN Server | `vpn01` | `10.10.10.11` | WireGuard Gateway (RADIUS client) |
| Windows Client | — | `10.10.10.254` | Test VPN Client |

---

## ⚙️ Verification of Accounting Listener

```bash
sudo ss -lunp | grep 1813
Result: FreeRADIUS listening on UDP/1813
(see Screenshot: ss_1813_listener.png)