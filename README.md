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
-Host Machine with hypervisor such as VirtualBox or VMWare.
-Ubuntu 22.04 to install on mentioned Virtual Machine.
-Internet Connectivity

## **Project Overview**
1. Set up Ubuntu on a dedicated VM to isolate the honeypot.
2. Install Cowrie.
3. Forward port 22 to Cowries default port (2222) in order to capture login attempts.
4. Capture attack logs.
5. Analyze logs with Python.
6. Visualize the data with Matplotlib.

## **Environment Setup**
1. ### Create a Ubuntu VM



