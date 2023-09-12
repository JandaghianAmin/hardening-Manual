# hardening-Manual
Please note that these scripts should be used with caution and tailored to your specific server's needs and configurations.

Update and Upgrade:

```
#!/bin/bash
apt update
apt upgrade -y
```

Firewall Configuration with UFW:

```
#!/bin/bash
apt install ufw -y
ufw default deny incoming
ufw default allow outgoing
ufw allow <custom-port>/tcp  # Replace <custom-port> with your chosen SSH port
ufw enable
```


SSH Hardening:

```
#!/bin/bash
# Change SSH port
sed -i 's/Port 22/Port <custom-port>/' /etc/ssh/sshd_config
```

# Disable root login
```
sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
service ssh restart
```

User Management:

```
#!/bin/bash
# Create a new user
adduser myuser

# Add the new user to the sudo group
usermod -aG sudo myuser

# Disable password authentication for SSH (requires SSH key)
sed -i 's/PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config

service ssh restart
```


File Permissions:

```
#!/bin/bash
# Example: Lock down permissions on sensitive files
chmod 600 /etc/shadow
chmod 600 /etc/gshadow
chmod 644 /etc/passwd
chmod 644 /etc/group
```

Install Security Software:

```
#!/bin/bash
apt install fail2ban logwatch tripwire -y
```

Kernel Hardening:

```
#!/bin/bash
# Enable ASLR
echo "kernel.randomize_va_space=2" >> /etc/sysctl.conf

# Enable DEP
echo "kernel.exec-shield=1" >> /etc/sysctl.conf

sysctl -p
```

Disable Unnecessary Services:

```
#!/bin/bash
systemctl disable <service-name>
```

Automatic Security Updates:

```
#!/bin/bash
apt install unattended-upgrades -y
dpkg-reconfigure --priority=low unattended-upgrades
```
Please replace <custom-port> and <service-name> with your chosen values. 
Make sure to review and customize these scripts to match your server's specific requirements.
Run each script as root or with sudo privileges.

Remember that while automation can be helpful, it's essential to understand the changes you are making to your server's configuration and regularly audit your security measures to ensure they remain effective.
