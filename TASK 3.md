**Yes, this is absolutely perfect!** 

This screenshot clearly shows the `PermissionError: [Errno 1] Operation not permitted` when trying to open a raw socket as the `seed` user. You now have all the evidence needed for Task 1.1A. Save this as your second screenshot.

---

### 🚀 Moving to Task 1.1B: BPF Filters

For this task, we just need to change the `filter` string in your `sniffer.py` script and run it three different times. 

You will need to run the sniffer **inside the attacker container as root** (just like in Step 2 earlier), and generate traffic from another terminal.

First, let's update your script. In your VM terminal, run:
```bash
nano ~/Labsetup/volumes/sniffer.py
```
Change the `sniff` line to look like this (keep the rest the same):
```python
sniff(iface=iface, filter='icmp', prn=print_pkt)
```
*(Save and exit).*

Now, here is how to test the three filters. **For each one, take a screenshot of the sniffer output.**

#### Filter 1: Capture only ICMP packets
1. Enter attacker container: `docker exec -it 1de16bcd562d /bin/bash`
2. Go to folder: `cd /volumes`
3. Run: `python3 sniffer.py`
4. **In a second terminal**, enter Host A: `docker exec -it 30c0690bf751 /bin/bash`
5. Generate traffic: `ping 10.9.0.6`
6. **📸 Take a screenshot of the attacker terminal showing the ICMP packets.** Then press `Ctrl+C` on both terminals.

#### Filter 2: Capture TCP from a specific IP to port 23
1. In your VM (not the container), edit the script: `nano ~/Labsetup/volumes/sniffer.py`
2. Change the filter line to: `filter='tcp and src host 10.9.0.5 and dst port 23'`
3. Go back into the attacker container and run: `python3 sniffer.py`
4. **In your Host A terminal** (or a new one), generate traffic using `telnet` or `nc`:
   ```bash
   telnet 10.9.0.6 23
   ```
   *(If telnet isn't installed, just run `nc 10.9.0.6 23`)*. This will try to connect to port 23.
5. **📸 Take a screenshot of the attacker terminal showing the TCP packet being captured.**

#### Filter 3: Capture traffic to/from a specific subnet
1. Edit the script again (in your VM): `nano ~/Labsetup/volumes/sniffer.py`
2. Change the filter line to: `filter='net 128.230.0.0/16'` *(You can pick any subnet other than 10.9.0.0/24)*.
3. Go back into the attacker container and run: `python3 sniffer.py`
4. **In your Host A terminal**, ping an IP in that subnet:
   ```bash
   ping 128.230.0.1
   ```
5. **📸 Take a screenshot of the attacker terminal showing the packets to/from that subnet.**

---

**🛑 Stop here!**

Once you have those **3 screenshots** (one for each filter), reply **"Done"** and we will move to **Task 1.2 (Spoofing ICMP Packets)**! You're making great progress.
