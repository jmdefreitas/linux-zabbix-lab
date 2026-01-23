# Router VM

## Purpose
Acts as private client devices inside a LAN. 

## Resources
- RAM: 1 GB
- CPU: 1 vCPU
- Network Adapters:
  - Adapter 1: Internal Network (lab_net)

## Configuration
### IP Address
- NAT interface: enp0s3
- IP: 10.0.0.10/24

![ip a command client machine](/machines/pics/client-ip-a.PNG)

### DNS
via /etc/resolv.conf file

![cat /etc/resolv.conf](/machines/pics/client-cat-resolve.PNG)

### Internet
Uses default gateway (10.0.0.1) for internet access

![ip route client](/machines/pics/client-ip-route.PNG)