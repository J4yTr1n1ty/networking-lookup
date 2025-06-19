# 🧷 Router and Switch Configuration Structure (Essentials Only)

## 1. Set Hostname

```bash
hostname <DEVICE_NAME>
```

- Names the router or switch for easy identification.

## 2. Interface IP Address (Router)

```bash
interface GigabitEthernet0/X
 ip address <IP_ADDRESS> <SUBNET_MASK>
```

- Assigns IP and subnet to a router's interface.

## 3. DHCP Relay on Router Interface

```bash
interface GigabitEthernet0/X
 ip helper-address <DHCP_SERVER_IP>
```

- Forwards DHCP requests to the server.

## 4. VLAN Subinterface (Router)

```bash
interface GigabitEthernet0/0.<VLAN_ID>
 encapsulation dot1Q <VLAN_ID>
 ip address <IP_ADDRESS> <SUBNET_MASK>
 [ip helper-address <DHCP_SERVER_IP>]
```

- Used for inter-VLAN routing (router-on-a-stick).

## 5. Static Routes (Router)

```bash
ip route <DESTINATION_NETWORK> <SUBNET_MASK> <NEXT_HOP_IP>
```

Examples:

```bash
ip route 0.0.0.0 0.0.0.0 192.168.1.1
ip route 192.168.22.0 255.255.255.0 192.168.1.9
```

- Manually defines where to send traffic.

## 6. Create VLAN (Switch)

```bash
vlan <VLAN_ID>
 name <OPTIONAL_NAME>
exit
```

- Adds a VLAN to the switch's VLAN database.

## 7. Assign Access Port (Switch)

```bash
interface FastEthernet0/X
 switchport access vlan <VLAN_ID>
```

- Places the port into a specific VLAN.

## 8. Configure Trunk Port (Switch)

```bash
interface <INTERFACE>
 switchport mode trunk
```

- Allows multiple VLANs on a link to another switch or router.

## 9. Allowed VLANs on Trunks (Switch)

```bash
interface <INTERFACE>
 switchport trunk allowed vlan 1-199,201-1005
 switchport mode trunk
```

- Sets exactly which VLANs are allowed on the trunk.
- To **add VLAN 200**, you must expand the list:

```bash
switchport trunk allowed vlan 1-200,201-1005
```

(or simply)

```bash
switchport trunk allowed vlan 1-1005
```

- To reset to default (all VLANs allowed):

```bash
no switchport trunk allowed vlan
```

---

# 🛠 Useful Show Commands

| Purpose             | Command                      |
| ------------------- | ---------------------------- |
| Link status         | `show interfaces status`     |
| VLAN existence      | `show vlan brief`            |
| Port mode           | `show interfaces switchport` |
| Trunk configuration | `show interfaces trunk`      |
| IP interface status | `show ip interface brief`    |
| Routing table       | `show ip route`              |

---

# 🧪 Troubleshooting Checklist (Simplified for Exams)

## 1. PC Network Configuration

- Is the IP address and subnet mask correct?
- Do both ends of the link use matching speed/duplex (or both `auto`)?
- Is the link up (green, not red arrow)?

## 2. Routing Issues

- Is a default route (`ip route 0.0.0.0`) present?
- Are all needed static routes configured?
- Can neighboring routers reach each other?

## 3. VLAN Problems

- Was the VLAN created on the switch?
- Is the access port assigned to the correct VLAN?
- Is the VLAN **included** in the trunk's allowed list?
- Is the trunk port properly configured?

## 4. Device-to-Device Communication

- **Same subnet/VLAN**: check VLAN assignment and IP/subnet correctness.
- **Different subnets/VLANs**: check router-on-a-stick subinterfaces (or SVIs) and static routes.

## 5. Tools and Observations

- Use `show` commands to inspect interfaces, VLANs, routes, trunks.
- In Packet Tracer, red cables or port arrows indicate link problems.
- Most issues stem from simple misconfigurations: wrong VLAN, missing route, incorrect IP/subnet, or trunk mismatch.
