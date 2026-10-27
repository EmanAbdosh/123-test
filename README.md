# 123-test

Okay, stop reading the GitHub page. You don't need to read the whole tutorial. I am going to give you the **exact steps and commands** to do Task 2 right now. Just copy and paste what I give you.

Make sure you are inside your SEED Ubuntu VM (the one you just took a screenshot of).

### 🛠️ Task 2: Step-by-Step

Open a terminal in your VM and do exactly this:

**Step 1: Create a workspace and your `owner.txt` file**
Type these commands one by one:
```bash
mkdir ~/task2_docker
cd ~/task2_docker
nano owner.txt
```
*   Inside the `nano` editor, type your name and today's date (e.g., `John Doe - October 26, 2026`).
*   Press `Ctrl+O` then `Enter` to save. Press `Ctrl+X` to exit.

**Step 2: Create the Dockerfile**
Type this command:
```bash
nano Dockerfile
```
*   Inside the editor, copy and paste **exactly** this code:
```dockerfile
FROM ubuntu:20.04
RUN apt-get update && apt-get install -y curl
# THIS IS THE FIXED COPY LINE
COPY owner.txt /owner.txt
CMD ["/bin/bash"]
```
*   Press `Ctrl+O` then `Enter` to save. Press `Ctrl+X` to exit.

**📸 SCREENSHOT 1:** Take a screenshot of your terminal showing your fixed Dockerfile. You can use the `cat Dockerfile` command to display it nicely on the screen before screenshotting.

**Step 3: Build the Docker Image**
Run this command to build the container:
```bash
sudo docker build -t mytest .
```
*(Note: It might take a minute or two to download the Ubuntu base image).*

**📸 SCREENSHOT 2:** Once it finishes successfully and says `Successfully tagged mytest:latest`, take a screenshot of the terminal showing the successful build.

**Step 4: Run the container and verify the file**
Run this command to start the container and enter it:
```bash
sudo docker run -it mytest /bin/bash
```
*   Your terminal prompt will change (it will look something like `root@12345:/#`). This means you are now *inside* the container.
*   Type this command to read your file:
```bash
cat /owner.txt
```

**📸 SCREENSHOT 3:** Take a screenshot showing the `cat /owner.txt` command and your name/date printing out inside the container.

**Step 5: Clean up (CRITICAL)**
*   Type `exit` to leave the container.
*   Run this command to delete the container so it doesn't mess up Task 3:
```bash
sudo docker rm $(sudo docker ps -aq)
```
*(This removes any stopped containers).*

---

**🛑 STOP HERE.**

You now have all 3 screenshots for Task 2. Reply **"Done"** and I will give you the exact copy-paste commands for Task 3 (Setting up the Sniffing Lab environment).
