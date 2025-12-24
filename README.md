# LAB: MAC Spoofing

## Environment
- **Attacker:** Kali Linux
- **Target:** Ubuntu Linux
- **Network Type:** Same Layer-2 network (VirtualBox / VMware / LAN)

---

## Objective
- Change Kali’s MAC address to impersonate another host
- Verify network connectivity using a spoofed MAC
- Observe how MAC spoofing impacts network behavior
- Understand how defenders detect MAC spoofing and impersonation

---

## Background
A MAC (Media Access Control) address is a Layer-2 identifier used by switches to deliver frames within a local network.  
Many networks still rely on MAC-based filtering or trust, which can be easily bypassed using MAC spoofing.

---

## Before You Start
1. (Kali + Ubuntu)
2. Identify your active network interface:
<img width="1919" height="1031" alt="Screenshot 2025-12-23 180717" src="https://github.com/user-attachments/assets/dcc23448-2e37-4411-9743-8319c9dd2b24" />


<img width="1916" height="1033" alt="image" src="https://github.com/user-attachments/assets/7c1576e4-ac5e-4730-a0cf-7f3be038f3f8" />


## Steps
1. Check current MAC
  - ip link show <interface>

2. Bring interface down
  - sudo ip link set dev <interface> down
<img width="654" height="511" alt="Screenshot 2025-12-22 223152" src="https://github.com/user-attachments/assets/9a4cf22a-4f22-4806-9459-d23d6356b093" />


3. Set a random MAC
  - sudo macchanger -r <interface>
<img width="951" height="1027" alt="image" src="https://github.com/user-attachments/assets/df025652-ee4f-4ce1-95d5-fe45a511b4fa" />

4. Bring interface up
  - sudo ip link set dev <interface> up
<img width="960" height="1032" alt="image" src="https://github.com/user-attachments/assets/1aa376cd-f47f-49f3-9da0-c4aed2bea7bf" />

5. Verify new MAC
  - ip link show <interface>

6. Test connectivity
  - ping -c 4 <Ubuntu IP>
<img width="960" height="1032" alt="image" src="https://github.com/user-attachments/assets/f380495d-f3ba-491e-82e2-e661a6f8982f" />

7. Restore original MAC
  - sudo macchanger -p <interface>

---


## What This Does

- macchanger -r assigns a new, random MAC address

- Bringing the interface down and back up applies the MAC change

- Pinging the target host confirms whether the network accepts the spoofed MAC address

## Why This Matters for Intrusion Detection

- MAC-based controls are easily bypassed. Analysts should monitor MAC-to-port bindings
and alert on MAC movement or duplicates, common signs of impersonation or lateral
access attempts. Based on the results 
