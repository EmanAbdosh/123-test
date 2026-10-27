Here is exactly how that section of your report should look. You can copy and paste this entire block directly into your Word document.

It combines the code, the explanation, and the placeholder for your screenshot.

***

### 3.1 Task 1.1: Sniffing Packets

**Code:**
```python
#!/usr/bin/env python3
from scapy.all import *

def print_pkt(pkt):
    pkt.show()

# The specific interface name found using ifconfig
iface = 'br-d76fa343d352' 

print(f"Sniffing on {iface}...")

# Sniffing only ICMP packets and calling print_pkt for each one
sniff(iface=iface, filter='icmp', prn=print_pkt)
```
*(Note: For Task 1.1B, I simply changed the `filter` string to `'tcp and src host 10.9.0.5 and dst port 23'` and then to `'net 128.230.0.0/16'` and re-ran the code).*

**Code Explanation:**
*   The script uses Scapy’s `sniff()` function. The `iface` parameter binds the sniffer to the specific bridge interface (`br-d76fa343d352`) so it can monitor the container network. 
*   The `filter='icmp'` parameter applies a BPF (Berkeley Packet Filter) to capture only ICMP packets, ignoring all other traffic.
*   The `prn=print_pkt` parameter tells Scapy to invoke the `print_pkt()` callback function for every single packet captured, which then prints the packet details to the screen using `pkt.show()`.

**Observations (Root vs. Non-Root):**
*   **As Root:** The script successfully captured the ICMP echo requests and replies between Host A and Host B.
*   **As Non-Root (seed user):** The program immediately crashed with a `PermissionError: [Errno 1] Operation not permitted`. 
*   **Why?** Packet sniffing requires creating a **raw socket** (`AF_PACKET`), which allows the program to bypass the OS's normal network stack and see all traffic on the wire. Operating systems restrict raw socket creation to the root user for security reasons, preventing standard users from spying on other users' network traffic.

**[INSERT SCREENSHOT HERE: Sniffer running as root capturing ICMP packets]**
**[INSERT SCREENSHOT HERE: Sniffer failing with PermissionError as non-root]**
**[INSERT SCREENSHOT HERE: Filter 1 - ICMP]**
**[INSERT SCREENSHOT HERE: Filter 2 - TCP port 23]**
**[INSERT SCREENSHOT HERE: Filter 3 - Subnet 128.230.0.0/16]**

***

You can do the exact same format for the other sections:
1. **Code:** Paste the Python script.
2. **Code Explanation:** Paste the explanation I gave you.
3. **Observation:** Paste the observation text.
4. **Screenshots:** Insert your pictures below.

You are fully set up to get an A on this report! Let me know if you need the exact formatting for the other tasks (1.2, 1.3, 1.4) and I can write those out too.
