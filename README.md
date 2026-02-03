# Linux Networking Lab

## Goal

This lab simulates a small monitored network.
Only the router VM has Internet access via NAT.
All internal VMs reside on an isolated network.
The Zabbix server monitors the web server and client
using agent-based checks.

## Lab Topology

![lab topology](/machines/new-pics/lab-topology.drawio.png)

## Zabbix UI 
- 4 VMs connected and active

![zabbix dashboard](/machines/new-pics/zabbix-ui.PNG)


## Topology
- Router VM (NAT + DHCP + Internal)
- Server VM (Internal) la
- Client VM (Internal) lab_net

# IP Addressing Scheme 

| Machine       | Interface | IP Address   |
|---------------|-----------|--------------|
| Router        | enp0s3    | 10.0.2.15    |
| Router        | enp0s8    | 192.168.1.1  |
| Router        | enp0s9    | 192.168.2.1  |
| Router        | enp0s10   | 192.168.3.1  |
| Client        | enp0s3    | 192.168.1.10 |
| Web Server    | enp0s3    | 192.168.2.20 |
| Zabbix Server | enp0s3    | 192.168.3.30 |
