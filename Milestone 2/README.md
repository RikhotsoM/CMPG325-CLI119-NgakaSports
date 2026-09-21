# Milestone 2 — Network Implementation

**Topology:** Extended Star / Cascaded Star (2-Tier Hierarchical)
**Routing Method:** Router-on-a-Stick (802.1Q trunking)
**VLANs:** 10 (Admin), 20 (Finance), 30 (Sports), 40 (Guest), 50 (Printer)
**Gateway Range:** 10.46.10.1 – 10.46.50.1 /24

All VLANs configured, trunks active, inter-VLAN routing working.
Testing: All gateways reachable; minor initial packet loss = normal convergence.
Files: Packet Tracer project + documentation + full test evidence.

The network uses an Extended Star / Cascaded Star, represented as a 2-Tier Hierarchical design. The Core Router connects to Switch-1, which connects to Switch-2. End devices are distributed across the access switches.

**Trunking**
802.1Q (dot1Q) trunking is used to carry multiple VLANs across the relevant links. The native VLAN is VLAN 1, and the required VLANs are explicitly allowed on the trunk links.

**Inter-VLAN Routing**
The Core Router's GigabitEthernet interface is configured with five subinterfaces. Each subinterface uses an 802.1Q VLAN tag corresponding to the VLAN ID and provides the default gateway for that VLAN.

**Port Assignments**
End-device ports are configured as access ports in their respective VLANs. The inter-switch link and switch-to-router link are configured as trunk links where required.

**Cabling**
•	Switch-to-Router: Straight-Through cable.
•	PC-to-Switch: Straight-Through cable.
•	Switch-to-Switch: Crossover cable.

