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

# IP Addressing Scheme (static)

| Machine | Interface | IP Address |
|---------|-----------|------------|
| Router  | enp0s3    | 10.0.2.15  |
| Router  | enp0s8    | 10.0.0.1   |
| Server  | enp0s3    | 10.0.0.20  |
| Client  | enp0s3    | 10.0.0.10  |

Subnet: 10.0.0.0/24
Gateway: 10.0.0.1

Client (10.0.0.10) -> Router (10.0.0.1) -> NAT -> Internet
Server (10.0.0.20) ->

## Current Status
✔ Routing configured  
✔ NAT working  
✔ Internal communication verified

## Firewall Security

Allowed:
    - Client -> Server : SSH 22




Track 2: Firewalling & Network Control (CORE NETWORKING)
Track 3: Turn Server Into a Real Server
Track 4: Break & Fix 
Track 5: Monitoring & Visibility