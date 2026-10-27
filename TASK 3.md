Perfect! Your interface name is **`br-d76fa343d352`**. 

Now we are on **Task 3.1: Task 1.1A (Sniffing Packets)**. We will write the Python script and run it. 

Please follow these steps exactly:

### 📝 Step 1: Create the Sniffer Script
In your VM terminal (you should currently be inside `~/Labsetup`), type:
```bash
cd volumes
nano sniffer.py
```
Copy and paste this exact code into the editor:
```python
#!/usr/bin/env python3
from scapy.all import *

def print_pkt(pkt):
    pkt.show()

# Your specific interface name
iface = 'br-d76fa343d352' 
print(f"Sniffing on {iface}...")
sniff(iface=iface, filter='icmp', prn=print_pkt)
```
*Press `Ctrl+O`, `Enter` to save, then `Ctrl+X` to exit.*

### 🐳 Step 2: Enter the Attacker Container
According to the lab, we must run our attack code inside the attacker container (which is in host mode and can see all traffic). 
Run this command to see your running containers:
```bash
docker ps
```
Look for the container named **`seed-attacker`** and copy its ID (it will be the first few characters of the ID, like `a1b2c3d4`).
Now enter it:
```bash
docker exec -it <container_id> /bin/bash
```
*(Replace `<container_id>` with your actual ID).*
Once inside, navigate to the shared volumes folder:
```bash
cd /volumes
```

### 🚀 Step 3: Run as Root (Successful)
Run your sniffer:
```bash
python3 sniffer.py
```
It will say `Sniffing on br-d76fa343d352...` and hang there. 

**To generate traffic:** Open a **second terminal tab** in your VM (don't close the first one!). Get into Host A's container:
```bash
docker exec -it <hostA_container_id> /bin/bash
ping 10.9.0.6
```
Look back at your **first terminal**. You should see `pkt.show()` printing the ICMP packets! 

**📸 SCREENSHOT 1:** Take a screenshot of the first terminal showing the sniffer capturing the ICMP packets.

Stop the sniffer by pressing `Ctrl+C`. Exit the attacker container by typing `exit`.

### 🚫 Step 4: Run as Non-Root (Failed)
Now, let's prove why root is needed. Run the script directly on your VM (not inside the container):
```bash
python3 sniffer.py
```
*(Or switch to the `seed` user using `su seed` and run it).*
It will immediately fail with a `PermissionError` or `Socket error: Operation not permitted`.

**📸 SCREENSHOT 2:** Take a screenshot of this error message. 

*(Your report explanation: "Sniffing requires root privilege because it needs to open a raw socket to capture all network traffic, bypassing the OS's normal filtering. Without root, the OS blocks this for security reasons.")*

---

### 🛑 Stop Here!
Once you have those **two screenshots** (one successful sniff, one permission denied), reply **"Done"** and we will move to **Task 1.1B (BPF Filters)**!
