# Networking

# 1. Configuring IP Addressing
   
a) Assigning addresses for RTR-L:
Network: 10.10.10.0/27
VLAN 10: 14 hosts (10.10.10.1 - 10.10.10.14)
RTR-L: 10.10.10.1/27
VLAN 20: 6 hosts (10.10.10.17 - 10.10.10.22)
RTR-L: 10.10.10.17/27

b) Assigning addresses for RTR-R:
Network: 20.20.20.0/27
VLAN 10: 6 hosts (20.20.20.1 - 20.20.20.6)
RTR-R: 20.20.20.1/27
VLAN 20: 2 hosts (20.20.20.9 - 20.20.20.10)
RTR-R: 20.20.20.9/27

c) Assigning addresses for PCs:
PC1: 10.10.10.2/27
PC2: 10.10.10.18/27
PC3: 20.20.20.2/27
PC4: 20.20.20.10/27
d) Assigning addresses between routers:
RTR-L: 30.30.30.1/30
RTR-R: 30.30.30.2/30

# 2. Configuring Loopback Interfaces

## RTR-L
ip address add 1.1.1.1/32 dev lo

## RTR-R
ip address add 2.2.2.2/32 dev lo


3. Configuring VLANs in Open vSwitch
## SW1
ovs-vsctl add-br br0
ovs-vsctl add-port br0 ens20 tag=10
ovs-vsctl add-port br0 ens21 tag=20

## SW2
ovs-vsctl add-br br0
ovs-vsctl add-port br0 ens20 tag=10
ovs-vsctl add-port br0 ens21 tag=20

# 4. Configuring BGP between Routers

## RTR-L (AS 65001)
router bgp 65001
  neighbor 30.30.30.2 remote-as 65002
  network 10.10.10.0/27
  network 1.1.1.1/32

## RTR-R (AS 65002)
router bgp 65002
  neighbor 30.30.30.1 remote-as 65001
  network 20.20.20.0/27
  network 2.2.2.2/32

# 5. Configuring OSPF for Dynamic Routing
   
## RTR-L
router ospf
  network 10.10.10.0/27 area 0
  network 30.30.30.0/30 area 0

## RTR-R
router ospf
  network 20.20.20.0/27 area 0
  network 30.30.30.0/30 area 0

# 6. Enabling Telnet Connectivity between PC
   
## PCn (Setting up the Telnet server)
sudo systemctl enable xinetd
sudo systemctl start xinetd
sudo useradd -m usertelnet
echo "usertelnet:P@ssw0rdTelnet" | sudo chpasswd

## Connecting from PCm
telnet PC1
