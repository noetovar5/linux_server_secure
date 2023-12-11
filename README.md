# linux_server_secure
Securing a Linux server involves a series of steps to safeguard it from potential threats. Here's a step-by-step guide to secure a new Linux server instance:
Securing a Linux server involves a series of steps to safeguard it from potential threats. Here's a step-by-step guide to secure a new Linux server instance:

### Step 1: Update the System
Open a terminal or SSH into your server (replace `<your_username>` and `<your_server_ip>` with your actual username and server IP address):
```bash
ssh <your_username>@<your_server_ip>
```

Update the package lists and upgrade installed packages to their latest versions:
```bash
sudo apt update
sudo apt upgrade
```

### Step 2: Create a New User with Sudo Privileges
Create a new user (replace `<new_username>` with your desired username):
```bash
sudo adduser <new_username>
```

Grant administrative privileges (sudo) to the new user:
```bash
sudo usermod -aG sudo <new_username>
```

### Step 3: Configure SSH Access
Edit the SSH configuration file to enhance security:
```bash
sudo nano /etc/ssh/sshd_config
```

Make the following changes:
- Change SSH default port (optional but recommended) by modifying `Port 22` to another port, e.g., `Port 2222`.
- Disable root login by setting `PermitRootLogin no`.
- Enable public key authentication by setting `PasswordAuthentication no`.
- Restart the SSH service to apply changes:
  ```bash
  sudo systemctl restart sshd
  ```

### Step 4: Set Up Firewall (UFW)
Enable the Uncomplicated Firewall (UFW) and allow necessary connections:
```bash
sudo apt install ufw

# Allow SSH on the new port (if changed from default)
sudo ufw allow <new_ssh_port>/tcp

# Enable the firewall
sudo ufw enable

# Check the status of UFW
sudo ufw status
```

### Step 5: Install and Configure Fail2Ban
Install Fail2Ban to protect against brute-force attacks:
```bash
sudo apt install fail2ban
```

Copy the default configuration file:
```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

Edit the configuration file:
```bash
sudo nano /etc/fail2ban/jail.local
```

Ensure the following parameters are set:
- `bantime = 1h` (adjust to your preference)
- `findtime = 10m` (adjust to your preference)
- `maxretry = 3` (adjust to your preference)
- Save and exit the file.

Restart Fail2Ban to apply changes:
```bash
sudo systemctl restart fail2ban
```

### Step 6: Regular Software Updates and Monitoring
Keep your system updated regularly:
```bash
sudo apt update
sudo apt upgrade
```

Install and configure monitoring tools like `unattended-upgrades` to automatically update packages.

### Step 7: Backup Configuration and Regular Backups
Implement a backup strategy to ensure data safety. Use tools like `rsync` or `tar` to create backups and store them securely.

### Step 8: Harden Specific Services
For specific services like databases, web servers, or other applications, follow best practices and security guidelines provided in their respective documentation.

### Step 9: Periodic Security Audits
Regularly perform security audits using tools like Lynis or OpenVAS to identify vulnerabilities and enhance security measures.

Remember, this guide offers foundational security measures. Regularly updating and monitoring your server is crucial for ongoing security. Implementing additional security measures might be necessary depending on your server's specific needs and use case.
