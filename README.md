# Azure Linux VM Hardening & Web Deployment Lab

## 📌 Project Overview
This portfolio project demonstrates the end-to-end provisioning, security hardening, and configuration of an enterprise-ready Linux environment within Microsoft Azure. The objective of this lab is to move beyond default cloud templates to deploy a resilient, public-facing web server implementing the Principle of Least Privilege and active intrusion prevention.

---

## 🏗️ Architecture & Component Design

The infrastructure is built with layered defensive zones to secure data flow from the public internet down to the application host:

1. **Azure Resource Group (`group1`):** Logical isolation container managing lifecycle and access policies for all project resources.
2. **Network Security Group (NSG):** Perimeter layer stateful firewall strictly limiting ingress access vectors to explicit services (Port 22 for administrative SSH, Port 80 for public HTTP web traffic).
3. **Ubuntu 24.04 LTS Compute Instance (`VM1`):** The core Linux engine hosted on a cost-optimized, resource-constrained footprint (Standard B2ts v2).
4. **SSH Cryptographic Gateway:** Enforced authentication plane utilizing asymmetric RSA key pairs.
5. **Fail2Ban Intrusion Prevention:** Dynamic log parsing engine interacting directly with the Linux kernel firewall to throttle automated scanning threats.
6. **Nginx High-Performance Web Engine:** Web application layer hosting and isolating reverse-proxy environments.

---

## 🛠️ Skills Demonstrated
* **Cloud Infrastructure Engineering:** Resource orchestration, Azure networking, virtual topology routing, and security group design.
* **Linux System Administration:** User account governance, environment variable configuration, file permission scoping (`chmod`/`chown`), and systemd daemon life-cycle management.
* **Defensive Cybersecurity:** Disabling interactive authentication backends, implementing asymmetric key access control, firewall tables mapping, and proactive log analysis.

---

## 🚀 Step-by-Step Implementation

### Phase 1: Azure Provisioning & Setup
1. Created an isolated **Resource Group** named `group1` within the `West Europe` region.
2. Provisioned an **Ubuntu Server 24.04 LTS (Gen2)** virtual machine named `VM1`.
3. Adjusted cost boundaries by configuring an **Auto-Shutdown schedule** at `23:00:00 (UTC)` to avoid unnecessary credit depletion.
4. Downloaded the private cryptographic handshake signature file (`VM1_key.pem`).

### Phase 2: System Access & Hardening
1. Opened local terminal environments and initiated a secure shell connection:
   ```bash
   ssh -i /path/to/VM1_key.pem azureuser@20.56.18.70

   Provisioned an isolated, non-generic administrative account to eliminate shared root vectors:
  ** sudo adduser developer
sudo usermod -aG sudo developer**

Secured file access spaces and securely cloned the original authorized cryptographic signatures over to the newly restricted administrative    user profile:

sudo cp /home/azureuser/.ssh/authorized_keys /home/developer/.ssh/authorized_keys
sudo chown -R developer:developer /home/developer/.ssh
sudo chmod 600 /home/developer/.ssh/authorized_keys

Phase 3: Enforcing Public-Key-Only Authentication
Intercepted default authentication schemes by editing the cloud-init drop-in configuration workspace:

sudo nano /etc/ssh/sshd_config.d/50-cloud-init.conf

Appended hard overrides to eliminate brute-force attack windows entirely:
PasswordAuthentication no
KbdInteractiveAuthentication no
PubkeyAuthentication yes

Committed changes by cycling the core SSH transport daemon:
sudo systemctl restart ssh

Phase 4: Intrusion Prevention Framework (Fail2Ban)
Deployed Fail2Ban to monitor incoming handshakes and protect system daemons:


sudo apt update && sudo apt install fail2ban -y
Instantiated a local persistence profile config file:

sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo systemctl start fail2ban
sudo systemctl enable fail2ban
Phase 5: Nginx Web Server Deployment
Installed the Nginx web application hosting package:

sudo apt install nginx -y
Updated the Azure NSG edge firewall layout by appending an inbound security exception rule opening Port 80 (HTTP).

Overwrote default configurations with a project-specific deployment signature:

echo "<h1>Lab 1: Complete! Defenses Active.</h1>" | sudo tee /var/www/html/index.nginx-debian.html
🧪 Verification & Defensive Diagnostics
Test A: Proof-of-Hardening
Attempting an explicit password-based shell integration bypass bypasses key authorization:

ssh developer@20.56.18.70
Result: Permission denied (publickey). The server refuses negotiation parameters immediately without serving an interactive password challenge prompt.

Test B: Perimeter Integrity Validation
Checked operational telemetry parameters directly from the Fail2Ban administration client:

sudo fail2ban-client status sshd
Manually forced an isolated IP block parameter (8.8.8.8) to evaluate packet drop responses at the low-level kernel interface space:

sudo fail2ban-client set sshd banip 8.8.8.8
sudo nft list ruleset | grep 8.8.8.8
Result: Confirmed successful injection of the block condition directly into nftables/iptables, showing elements = { 8.8.8.8 }.

Test C: Public Application State
Navigating to http://20.56.18.70 over standard web browsers resolves to the custom index file layout, validating web host exposure.

📈 Summary Conclusion
This environment represents a securely hardened, production-ready node. By disabling standard interactive passwords, implementing active intrusion prevention, and strictly routing incoming protocols, the overall attack surface area was minimized significantly.
