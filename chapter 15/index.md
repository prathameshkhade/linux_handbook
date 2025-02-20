# Chapter 15: Advanced Security Practices**  

## 15.1 Intrusion Detection and Prevention
### 15.1.1 Understanding Intrusion Detection and Prevention Systems (IDS/IPS)**  
- Difference between IDS and IPS  
- Host-based vs. Network-based IDS  
### 15.1.2 Implementing `fail2ban` for Brute-force Protection**  
- Installing and configuring `fail2ban`  
- Creating and modifying jail configurations  
- Monitoring logs and handling banned IPs  
### 15.1.3 Monitoring for Suspicious Activity**  
- Using `auditd` for system auditing  
- Monitoring system logs (`/var/log/auth.log`, `/var/log/secure`)  
- Using `psad` for detecting port scans  
- Real-time monitoring tools (`OSSEC`, `Suricata`, `Snort`)  

## 15.2. Data Encryption and Backup Security
### 15.2.1 Importance of Encryption and Secure Backups
- Why encryption matters for data security  
- Encryption at rest vs. encryption in transit  
### 15.2.2 Using `gpg` for File and Email Encryption
- Generating GPG keys  
- Encrypting and decrypting files  
- Using GPG for secure email communication  
### 15.2.3 Encrypting Disk and Partitions
- Using LUKS for full disk encryption  
- Configuring encrypted `/home` and swap partitions  
### 15.2.4 Secure Backup Practices
- Best practices for backup encryption  
- Secure remote backups using `rsync` and `ssh`  
- Automating encrypted backups with `duplicity`  

## 15.3 Mandatory Access Control (SELinux & AppArmor)
### 15.3.1 Understanding Mandatory Access Control (MAC) vs. Discretionary Access Control (DAC)**  
- Difference between SELinux, AppArmor, and traditional Linux permissions  
### 15.3.2 SELinux (Security-Enhanced Linux) - Red Hat Based Systems**  
- SELinux modes: Enforcing, Permissive, Disabled  
- Understanding SELinux contexts and labels  
- Managing SELinux policies using `semanage`, `setsebool`, `audit2allow`  
- Troubleshooting SELinux denials (`ausearch`, `sealert`)  
### 15.3.3 AppArmor - Ubuntu and Debian-Based Systems**  
- Enabling and managing AppArmor profiles  
- Creating custom AppArmor policies  
- Debugging AppArmor violations (`aa-status`, `journalctl`)  

## 15.4 Secure SSH Configuration and Hardening**  
- **Disabling Root Login and Using Key-based Authentication**  
- **Restricting SSH Access with Allow/Deny Rules**  
- **Implementing Two-Factor Authentication for SSH**  
- **Configuring SSH Chroot Jails for Restricted Access**  

## 15.5 Kernel Hardening and Security Enhancements**  
- **Applying Kernel Security Updates Regularly**  
- **Using `sysctl` for System Hardening (`/etc/sysctl.conf`)**  
- **Enabling Address Space Layout Randomization (ASLR)**  
- **Securing `/tmp`, `/var`, and `/home` with Mount Options**  

## 15.6 Network Security and Firewall Management**  
- **Configuring `iptables` and `nftables` for Packet Filtering**
- **Detecting and Preventing Port Scans (`nmap`, `fail2ban`)**  
- **Mitigating DDoS Attacks using Rate Limiting**  

## 15.7 Security Auditing and Compliance**  
- **Using `Lynis` for Linux Security Auditing**  
- **Automating Security Compliance Checks**  
- **Implementing a Logging and Alerting System (`logwatch`, `rsyslog`)**  
