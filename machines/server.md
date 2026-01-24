# Server VM

## Purpose
Acts as server for outbound devices to communicate with internal server. 

## Resources
- RAM: 1 GB
- CPU: 1 vCPU
- Network Adapters:
  - Adapter 1: Internal Network (lab_net)

## Configuration
### IP Address
- NAT interface: enp0s3
- IP: 10.0.0.20/24
- Default Gateway: 10.0.0.1

```bash
sudo ip addr add 10.0.0.20/24 dev enp0s3
sudo ip route add default via 10.0.0.1
```

![server ip a ip route command](/machines/pics/server.PNG)

### DNS
via /etc/resolv.conf file

![cat /etc/resolv.conf](/machines/pics/server-cat-resolf.PNG)

### SSH
- After Internet connection and DNS server is established I can install openssh-server.

```bash
sudo apt install openssh-server -y
sudo systemctl status ssh
```

![server ssh status](/machines/pics/ssh-server.PNG)


### Persistent Network Configuration

- Using /etc/netplan/01-network-manager-all.yaml file

![client yaml file](/machines/pics/server-yaml-file.PNG)


## Secure Remote Administration (SSH)

Implemented key-based SSH authentication for server administration.

Security measures:
- Disabled root SSH login
- Disabled password authentication
- Restricted SSH access to LAN interface
- Limited SSH access to admin user only

Verification:
- Client connects without password prompt
- SSH service listens only on 10.0.0.20
- Configuration persists after reboot

![server ssh config](/machines/pics/server-ssh-config.PNG)
![client to server ssh](/machines/pics/client-to-server-ssh.PNG)



### Firewall
- Device Hardening (Only allow services that are used) using ufw command (iptables are more complicated)

* Reset & enable logging
```bash
sudo ufw reset
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw logging on
```

* Allow only ssh on port 22
```bash
sudo ufw allow from 10.0.0.10 to any port 22 proto cp
```

* Enable and verify
```bash
sudo ufw enable
sudo ufw status verbose
```

![server ufw status verbose](/machines/pics/server-ufw-status-verbose.PNG)

* Blocking attempts to log into a different port (111, 222, 333 as examples)
![blocked ssh attempts into wrong ports](/machines/pics/server-ssh-logs.PNG)

### Creating a webserver with NGINX

- install and enable nginx
- create simple webpage on /var/www/html/index.html
![nginx stats](/machines/pics/server-nginx.PNG)
![website](/machines/pics/simple%20web%20server.PNG)
- allow connection to port 80
```bash
sudo ufw allow 80/tcp
```