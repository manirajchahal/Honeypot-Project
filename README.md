# Honeypot-Project
This project sets up an SSH honeypot using Cowrie, captures login attempts, and provides basic log analysis. The goal is to demonstrate hands-on cybersecurity skills—ranging from VM setup and network configuration to data parsing and visualization of attacker behavior.

## **Table of Contents**

1. Prerequisites
2. Project Overview
3. Environment Setup
4. Installing and Configuring Cowrie
5. Port Forwarding
6. Verifying and Testing the Honeypot
7. Log Analysis
8. Ethics Disclaimer
9. License

## **Prerequisites**
-Host Machine with hypervisor such as VirtualBox or VMWare. \
-Ubuntu 22.04 to install on mentioned Virtual Machine. \
-Internet Connectivity.

## **Project Overview**
1. Set up Ubuntu on a dedicated VM to isolate the honeypot.
2. Install Cowrie.
3. Forward port 22 to Cowries default port (2222) in order to capture login attempts.
4. Capture attack logs.
5. Analyze logs with Python.
6. Visualize the data with Matplotlib.

## **Environment Setup**
1. ### Create a Ubuntu VM
     1. Download Ubuntu 22.04 ISO from [Ubuntu's](https://ubuntu.com/download/desktop) website.
     2. Create a new VM (ex. VirtualBox): \
        -Name: HoneypotVM \
        -Type: Linux \
        -Version: Ubuntu (64-bit) \
        -RAM: 2GB (minimum) \
        -HDD: 20GB (minimum)
   3. Attach the ISO to the VM and install Ubuntu.
   4. Choose either bridged or NAT networking type under Network. (Bridged for easier connection to other VM's and have a local IP, NAT for security).
   5. Use the following command to update your VM: \
   `sudo apt-get update && sudo apt-get upgrade -y`

## **Installing and Configuring Cowrie**

1. Installing Dependencies: \
   `sudo apt-get update` \
   `sudo apt-get install -y python3-virtualenv python3-dev libssl-dev libffi-dev build-essential git`
2. Clone the Cowrie Repository: \
   `cd ~` \
   `git clone https://github.com/cowrie/cowrie.git` \
   `cd cowrie`
3. Set Up a Python Virtual Environment: \
   `python3 -m venv cowrie-env` \
   `source cowrie-env/bin/activate` \
   `pip install --upgrade pip`
4. Install Cowrie Requirements: \
   `pip install -r requirements.txt`
5. Create Local Copy of Cowrie Config: \
   `cp etc/cowrie.cfg.dist etc/cowrie.cfg`
6. Start/Stop Cowrie:
   1. Start Cowrie:
      `bin/cowrie start`
   2. Check Cowrie Status:
      `bin/cowrie status`
   3. Stop Cowrie:
      `bin/cowrie stop`

## **Port Forwarding**

Attackers typically look to attack port 22, the SSH port. In order for Cowrie to listen, the information from port 22 has to be forwarded to prot 2222, which is Cowrie's port.

Using iptables on the VM. \
`sudo iptables -t nat -A PREROUTING -p tcp --dport 22 j REDIRECT --to-port 2222` 

In order to keep this rule persistent, even after reboots, use the following code: \
`sudo apot-get install -y iptables-persistent` 

When prompted, ask to save current IPv4 (and/or IPv6) rules. 

Now, any traffic to port 22 will go to port 2222, where Cowrie listens.

## Verifying and Testing the Honeypot

1. Verify Cowrie is started.

   To Start Cowrie: \
   `cd ~/cowrie` \
   `source cowrie-env/bin/activate` \
   `bin/cowrie start` 

2. Test SSH from host: \
   `ssh root@<honeypot-VM-IP>`

   - You might see "Permission Denied" if Cowrie is configured to reject login attempts.
   - Check Cowrie's log directory to confirm it's logging:
  
     `tail -f~/cowrie/var/log/cowrie/cowrie.log`

3. Look for JSON logs in: \
   `~/cowrie/var/log/cowrie/cowrie.json`

   You should see "cowrie.login.failed" events whenever a SSH attempt is made to port 22.

## Log Analysis

We can parse these logs to see who is trying to log in and which credentials they use.

1. Parsing with a Simple Python Script
   Create a script called parse_cowrie_logs.py (in ~/cowrie-analysis or a folder of your choice):

   **Reference Log Parser Folder/JSON Log Parser**
   You should see your **failed login attempts** in the console.

2. Export to CSV:

   If you want a CSV file:

   **Reference Log Parser Folder/Updated JSON Log Parser**

   You can run the script by entering: \

   `python3 parse_cowrie_logs.py`

   A file named cowrie_events.csv is generated with your honeypot logs.

3. Analyzing with Pandas:
   1. Install Pandas:
      `pip install pandas`
   2. Create analysis.py:
  
      **Reference Analyze CSV Folder/Pandas**

   3. Run:
      `python3 analysis.py`

      You'll get a list of your most frequent attackers.

 4. Visualizing Attacks:

      To generate a simple bar chart:

      **Reference Analyze CSV Folder/Pandas + Updated Matplotlib**

## Ethical Disclaimer
- **Intended Use:** This honeypot project is for **educational** and **research** purposes only.
- **Network Isolation:** You should run this honeypot in a **controlled environment** or a dedicated VM.
- **Liability:** You are responsible for how you deploy and maintain your honeypot. Exposing a honeypot to the internet can attract malicious traffic. Ensure you understand the risks and properly segment from production or personal data.

## License
Feel free to use and modify this project under the ters of the MIT license.

   
   



