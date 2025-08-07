# Azure-VM-Hardening-
Azure Virtual Machine created with a virtual network, and UFW hardening. 

## 🧠 Overview
This project demonstrates the deployment and hardening of a Linux virtual machine (VM) in Microsoft Azure. The VM is hosted inside a Virtual Network (VNet) and configured to allow only secure access using SSH key authentication. Additional security measures include firewall rules (UFW), Network Security Group (NSG) configuration, and the removal of password-based login.

---

## ☁️ Azure Infrastructure

- ✅ **Virtual Network (VNet)** created with:
  - Address space: `10.0.0.0/16`
  - Subnet: `10.0.1.0/24`
- ✅ **Linux Virtual Machine (Ubuntu)** deployed in subnet
- ✅ **Network Security Group (NSG)** with inbound rules for:
  - Port 22 (SSH)
  - (Optional) Port 80/443 for web traffic

---

## 🔐 SSH Key Authentication

- Generated SSH key pair locally with:
  ```bash
  ssh-keygen -t ed25519
