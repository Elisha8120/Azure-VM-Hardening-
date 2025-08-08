# Azure-VM-Hardening-
Azure Virtual Machine created with a virtual network, and UFW hardening. 

## 🧠 Overview
This project demonstrates the Deployment and hardening of a Linux virtual machine (VM) in Microsoft Azure. The VM is hosted inside a Virtual Network (VNet) and configured to allow only secure access using SSH key authentication. Additional security measures include firewall rules (UFW), Network Security Group (NSG) configuration, and the removal of password-based login.

---

## ☁️ Azure Infrastructure

- ✅ **Virtual Network (VNet)** created with:
  - Address space: `10.0.0.0/16`
  - Subnet: `10.0.1.0/24`
- ✅ **Linux Virtual Machine (Ubuntu)** deployed in subnet
- ✅ **Network Security Group (NSG)** with inbound rules for:
-   Port 22 (SSH)
-
-    ![image alt](https://github.com/Elisha8120/Azure-VM-Hardening-/blob/fa058d43db494f4ae4226299db6862b17313e467/Screenshot%202025-08-07%20161835.png)
    
 

---
## Azure Steps 
1) Create a Resource Group

2) Create a Virtual Network (VNet) and Subnet

3) Create a Network Security Group (NSG)

4) Create the Virtual Machine (VM)

![imagealt](https://github.com/Elisha8120/Azure-VM-Hardening-/blob/88eaa83c79a1c209260074698fa24fffc205cee4/Screenshot%202025-08-07%20161216.png)


5) Verifying VM & Private IP (confirm VNet connection)

    ![imagalt](https://github.com/Elisha8120/Azure-VM-Hardening-/blob/d39e0ec58360eefb52b54fa571f08a6010082350/Screenshot%202025-08-08%20074213.png)

7)  Following least privilage access and getting rid of a brute force attack vulnerability

   ![imagalt](https://github.com/Elisha8120/Azure-VM-Hardening-/blob/057fcb804f99b4d62359818f8f08c462aa0c6404/Screenshot%202025-08-07%20134003.png)
  
8) Added User and keys to ensure I and anyone needed can ssh directly into the system, getting rid of a brute force attack vulnerability. 
  
![imagealt](https://github.com/Elisha8120/Azure-VM-Hardening-/blob/197e0bceaffe709ba6e5cb6c92cc977decc0af6e/Screenshot%202025-08-07%20164246.png)

9) Harden SSH: disable password auth + lock root login

![imagealt](https://github.com/Elisha8120/Azure-VM-Hardening-/blob/7ee09ca1c04367fc77b8ac35a4b78ab67cd254b4/Screenshot%202025-08-07%20141655.png)

Test first: keep your current SSH session open and open a new session to verify key-auth works before logging out.

Host Hardening with UFW (basic) — commands and notes

These are the exact commands I ran on the Ubuntu VM to restrict inbound traffic and secure SSH access.

1. Install or ensure UFW is present

sudo apt update && sudo apt install -y ufw

2. Allow important ports BEFORE enabling UFW (very important to avoid lockout)

Allow SSH (or better: restricted to your IP)

# allows SSH from anywhere (lab):
sudo ufw allow ssh
# preferred: allow SSH from your IP only
sudo ufw allow from 40.112.236.132 to any port 22 proto tcp
# rate-limit SSH to prevent brute-force
sudo ufw limit ssh

3. Allow web ports if needed

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

4. Set default policies and enable

sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw enable    # Answer 'y' when prompted

5. Verify firewall rules

sudo ufw status verbose

Expected output example:

Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    <your-ip>/32
80/tcp                     ALLOW IN    Anywhere
443/tcp                    ALLOW IN    Anywhere
22/tcp (v6)                ALLOW IN    Anywhere (v6)


Repro - quick checklist 

Create resource group: ResourceGroup1

Create VNet MyVNet with 10.0.0.0/16 and subnet 10.0.1.0/24.

Create MyNSG and add rule to allow SSH (or restrict to your IP).

Deploy Ubuntu VM in MySubnet and attach MyNSG.

Add public SSH key to VM and verify key-based login.

On VM: create admin user, disable PasswordAuthentication, enable UFW with allow ssh, deny incoming, allow outgoing.

Test and capture outputs.
