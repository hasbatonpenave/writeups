![[Pasted image 20260312162825.png]]
pour les 4 comptes:
sudo adduser admin_sys
sudo adduser monitoring
sudo adduser deploiement
sudo adduser backup_service

Installation pam:
https://oneuptime.com/blog/post/2026-03-02-how-to-configure-password-complexity-rules-with-pam-on-ubuntu/view
sudo apt install libpam-pwquality 
sudo nano /etc/security/pwquality.conf

![[Pasted image 20260313110024.png]]

 sudo nano /etc/login.defs

![[Pasted image 20260313111754.png]]
![[Pasted image 20260313154549.png]]
```bash
sudo usermod -s /usr/sbin/nologin backup_service
```
Mission 2: 

![[Pasted image 20260313112849.png]]

![[Pasted image 20260313113052.png]]
![[Pasted image 20260313155701.png]]

![[Pasted image 20260313160023.png]]

```bash
sudo apt install openssh-server
```

![[Pasted image 20260313160907.png]]


Mission 3
![[Pasted image 20260313161345.png]]

sudo -u admin_sys ssh-keygen -t ed25519 -C "admin_sys@server" -f /home/admin_sys/.ssh/id_ed25519

sudo -u deploiement ssh-keygen -t ed25519 -C "deploiement@server" -f /home/deploiement/.ssh/id_ed25519

sudo -u monitoring ssh-keygen -t ed25519 -C "monitoring@server" -f /home/monitoring/.ssh/id_ed25519

sudo chattr +i /etc/ssh/sshd_config
sudo nano /etc/ssh/sshd_config

![[Pasted image 20260313162810.png]]
sudo chattr +i /etc/ssh/sshd_config


sudo apt install fail2ban -y 
sudo nano /etc/fail2ban/jail.local

![[Pasted image 20260313163133.png]]

sudo fail2ban-client status sshd