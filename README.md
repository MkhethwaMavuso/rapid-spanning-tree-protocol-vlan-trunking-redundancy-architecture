# rapid-spanning-tree-protocol-vlan-trunking-redundancy-architecture
Multi-Tier Enterprise Campus Network featuring a central Layer 3 gateway/DHCP router (2911 R1), dual redundant Layer 3 distribution switches (Dist-1 and Dist-2), departmental access switches (Acc-1 for First Floor VLAN 1 and Acc-2 for Second Floor VLAN 2), end-user hosts, and an isolated upstream ISP edge (8.8.8.8).

🌐 Quick Overview & Network Specs🟢 Architecture & Topology: Multi-Tier Enterprise Campus Network featuring a central Layer 3 gateway/DHCP router (2911 R1), dual redundant Layer 3 distribution switches (Dist-1 and Dist-2), departmental access switches (Acc-1 for First Floor VLAN 1 and Acc-2 for Second Floor VLAN 2), end-user hosts, and an isolated upstream ISP edge (8.8.8.8).   🔵 VLAN & Subnet Allocations:VLAN 1 (First Floor): Managing end-user devices PC0 and PC1 across subnet 192.168.1.0/24.   VLAN 2 (Second Floor): Managing end-user devices PC2 and PC3 across subnet 192.168.2.0/24.   WAN / ISP Transit Link: Point-to-point connection (203.0.113.2/30) connecting gateway router R1 to the upstream 2911 ISP router.   🟣 Spanning Tree Protocol (STP / RSTP) Root Bridge Hierarchy:Dist-1 is designated as the Root Bridge across all active VLANs ("This bridge is the root in all VLANs"), ensuring deterministic, loop-free Layer 2 paths and optimized forwarding topologies.   💡 Key Technical Insights & What This Lab Demonstrates🔍 Rapid Spanning Tree Protocol (802.1w / RSTP) Convergence: Demonstrates sub-second topology convergence compared to legacy 802.1D STP by utilizing point-to-point link types, proposal-agreement handshake mechanisms, and alternative/backup port roles.   ⚡ Layer 2 Path Redundancy & Loop Prevention: Shows how dual-homed uplinks between access switches (Acc-1, Acc-2) and distribution switches (Dist-1, Dist-2) provide seamless failover protection while blocking redundant loops (indicated by designated blocking/alternate orange port states on redundant links).   🛠️ Inter-VLAN Routing & DHCP Services: Configures router R1 to act as both the default gateway (Router-on-a-Stick sub-interfaces) for VLAN 1 and VLAN 2 and the central DHCP server distributing dynamic IP configurations to all floor workstations.   🛡️ Campus Security & Perimeter Mitigation: Outlines the positioning of access ports and mitigation controls against unauthorized rogue devices (such as Laptop-PT Hacker) attempting to bridge traffic across floors.   🛠️ Essential Cisco CLI Configuration SnippetsConfiguring RSTP Mode & Root Bridge Priority on Distribution Switch (Dist-1)Cisco CLIspanning-tree mode rapid-pvst
spanning-tree vlan 1,2 root primary
Configuring Trunking & PortFast on Access Edge Switches (Acc-1)Cisco CLIinterface GigabitEthernet0/1
 description Trunk Link to Distribution Dist-1
 switchport mode trunk
!
interface FastEthernet0/1
 description Access Port for PC0
 switchport mode access
 switchport access vlan 1
 spanning-tree portfast
 spanning-tree bpduguard enable
🧪 Verification & Audit CommandsBash# Verify Spanning Tree operational mode, root bridge bridge ID, and local root port status
show spanning-tree
show spanning-tree summary

# Inspect VLAN trunking encapsulation, active state, and allowed VLANs on trunk interfaces
show interfaces trunk

# Verify MAC address table learning and port-to-VLAN forwarding mappings
show mac address-table
