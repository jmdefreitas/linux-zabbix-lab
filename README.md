# Linux Networking Lab

## Goal
Build a virtual Linux lab to practice networking fundamentals:
- Routing
- NAT
- Internal networks
- Linux services

## Environment
- Host RAM: 12 GB
- Hypervisor: VirtualBox
- OS: Lubuntu 24.04.3

## Topology
- Router VM (NAT + Internal)
- Server VM (Internal) lab_net
- Client VM (Internal) lab_net

# IP Addressing Scheme 
* dhcp server running on router vm

| Machine     | Interface | IP Address |
|---------    |-----------|------------|
| Router      | enp0s3    | 10.0.2.3   |
| Router      | enp0s8    | 10.0.0.1   |
| Server      | enp0s3    | 10.0.0.20  | * reserved
| Client      | enp0s3    | DHCPv4     |
| Outside VM  | enp0s3    | 10.0.2.15  |

Subnet: 10.0.0.0/24
Gateway: 10.0.0.1

#Todo: fix this
Client (10.0.0.10)| -> Router (10.0.0.1) -> NAT -> Internet
Server (10.0.0.20)| 

## Current Status
✔ Routing configured  
✔ NAT working  
✔ Internal communication verified

