\# AAA Server — FreeRADIUS (aaa01)



This directory contains the configuration and verification files for the FreeRADIUS server used in the AAA Lab project.



---



\## 📘 Overview

The FreeRADIUS server (`aaa01`) acts as the \*\*Authentication\*\*, \*\*Authorization\*\*, and \*\*Accounting\*\* server in this lab.  

It validates VPN users, authorizes them for access, and logs their session start/stop events for accounting.



\*\*IP Address:\*\* 10.10.10.10  

\*\*Service Ports:\*\*

\- Authentication: UDP/1812  

\- Accounting: UDP/1813  



---



\## ⚙️ Configuration Files



| File | Purpose |

|------|----------|

| `configs/clients.conf.example` | Defines authorized RADIUS clients (e.g., VPN server `vpn01`) and shared secrets (sanitized). |

| `configs/users.example` | Contains local user credentials for authentication (`testuser` / `examplepass`). |

| `/etc/freeradius/3.0/radiusd.conf` | Main FreeRADIUS configuration (default settings). |

| `/var/log/freeradius/radacct/` | Directory where accounting logs (Start/Stop records) are stored. |



---



\## 🔐 Authentication Test



\*\*Local RADIUS test (on `aaa01`):\*\*

```bash
#using placeholder <SHARED_SECRET> for the RADIUS client secret in examples
radtest testuser testpass 127.0.0.1 0 <SHARED_SECRET>

```

\*\*Expected Output:\*\*

```

Sent Access-Request Id ...

Received Access-Accept Id ...

```



\*\*Remote RADIUS test (from vpn01):\*\*

```bash

radtest testuser example 10.10.10.10 0 <SHARED_SECRET>

```



---



\## 📄 Accounting Verification



Check if FreeRADIUS is listening for accounting:

```bash

sudo ss -lunp | grep 1813

```



Expected:

```

udp   UNCONN  0  0  0.0.0.0:1813   \*:\*   users:(("freeradius",pid=XXX,...))

```



To verify accounting entries:

```bash

sudo ls -l /var/log/freeradius/radacct/127.0.0.1/

sudo cat /var/log/freeradius/radacct/127.0.0.1/detail-\*

```



Expected entries:

```

Acct-Status-Type = Start

Acct-Status-Type = Stop

```



---



\## 🧾 Screenshots



| Screenshot | Description |

|-------------|--------------|

| `ss\_1813\_listener.png` | Shows FreeRADIUS listening on port 1813 |

| `accounting\_start\_stop.png` | Shows accounting Start/Stop log entries |



---



\## 🧩 Summary



✅ \*\*Authentication\*\* — `testuser/examplepass` verified via RADIUS  

✅ \*\*Authorization\*\* — via users file policies  

✅ \*\*Accounting\*\* — verified Start/Stop records  





---



\*\*Author:\*\* Ibrahim Shadi  

\*\*Project:\*\* AAA Lab — FreeRADIUS + WireGuard  

\*\*Date:\*\* October 2025



