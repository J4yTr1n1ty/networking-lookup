# Cisco CLI Operations Guide for Packet Tracer

## CLI Navigation and Modes

### Access Modes
```
Router> (User EXEC mode)
Router# (Privileged EXEC mode)
Router(config)# (Global Configuration mode)
Router(config-if)# (Interface Configuration mode)
Router(config-vlan)# (VLAN Configuration mode)
Router(config-router)# (Router Configuration mode)
```

### Basic Navigation
```
enable                    # Enter privileged mode
configure terminal        # Enter global config mode
exit                     # Go back one level
end                      # Return to privileged mode
```

## Interface Configuration

### Basic Interface Setup
```
interface fastethernet0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
description "LAN Interface"
```

### Serial Interface Configuration
```
interface serial0/0/0
ip address 10.0.0.1 255.255.255.252
clock rate 64000         # On DCE side only
no shutdown
```

### Interface Status and Information
```
show interfaces
show ip interface brief
show interfaces status
```

## VLAN Configuration

### Creating VLANs on Switch
```
vlan 10
name Sales
vlan 20
name Engineering
vlan 30
name Management
```

### Assigning Ports to VLANs
```
interface fastethernet0/1
switchport mode access
switchport access vlan 10

interface range fastethernet0/2-5
switchport mode access
switchport access vlan 20
```

### Trunk Configuration
```
interface fastethernet0/24
switchport mode trunk
switchport trunk allowed vlan 10,20,30
```

### VLAN Verification
```
show vlan brief
show interfaces trunk
show interfaces switchport
```

## Inter-VLAN Routing

### Router-on-a-Stick Configuration
```
interface fastethernet0/0
no shutdown

interface fastethernet0/0.10
encapsulation dot1q 10
ip address 192.168.10.1 255.255.255.0

interface fastethernet0/0.20
encapsulation dot1q 20
ip address 192.168.20.1 255.255.255.0
```

### Layer 3 Switch Configuration
```
ip routing

interface vlan 10
ip address 192.168.10.1 255.255.255.0
no shutdown

interface vlan 20
ip address 192.168.20.1 255.255.255.0
no shutdown
```

## Static Routing

### Adding Static Routes
```
ip route 192.168.2.0 255.255.255.0 10.0.0.2
ip route 0.0.0.0 0.0.0.0 10.0.0.2              # Default route
```

### Viewing Routing Table
```
show ip route
show ip route static
```

## Dynamic Routing

### RIP Configuration
```
router rip
version 2
network 192.168.1.0
network 10.0.0.0
no auto-summary
```

### OSPF Configuration
```
router ospf 1
network 192.168.1.0 0.0.0.255 area 0
network 10.0.0.0 0.0.0.3 area 0
```

### EIGRP Configuration
```
router eigrp 100
network 192.168.1.0
network 10.0.0.0
no auto-summary
```

## DHCP Configuration

### DHCP Pool Setup
```
ip dhcp pool LAN_POOL
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
dns-server 8.8.8.8
lease 7

ip dhcp excluded-address 192.168.1.1 192.168.1.10
```

### DHCP Verification
```
show ip dhcp binding
show ip dhcp pool
show ip dhcp conflict
```

## Access Control Lists (ACLs)

### Standard ACL
```
access-list 10 permit 192.168.1.0 0.0.0.255
access-list 10 deny any

interface fastethernet0/1
ip access-group 10 in
```

### Extended ACL
```
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 443
access-list 100 deny ip any any

interface fastethernet0/0
ip access-group 100 out
```

### Named ACL
```
ip access-list extended WEB_TRAFFIC
permit tcp 192.168.1.0 0.0.0.255 any eq 80
permit tcp 192.168.1.0 0.0.0.255 any eq 443
deny ip any any
```

## NAT Configuration

### Static NAT
```
ip nat inside source static 192.168.1.10 203.0.113.10

interface fastethernet0/0
ip nat inside

interface serial0/0/0
ip nat outside
```

### Dynamic NAT with Overload (PAT)
```
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 interface serial0/0/0 overload

interface fastethernet0/0
ip nat inside

interface serial0/0/0
ip nat outside
```

## Security Configuration

### Console Password
```
line console 0
password cisco
login
```

### VTY (Telnet/SSH) Password
```
line vty 0 4
password cisco
login
```

### Enable Password
```
enable password cisco
enable secret cisco123        # Encrypted version
```

### SSH Configuration
```
hostname Router1
ip domain-name example.com
crypto key generate rsa
ip ssh version 2

username admin privilege 15 secret admin123
line vty 0 4
transport input ssh
login local
```

## Spanning Tree Protocol

### STP Configuration
```
spanning-tree mode rapid-pvst
spanning-tree vlan 1 root primary
spanning-tree vlan 10 root secondary

interface fastethernet0/1
spanning-tree portfast
spanning-tree bpduguard enable
```

### STP Verification
```
show spanning-tree
show spanning-tree vlan 1
show spanning-tree interface fastethernet0/1
```

## Port Security

### Basic Port Security
```
interface fastethernet0/1
switchport mode access
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation shutdown
```

### Port Security Verification
```
show port-security
show port-security interface fastethernet0/1
show port-security address
```

## Troubleshooting Commands

### Connectivity Testing
```
ping 192.168.1.1
traceroute 192.168.1.1
telnet 192.168.1.1
```

### Interface Diagnostics
```
show controllers serial0/0/0
show cdp neighbors
show cdp neighbors detail
show arp
```

### System Information
```
show version
show running-config
show startup-config
show flash
show processes
show memory
```

### Protocol-Specific Troubleshooting
```
show ip protocols
show ip ospf neighbor
show ip eigrp neighbors
show ip nat translations
show access-lists
```

## Configuration Management

### Saving Configuration
```
copy running-config startup-config
write memory                    # Alternative command
```

### Loading Configuration
```
copy startup-config running-config
```

### Backup Configuration to TFTP
```
copy running-config tftp
copy startup-config tftp
```

### Factory Reset
```
erase startup-config
reload
```

## Common Packet Tracer Scenarios

### Basic Router Setup
1. Configure hostname and passwords
2. Set up interfaces with IP addresses
3. Configure static routes or dynamic routing
4. Test connectivity with ping

### Switch Configuration
1. Create VLANs
2. Assign ports to VLANs
3. Configure trunk links
4. Set up inter-VLAN routing

### DHCP Server Setup
1. Configure DHCP pool
2. Set excluded addresses
3. Apply to appropriate interface
4. Test client connectivity

### Security Implementation
1. Set up passwords and user accounts
2. Configure ACLs for traffic filtering
3. Implement port security on switches
4. Set up SSH for remote access

## Tips for Packet Tracer

- Use `?` for command help at any point
- Tab completion works for commands
- Use `show` commands frequently to verify configurations
- Always use `no shutdown` on interfaces
- Save configurations regularly
- Use simulation mode to see packet flow
- Check cable types (straight-through vs crossover)