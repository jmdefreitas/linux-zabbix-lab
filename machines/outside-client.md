# Outside-Client VM

## Purpose
Acts as an outside client devices that will try to connect to the server. 

## Resources
- RAM: 1 GB
- CPU: 1 vCPU
- Network Adapters:
  - Adapter 1: NAT

## Configuration
### IP Address
- NAT interface: enp0s3
- IP: 10.0.2.15/24

Outside-client can reach server(10.0.0.20) nginx on port 80 using router vm wan interface (10.0.2.3).

![outside device reaches server](/machines/pics/outside-client%20curl%20server.PNG)