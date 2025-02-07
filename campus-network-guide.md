# Educational Institution Network Implementation Guide

## Required Components

### Core Layer
1. 1x Cisco Catalyst 3560 Layer 3 Switch
   - Supports inter-VLAN routing
   - Supports QoS
   - Multiple Gigabit ports

### Distribution Layer
2. 2x Cisco Catalyst 2960 Switches
   - Support for VLANs
   - Trunk capabilities
   - Port security features

### Access Layer
3. 4x Cisco Catalyst 2960 Switches
   - VLAN support
   - Basic security features
   - Multiple FastEthernet ports

### Wireless Infrastructure
4. 2x Cisco WRT300N Wireless Routers
   - Multiple SSID support
   - VLAN tagging capability
   - WPA2 security

### Servers
5. 1x Server for DHCP
6. 1x Server for DNS

### End Devices (for testing)
7. 6x PCs (2 for each VLAN)
8. 6x Laptops (2 for each VLAN)

### Cabling
9. Straight-through Ethernet cables
   - For device connections
   - Cat6 recommended for core-to-distribution
10. Console cables
    - For initial switch configuration

## Network Addressing Scheme

### VLAN Configuration
- VLAN 10 (Faculty): 192.168.10.0/24
- VLAN 20 (Students): 192.168.20.0/24
- VLAN 30 (Admin): 192.168.30.0/24
- VLAN 40 (Servers): 192.168.40.0/24
- VLAN 99 (Management): 192.168.99.0/24

## Implementation Steps

### Phase 1: Initial Setup

1. Launch Cisco Packet Tracer
   - Open new project
   - Save project with descriptive name
   - Set workspace to logical view

2. Place Core Components
   - Drag Core Switch (3560) to center
   - Name it "CORE-SW"
   - Position at top of topology

3. Place Distribution Switches
   - Add two 2960 switches
   - Name them "DIST-SW1" and "DIST-SW2"
   - Position below core switch

4. Place Access Switches
   - Add four 2960 switches
   - Name them "ACC-SW1" through "ACC-SW4"
   - Position below distribution switches

5. Add Wireless Components
   - Place two WRT300N routers
   - Name them "WAP1" and "WAP2"
   - Connect to nearest access switches

6. Add Servers
   - Place DHCP server
   - Place DNS server
   - Connect to core switch

7. Add End Devices
   - Add PCs and laptops
   - Distribute among access switches
   - Label according to department

### Phase 2: Physical Connections

1. Core to Distribution Links
   ```
   CORE-SW GigabitEthernet1/0/1 → DIST-SW1 GigabitEthernet1/0/1
   CORE-SW GigabitEthernet1/0/2 → DIST-SW2 GigabitEthernet1/0/1
   ```

2. Distribution to Access Links
   ```
   DIST-SW1 GigabitEthernet1/0/2 → ACC-SW1 GigabitEthernet1/0/1
   DIST-SW1 GigabitEthernet1/0/3 → ACC-SW2 GigabitEthernet1/0/1
   DIST-SW2 GigabitEthernet1/0/2 → ACC-SW3 GigabitEthernet1/0/1
   DIST-SW2 GigabitEthernet1/0/3 → ACC-SW4 GigabitEthernet1/0/1
   ```

3. Server Connections
   ```
   CORE-SW GigabitEthernet1/0/23 → DHCP Server
   CORE-SW GigabitEthernet1/0/24 → DNS Server
   ```

### Phase 3: Basic Switch Configuration

1. Core Switch Initial Configuration
   ```
   enable
   configure terminal
   hostname CORE-SW
   enable secret Cisco123
   line console 0
    password console123
    login
   line vty 0 15
    password telnet123
    login
   service password-encryption
   ```

2. Configure VTP on Core Switch
   ```
   vtp mode server
   vtp domain CAMPUS
   vtp password Cisco123
   ```

3. Create VLANs on Core Switch
   ```
   vlan 10
    name FACULTY
   vlan 20
    name STUDENTS
   vlan 30
    name ADMIN
   vlan 40
    name SERVERS
   vlan 99
    name MANAGEMENT
   ```

4. Configure Distribution Switches (Repeat for each)
   ```
   enable
   configure terminal
   hostname DIST-SW1
   vtp mode client
   vtp domain CAMPUS
   vtp password Cisco123
   ```

5. Configure Access Switches (Repeat for each)
   ```
   enable
   configure terminal
   hostname ACC-SW1
   vtp mode client
   vtp domain CAMPUS
   vtp password Cisco123
   ```

### Phase 4: VLAN and Interface Configuration

1. Configure Core Switch SVIs
   ```
   interface vlan 10
    ip address 192.168.10.1 255.255.255.0
   interface vlan 20
    ip address 192.168.20.1 255.255.255.0
   interface vlan 30
    ip address 192.168.30.1 255.255.255.0
   interface vlan 40
    ip address 192.168.40.1 255.255.255.0
   ip routing
   ```

2. Configure Trunk Ports
   ```
   interface range GigabitEthernet1/0/1-4
    switchport mode trunk
    switchport trunk allowed vlan 10,20,30,40,99
   ```

3. Configure Access Ports (Example for ACC-SW1)
   ```
   interface range FastEthernet1/0/1-8
    switchport mode access
    switchport access vlan 10
    switchport port-security
    switchport port-security maximum 2
   ```

### Phase 5: DHCP Configuration

1. Configure DHCP Server
   - IP Address: 192.168.40.2
   - Subnet Mask: 255.255.255.0
   - Default Gateway: 192.168.40.1

2. Create DHCP Pools
   ```
   ip dhcp pool FACULTY
    network 192.168.10.0 255.255.255.0
    default-router 192.168.10.1
    dns-server 192.168.40.10
   
   ip dhcp pool STUDENTS
    network 192.168.20.0 255.255.255.0
    default-router 192.168.20.1
    dns-server 192.168.40.10
   ```

### Phase 6: Wireless Configuration

1. Configure WAP1
   - SSID: Faculty_WiFi
   - VLAN: 10
   - Security: WPA2-Enterprise
   
2. Configure WAP2
   - SSID: Student_WiFi
   - VLAN: 20
   - Security: WPA2-Personal

### Phase 7: Security Implementation

1. Configure ACLs on Core Switch
   ```
   ip access-list extended FACULTY_ACL
    permit ip 192.168.10.0 0.0.0.255 any
    deny ip any any log
   
   ip access-list extended STUDENT_ACL
    permit ip 192.168.20.0 0.0.0.255 192.168.20.0 0.0.0.255
    permit ip 192.168.20.0 0.0.0.255 192.168.40.0 0.0.0.255
    deny ip any any log
   ```

### Phase 8: QoS Configuration

1. Basic QoS on Core Switch
   ```
   mls qos
   interface range GigabitEthernet1/0/1-24
    mls qos trust cos
    queue-set 2
   ```

### Phase 9: Testing and Verification

1. VLAN Verification
   ```
   show vlan brief
   show interfaces trunk
   ```

2. DHCP Verification
   ```
   show ip dhcp binding
   show ip dhcp pool
   ```

3. Connectivity Testing
   - Ping between devices in same VLAN
   - Ping between devices in different VLANs
   - Test wireless connectivity
   - Verify DHCP assignment
   - Test DNS resolution
