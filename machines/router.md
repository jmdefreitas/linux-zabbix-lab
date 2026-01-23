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
- Interface: enp0s9
- IP: 10.0.0.1/24

- NAT interface: enp0s3
- IP: 10.0.2.15/24

![ip a command router](/machines/pics/router-ip-a.PNG)

### IP Forwarding
Enabled via /etc/sysctl.conf

![sysctl.conf router](/machines/pics/router-cat-sysctl.PNG)

### NAT
iptables masquerade rule applied on NAT interface
