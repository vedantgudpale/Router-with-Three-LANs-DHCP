# Router With Three LANs DHCP

This Packet Tracer project demonstrates a basic routed network with DHCP (Dynamic Host Configuration Protocol) server functionality. The topology consists of a router acting as a DHCP server for three separate LAN segments, each connected to a dedicated switch.

## Topology Diagram
                                    R1 (ISR 4331 Router)
                        +------------------------------------------+
                        | GigabitEthernet0/0/0     GigabitEthernet0/0/1     GigabitEthernet0/0/2 |
                        |  IP: 192.168.1.1/24        IP: 192.168.2.1/24        IP: 192.168.3.1/24   |
                        +------------------------------------------+
                                     |                              |                              |
                                     |                              |                              |
                   +-----------------+-----------------+    +-----------------+-----------------+    +-----------------+-----------------+
                   |                 |                 |    |                 |                 |    |                 |                 |
          Switch 1 (3650 Switch)     Switch 2 (3650 Switch)     Switch 3 (3650 Switch)
        +---------------------+      +---------------------+      +---------------------+
        | VLAN 1 (Default)    |      | VLAN 1 (Default)    |      | VLAN 1 (Default)    |
        | Network: 192.168.1.0/24 |      | Network: 192.168.2.0/24 |      | Network: 192.168.3.0/24 |
        +---------------------+      +---------------------+      +---------------------+
            |       |       |            |       |       |            |       |       |
            PC     PC     PC             PC      PC      PC           PC      PC     PC
    ...     ...     ...        ...     ...     ...        ...     ...     ...
    
    (Network 1)      (Network 1)      (Network 1)      (Network 2)      (Network 2)      (Network 2)      (Network 3)      (Network 3)      (Network 3)

## Project Overview

The purpose of this project is to:

*   Set up a basic network infrastructure using a router and switches.
*   Configure a router as a DHCP server to automatically assign IP addresses to devices.
*   Create three separate IP subnets (LANs): `192.168.1.0/24`, `192.168.2.0/24`, and `192.168.3.0/24`.
*   Demonstrate routing between these subnets using the router.
*   Implement basic security configurations on the switches.

## Components Used

*   **1 Router:** Cisco ISR 4331 Router
*   **3 Switches:** Cisco Catalyst 3650 Switches
*   **21 PCs:**  Connected to the switches (approximately [Number] per switch)
*   **Copper Straight-Through Cables:** For all connections

## IP Addressing Scheme

*   **Network 1 (Switch 1):** `192.168.1.0/24`
    *   Router Interface `GigabitEthernet0/0/0`: `192.168.1.1/24` (Default Gateway for Network 1)
    *   DHCP Pool: `LAN-POOL-1` (Range: `192.168.1.0/24`, Gateway: `192.168.1.1`)
*   **Network 2 (Switch 2):** `192.168.2.0/24`
    *   Router Interface `GigabitEthernet0/0/1`: `192.168.2.1/24` (Default Gateway for Network 2)
    *   DHCP Pool: `LAN-POOL-2` (Range: `192.168.2.0/24`, Gateway: `192.168.2.1`)
*   **Network 3 (Switch 3):** `192.168.3.0/24`
    *   Router Interface `GigabitEthernet0/0/2`: `192.168.3.1/24` (Default Gateway for Network 3)
    *   DHCP Pool: `LAN-POOL-3` (Range: `192.168.3.0/24`, Gateway: `192.168.3.1`)

## Router R1 Configuration Commands

```cli
enable
configure terminal

! --- Interface GigabitEthernet0/0/0 Configuration (Network 1 - 192.168.1.0/24) ---
interface GigabitEthernet0/0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

! --- Interface GigabitEthernet0/0/1 Configuration (Network 2 - 192.168.2.0/24) ---
interface GigabitEthernet0/0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
exit

! --- Interface GigabitEthernet0/0/2 Configuration (Network 3 - 192.168.3.0/24) ---
interface GigabitEthernet0/0/2
ip address 192.168.3.1 255.255.255.0
no shutdown
exit

! --- DHCP Pool Configuration for Network 1 (192.168.1.0/24) ---
ip dhcp pool LAN-POOL-1
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
dns-server 8.8.8.8
exclude-address 192.168.1.1
exit

! --- DHCP Pool Configuration for Network 2 (192.168.2.0/24) ---
ip dhcp pool LAN-POOL-2
network 192.168.2.0 255.255.255.0
default-router 192.168.2.1
dns-server 8.8.8.8
exclude-address 192.168.2.1
exit

! --- DHCP Pool Configuration for Network 3 (192.168.3.0/24) ---
ip dhcp pool LAN-POOL-3
network 192.168.3.0 255.255.255.0
default-router 192.168.3.1
dns-server 8.8.8.8
exclude-address 192.168.3.1
exit

exit
copy running-config startup-config




Switch Configuration Commands

Switch>enable
Switch#configure terminal
Switch(config)#hostname S1
S1(config)#enable secret class
S1(config)#line console 0
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#line vty 0 15
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#service password-encryption
S1(config)#no ip domain-lookup
S1(config)#logging synchronous
S1(config)#exit
S1#copy running-config startup-config
# Router-with-Three-LANs-DHCP
