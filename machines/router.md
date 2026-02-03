# Router VM

dhcp configuration file - /etc/dhcp/dhcp.conf
dhcp enp0s8 ip statically assigned - /etc/netplan/01
tell dhcp server to listen on enp0s8 interface - /etc/default/isc-dhcp-server

## Purpose
- Acts as a gateway between the internal lab network and the internet.7
- Acts as gate between outside devices and server

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

Breaking NAT with 
```bash 
iptables -t nat -F
```
stops internet connection to both the server and client.

## Persistent Network Configuration

Static IPs were configured using Netplan to ensure consistency
across reboots.

- Router LAN: 10.0.0.1/24
- Client: 10.0.0.10/24
- Server: 10.0.0.20/24
- Default gateway: 10.0.0.1

NAT and IP forwarding were made persistent using sysctl and
iptables-persistent.

![router yaml file](/machines/pics/router-netplan-yaml-file.PNG)

- test and apply changes

```bash
sudo netplay try
sudo netplay apply
```

### Roter Firewall
- Set a Default-Deny forward policy

```bash
sudo iptables -P FORWARD DROP
```

- Allow established and related connection (STATEFUL)
```bash
# -A FORWARD → append rule to FORWARD chain
# -m conntrack → enable connection tracking
# ESTABLISHED → packets that belong to an existing connection
# RELATED → helper traffic (FTP, ICMP errors, etc.)
# -j ACCEPT → allow it
sudo iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```

### Dynamic IP addresses with isc-dhcp-server
- Configuring dhcp server:
  ip address range
  default gateway
  subnet mask
  nameserver
  ip address reservation for the server vm

![dhcp server config file](/machines/pics/dhcpd.conf%20configuration.PNG)

- Checking router dhcp server log 
```bash
sudo journalctl -u isc-dhcp-server -f
```

![router's dhcp server log](/machines/pics/router%20dhcp%20log.PNG)


### Exposing the server 
- Outside devices should not have access to internal server using 10.0.0.2
- Outside devices should communicate with router wan interface and router forwards those requests to its internal interface reaching the server

Allow nat on internal interface
Add port forwading DNAT
Add stateful firewall forwading 

```bash
sudo iptables -t nat -A POSTROUTING -o enp0s8 -j MASQUERADE
sudo iptables -t nat -A PREROUTING -i enp0s3 -p tcp --dport 80 -j DNAT --to-destination 10.0.0.20:80
sudo iptables -A FORWARD -p tcp -d 10.0.0.20 --dport 80 -j ACCEPT
sudo iptables -A FORWARD -m state --state ESTABLISHED,RELATED -j ACCEPT
```

![router ip table](/machines/pics/router%20iptables.PNG)