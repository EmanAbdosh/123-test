**Yes, these screenshots are absolutely perfect!** 

You have exactly what the rubric asks for:
1. The fixed `COPY owner.txt /owner.txt` line.
2. The successful `docker build` and `run` commands.
3. The `cat /owner.txt` output showing your name and date inside the container (`Eman Abdosh - October 26, 2026`).

Save those three images into your screenshot folder. 

---

### 🚨 CRITICAL STEP BEFORE TASK 3
Because we are about to start a completely new Docker environment (Task 3), you **MUST** clean up Task 2 first. If you don't, the new containers will conflict with the old ones and you will get IP/port errors.

Open your VM terminal and run these two commands:
```bash
sudo docker rm -f $(sudo docker ps -aq)
sudo docker rmi mytest
```
*(This removes the container and the image you just made).*

---

### 🚀 Task 3: Setup (Let's do this together)

Now we are moving to the Sniffing and Spoofing lab. We need to set up the `Labsetup` environment.

**Step 1: Download the Labsetup file**
In your VM terminal, run these commands to download and unzip the lab files:
```bash
cd ~
wget https://seedsecuritylabs.org/Labs_20.04/Files/Sniffing_Spoofing/Labsetup.zip
unzip Labsetup.zip
cd Labsetup
```

**Step 2: Start the environment**
Run these two commands (this might take a minute or two as it builds the containers):
```bash
dcbuild
dcup
```

**Step 3: Find your Bridge Interface Name**
This is the most important step for the code to work. Run:
```bash
ifconfig
```
Look through the output. You are looking for an interface that starts with **`br-`** and has the IP address **`10.9.0.1`**. 

It will look something like this:
`br-1a2b3c4d5e6f: flags=... inet 10.9.0.1 netmask...`

**🛑 STOP HERE and reply to me with that exact interface name.** 

*(For example, reply: "My interface is br-9f8e7d6c5b4a").* Once you give me that name, I will give you the exact Python code for **Task 1.1A** with your interface name already typed in so you can just copy, paste, and run it!













**Yes, that is perfect!** The container is gone, so it won't conflict with the new lab environment.

Now, let's dive right into **Task 3**. Follow these steps exactly in your VM terminal:

### 🚀 Step 1: Download and Set Up the Lab
Copy and paste these commands one by one:

```bash
cd ~
wget https://seedsecuritylabs.org/Labs_20.04/Files/Sniffing_Spoofing/Labsetup.zip
unzip Labsetup.zip
cd Labsetup
```

### 🚀 Step 2: Build and Start the Containers
Now run these two commands (the build might take a minute, so be patient):

```bash
dcbuild
dcup
```
*(If `dcbuild` or `dcup` says "command not found", run `docker-compose build` and `docker-compose up -d` instead).*

### 🚀 Step 3: Find Your Interface Name
Once the containers are running in the background, run this command:

```bash
ifconfig
```

Look closely at the output. You need to find the interface that starts with **`br-`** and has the IP address **`10.9.0.1`** next to the word `inet`.

**🛑 STOP HERE.** 

Reply to me with that exact interface name (for example: `br-1a2b3c4d5e6f`). 

Once you give me the name, I will give you the exact Python code for **Task 1.1A** with your interface name already filled in!
