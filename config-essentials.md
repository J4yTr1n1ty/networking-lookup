# Router and Switch Configuration Structure (Essentials Only)

## 1. Set Hostname

```
hostname <DEVICE_NAME>
```

- Names the router or switch for easy identification.

## 2. Interface IP Address (Router)

```
interface GigabitEthernet0/X
ip address <IP_ADDRESS> <SUBNET_MASK>
```

- Assigns IP and subnet to a router's interface.

## 3. DHCP Relay on Router Interface

```
interface GigabitEthernet0/X
ip helper-address <DHCP_SERVER_IP>
```

- Forwards DHCP requests to the server.

## 4. VLAN Subinterface (Router)

```
interface GigabitEthernet0/0.<VLAN_ID>
encapsulation dot1Q <VLAN_ID>
ip address <IP_ADDRESS> <SUBNET_MASK>
[ip helper-address <DHCP_SERVER_IP>]
```

- Used for inter-VLAN routing (router-on-a-stick).

## 5. Static Routes (Router)

```
ip route <DESTINATION_NETWORK> <SUBNET_MASK> <NEXT_HOP_IP>
Examples:
ip route 0.0.0.0 0.0.0.0 192.168.1.
ip route 192.168.22.0 255.255.255.0 192.168.1.
```

- Manually defines where to send traffic.

## 6. Create VLAN (Switch)

```
vlan <VLAN_ID>
name <OPTIONAL_NAME>
exit
```

- Adds a VLAN to the switch's VLAN database.

## 7. Assign Access Port (Switch)

```
interface FastEthernet0/X
switchport access vlan <VLAN_ID>
```

- Places the port into a specific VLAN.

## 8. Configure Trunk Port (Switch)

```
interface <INTERFACE>
switchport mode trunk
switchport trunk allowed vlan 10,20,30
```

- Sets exactly which VLANs are allowed on the trunk.
- To add VLAN 200: must manually include it in the list.
  e.g., switchport trunk allowed vlan 1-200,201-1005 (or switchport trunk allowed vlan 1-1005)
- To reset to default (all VLANs allowed):
  no switchport trunk allowed vlans

## 9. Useful Show Commands

# Router and Switch Configuration Structure (Essentials Only)

```
show interfaces status! Link/port state
show vlan brief! VLANs on switch
show interfaces switchport! Access/trunk info
show interfaces trunk! Trunked VLANs
show ip interface brief! IP assignment
show ip route! Routing table
```

## Troubleshooting Checklist (Simplified for Exams)

## 1. PC Network Configuration

- Is the PC using the correct IP address and subnet mask?
- Do both ends of the Ethernet link have matching speed/duplex settings (or both set to auto)?
- Does the interface have a green light (no red arrow)?

## 2. Routing Issues

- Does the router have a default route (`ip route 0.0.0.0`)?
- Are there static routes for all required networks?
- Can neighboring routers reach each other?

## 3. VLAN Problems

- Has the required VLAN been created on the switch?
- Is the VLAN assigned correctly to access ports (e.g., for PCs or servers)?
- Is the VLAN included on the trunk link between switches or routers?
- Are the trunk ports properly configured (`switchport mode trunk`)?

## 4. Device-to-Device Connectivity

- Devices in the same VLAN and subnet can't communicate?

## -> Check if they're in the same VLAN and have matching IP settings

- Devices in different VLANs can't communicate?

## -> Check router subinterfaces or SVIs, VLAN tagging, and routes

## 5. Tools and Checks

- Use 'show' commands to verify interfaces, VLANs, routes, and trunk status.
- Use cable color (green/red) in Packet Tracer to identify link issues quickly.

## Tips

- Most problems are due to simple misconfigurations: wrong VLAN, IP mismatch, missing route, or disabled port.
