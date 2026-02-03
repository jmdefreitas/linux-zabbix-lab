# Linux Networking Lab

## Goal
Build a virtual Linux lab to practice networking fundamentals, monitoring and basic troubleshooting:
- Routing
- NAT
- Internal networks
- Linux services
- Zabbix

## Lab Topology

![lab topology](/machines/pics/lab-topology.drawio.png)

## Topology
- Router VM (NAT + DHCP + Internal)
- Server VM (Internal) lab_net
- Client VM (Internal) lab_net

# IP Addressing Scheme 

| Machine       | Interface | IP Address   |
|---------------|-----------|--------------|
| Router        | enp0s3    | 10.0.2.15    |
| Router        | enp0s8    | 192.168.1.1  |
| Client        | enp0s8    | 192.168.1.10 |
| Web Server    | enp0s8    | 192.168.1.20 |
| Zabbix Server | enp0s8    | 192.168.1.30 |


Subnet: 192.168.1.0/24
Gateway: 192.168.1.1
