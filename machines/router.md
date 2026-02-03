# Router VM

## Purpose
- Acts as a gateway between the internal network and the internet.

## Resources
- RAM: 2 GB
- CPU: 1 vCPU
- Network Adapters:
  - Adapter 1: NAT
  - Adapter 2: Internal Network (intnet-client)
  - Adapter 3: Internal Network (intnet-server)
  - Adapter 4: Internal Network (intnet-zabbix)

## Configuration
### IP Address
- NAT interface: enp0s3
- IP: 10.0.2.15/24

- Client Interface: enp0s8
- IP: 192.168.1.1/24

- Server Interface: enp0s9
- IP: 192.168.2.1/24

- Zabbix Interface: enp0s10
- IP: 192.168.3.1/24

![ip addresses](/machines/new-pics/ip%20addresses%20router-vm.PNG)

## Routing

### 1: Network Configuration

Static IPs were configured using Netplan utility to ensure consistency
across reboots.

![netplan.yaml file](/machines/new-pics/netplan-yaml%20file.PNG)

```bash
# test and apply changes
sudo netplay apply
```

### 2: Dynamic IP addresses with isc-dhcp-server
- DHCP configuration for all subnets on dhcpd.conf file.

![dhcpd.conf](/machines/new-pics/dhcpd.conf%20file.PNG)

```bash
# Checking dhcp server log 
sudo journalctl -u isc-dhcp-server -f
```

### 3: Enable IP Forwarding
```bash
sudo nano /etc/sysctl.conf
# Uncomment net.ipv4.ip_forward=1
sudo sysctl -p
```

### 4: Firewall & NAT reset

```bash
sudo iptables -F 
sudo iptables -t nat -F 
sudo iptables -X 
sudo iptables -P INPUT DROP 
sudo iptables -P FORWARD DROP 
sudo iptables -P OUTPUT ACCEPT 
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```

- This "resets" the iptables and configures it with a default-deny policy for inbound and forwarded traffic while allowing all outbound traffic;

```bash
# Allow ICMP (ping) to the router from LANs (optional but helpful)
sudo iptables -A INPUT -i enp0s8 -p icmp -j ACCEPT
sudo iptables -A INPUT -i enp0s9 -p icmp -j ACCEPT
sudo iptables -A INPUT -i enp0s10 -p icmp -j ACCEPT
```

### 5: NAT + Interface Forwarding

```bash
# -t nat target NAT
# -o enp0s3 specifies the outbound interface
# -j MASQUERADE hides the private IP addresses
sudo iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE

# Allow established and related forwarded traffic
sudo iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Allow LANs to reach the Internet (tighten later if you want)
sudo iptables -A FORWARD -i enp0s8 -o enp0s3 -s 192.168.1.0/24 -j ACCEPT
sudo iptables -A FORWARD -i enp0s9 -o enp0s3 -s 192.168.2.0/24 -j ACCEPT
sudo iptables -A FORWARD -i enp0s10 -o enp0s3 -s 192.168.3.0/24 -j ACCEPT
```

![router iptables](/machines/new-pics/router%20iptables.PNG)