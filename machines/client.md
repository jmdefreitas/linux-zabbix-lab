# Client VM

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
- Default Gateway: 10.0.0.1

```bash
sudo ip addr add 10.0.0.10/24 dev enp0s3
sudo ip route add default via 10.0.0.1
```

![ip a command client machine](/machines/pics/client.PNG)

### Testing Internet Access
- PING uses default gateway to reach the Router and then reaches the internet

```bash
ping -c 4 8.8.8.8
```

![ping command](/machines/pics/client%20ping.PNG)

### DNS
via /etc/resolv.conf file

![cat /etc/resolv.conf](/machines/pics/client-cat-resolve.PNG)

### SSH
- Client to server connection using ssh

![ssh client to server](/machines/pics/ssh%20client.PNG)


### Persistent Network Configuration

- Using /etc/netplan/01-network-manager-all.yaml file

![client yaml file](/machines/pics/client-yaml-file.PNG)

### Firewall AND server access

- trying to ssh into the server using the wrong port from client

![ssh into wrong port](/machines/pics/client-ssh-wrong-port.PNG)

- trying to access nginx server on the server VM. Connection reject due to server firewall
- After change server firewall rules, connection allowed
![blocked connection to website](/machines/pics/failed-connection-to-website.PNG)

### DNS local file
- Assigning a local domain name for http://10.0.0.20 using /etc/hosts
![domain name for server](/machines/pics/myserver-local.PNG)

### Dynamic IP addresses with isc-dhcp-server
- Changing network manager yaml file to use dhcp4 server
![new network manager yaml file](/machines/pics/client%20new%20network%20yaml%20file.PNG)

- Flushing Static IP address, deleting default route and requesting a new ip address from the router.
- Client can still reach server nginx due to ip reservation to the server

```bash
sudo ip addr flush dev enp0s3
sudo ip route del default
sudo netplan apply
```

- Static ip to client was 10.0.0.10 now is 10.0.0.41
![server new ip address](/machines/pics/client%20new%20ip%20address.PNG)