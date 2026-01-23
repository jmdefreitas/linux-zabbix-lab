# Router VM

## Purpose
Acts as a gateway between the internal lab network and the internet.

## Resources
- RAM: 2 GB
- CPU: 1 vCPU
- Network Adapters:
  - Adapter 1: NAT
  - Adapter 2: Internal Network (lab_net)

## Configuration
### IP Address
- NAT interface: enp0s3
- IP: 10.0.2.15/24

- Interface: enp0s9
- IP: 10.0.0.1/24

![ip a command router](/machines/pics/router-ip-a.PNG)

### IP Forwarding
Enabled via /etc/sysctl.conf

![sysctl.conf router](/machines/pics/router-cat-sysctl.PNG)

### Enable IP Forwarding
```bash
sudo nano /etc/sysctl.conf
# Uncomment net.ipv4.ip_forward=1
sudo sysctl -p
```

![enable ip forwarding](/machines/pics/enable%20ip%20forwarding.PNG)

### NAT + Interface Forwarding
```bash
# -t nat target NAT
# -o enp0s3 specifies the outbound interface
# -j MASQUERADE hides the private IP addresses
sudo iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE
# Allows outbound traffic
sudo iptables -A FORWARD -i enp0s8 -o enp0s3 -j ACCEPT
# Allows inbound traffic
sudo iptables -A FORWARD -i enp0s3 -o enp0s8 -m state --state ESTABLISHED,RELATED -j ACCEPT
```
