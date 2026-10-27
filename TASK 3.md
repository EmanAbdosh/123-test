**Yes, these are perfect!** 

*   **Image 1 (Filter 2):** Correct. It shows `proto = tcp`, source `10.9.0.5`, destination `10.9.0.6`, and the destination port is `telnet` (port 23). 
*   **Image 2 (Filter 3):** Correct. It shows `proto = icmp`, source `10.9.0.5`, destination `128.230.0.1` (and the reply).

Save both of these. You have officially completed **Section 3.1**! 

Now, let's knock out **Task 1.2: Spoofing ICMP Packets**. This is a quick one.

### 🚀 Task 1.2: Spoofing an ICMP Echo Request

In this task, you will send a fake ping to Host A (10.9.0.5) pretending to be someone else, and prove it using Wireshark.

**Step 1: Write the Spoofing Script**
In your VM terminal, go to your volumes folder:
```bash
cd ~/Labsetup/volumes
nano spoof_icmp.py
```
Paste this exact code:
```python
#!/usr/bin/env python3
from scapy.all import *

# We are spoofing a fake IP (1.2.3.4) sending to Host A (10.9.0.5)
ip_layer = IP(src="1.2.3.4", dst="10.9.0.5")
icmp_layer = ICMP()
packet = ip_layer / icmp_layer

print("Sending spoofed ICMP packet...")
send(packet)
print("Sent!")
```
*Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).*

**Step 2: Start Wireshark**
1. In your VM (not inside a container), open **Wireshark** from the applications menu.
2. Double-click on the **`br-d76fa343d352`** interface to start capturing.

**Step 3: Run the Attack**
1. Go to your terminal. Enter the attacker container:
   ```bash
   docker exec -it 1de16bcd562d /bin/bash
   ```
2. Run the script:
   ```bash
   cd /volumes
   python3 spoof_icmp.py
   ```

**Step 4: Stop and Capture**
1. Go back to Wireshark and click the red square (Stop) button.
2. In the Wireshark filter bar at the top, type: `icmp` and hit Enter.
3. You will see an **Echo (ping) request** with a source IP of `1.2.3.4` and a destination of `10.9.0.5`.

**📸 SCREENSHOT:** Take a screenshot of Wireshark showing the spoofed packet (make sure the Source column clearly says `1.2.3.4`).

---

**🛑 Stop here!** Once you have that Wireshark screenshot, reply **"Done"**, and we will move to **Task 1.3 (Traceroute)**!
