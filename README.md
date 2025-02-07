# College_Campus_NetworkHere's a **short and clear README** for your GitHub project:  

---

# **VLAN Network Configuration Project**  

## **Overview**  
This project sets up a **VLAN-based network** with multiple departments using **Cisco Packet Tracer**. It includes VLAN segmentation, inter-VLAN routing, and DHCP configuration for efficient IP management.  

## **Network Topology**  
- **VLAN 10 (Faculty)** → `192.168.10.0/24`  
- **VLAN 20 (Students)** → `192.168.20.0/24`  
- **VLAN 30 (Admin)** → `192.168.30.0/24`  
- **Core Switch** manages VLAN routing  
- **DHCP Server** assigns IPs dynamically  

## **Key Features**  
✅ VLAN-based segmentation for security  
✅ Trunking and Inter-VLAN Routing  
✅ DHCP Configuration for automatic IP allocation  
✅ End devices assigned static and dynamic IPs  

## **Setup Instructions**  
1. Open the **Packet Tracer** project file.  
2. Verify VLAN configurations using:  
   ```cisco
   show vlan brief
   ```  
3. Ensure trunk ports are configured:  
   ```cisco
   switchport mode trunk
   ```  
4. Check DHCP assignments using:  
   ```cisco
   show ip dhcp binding
   ```  

## **Project Files**  
- `CNN_DA_1.pkt` → Packet Tracer project file  
  

## **Contributors**  
- [Ashutosh Kumar]  

## **License**  
📜 MIT License  
