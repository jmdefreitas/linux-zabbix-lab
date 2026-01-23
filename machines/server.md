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