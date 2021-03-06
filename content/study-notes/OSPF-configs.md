---
title: "OSPF configs"
date: 2021-08-21T03:18:45-04:00
draft: false
author: "Daniel Arapi"
language: "English"
tags: ["study-notes", "ospf"]
---

! - Reset OSPF process
R1(config)#do clear ip ospf process

!~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

! - Configuring OSPF process
R1(config)#router ospf [PID]
! - Clear OSPF process for Router-ID to take effect+
R1(config-router)#router-id [x.x.x.x]
! - Select router interface to participate in OSPF process
R1(config-router)#network [ip_address] [wildcard] area [area_id]
R1(config-router)#network [ip_address] [0.0.0.0] area [area_id]
! - Advertise the default route to other OSPF routers as Type 5 LSA, configued on ASBR
! - The 'always' keyword will advertise even if route is not currently in the routing table
R1(config-router)#default-information originate always
R1(config-router)#exit
R1(config)#do show ip protocols

!~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

! - Passive Interface
R1(config)#router ospf [PID]
! - Make all ports passive interface
R1(config-router)#passive-interface default
! - Remove port from passive-interface state
R1(config-router)#no passive-interface FastEthernet 0/0
! - Make interface a passive interface
R1(config-router)#passive-interface FastEthernet 0/1
R1(config-router)#exit
! - Verify passive interfaces
R1(config)#do show ip protocols

!~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

! - Manipulate Reference-Bandwidth
R1(config)#router ospf [PID]
R1(config-router)#auto-cost reference-bandwidth [10000 Mbps]
R1(config-router)#exit
! - Verify reference bandwidth
R1(config)#do show ip ospf | include Reference

! - Manipulate Interface Cost
R1(config)#interface fastEthernet[#/#]
R1(config-if)#ip ospf cost [value]
R1(config-if)#exit
! - Verify interface cost
R1(config)#do show ip ospf interface FastEthernet 0/0 | include Cost

! - Manipulate Interface Bandwidth
R1(config)#interface fastEthernet[#/#]
! - This will not change the actual BW. Only used to manipulate L3 routing protocol metric calculations.
! - Look up "bandwidth inherit" command for configuring subinterface BW
R1(config-if)#bandwidth [Kbps]
R1(config-if)#exit
! Verify interface bandwidth
R1(config)#do show interfaces FastEthernet 0/0 | include BW

! - Hello / Dead Intervals
R1(config)#interface fastEthernet[#/#]
! - It is recommended that the dead interval should be 4 times the hello interval
! - Default is Hello (10 sec) and Dead (40 sec)
R1(config-if)#ip ospf hello-interval [seconds]
R1(config-if)#ip ospf dead-interval [seconds]
! - Number of hello packets to send within 1 second
(config-if)#ip ospf dead-interval minimal hello-multiplier [number-of-hello-packets]
R1(config-if)#exit
R1(config)#do show ip ospf interface FastEthernet[#/#] | include intervals

!~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

! - DR/BDR Election
! - Router with highest priority gets elected as DR.
! - Second highest priority is BDR.
R1(config)#interface fastEthernet[#/#]
R1(config-if)#ip ospf priority [#]
R1(config-if)#exit
R1(config)#do clear ip ospf process
R1(config)#do show ip ospf neighbor

!~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

! - Plain-Text Authentication on interface
R1(config)#interface fastEthernet[#/#]
R1(config-if)#ip ospf authentication
R1(config-if)#ip ospf authentication-key [password]
R1(config-if)#exit

! - Plain-Text authentication on entire area
R1(config)#router ospf [PID]
R1(config-router)#area [area_id] authentication

!~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

! - MD5 Authentication on interface, without key chain
R1(config)#interface fastEthernet[#/#]
R1(config-if)#ip ospf authentication message-digest
R1(config-if)#ip ospf message-digest-key [key_id] md5 [password]
R1(config-if)#exit

! - MD5 Authentication on interface, with key chain
R1(config)#key chain [chain-name]
! - Key-ID, algorithm, and password, must match on both ends of the link
R1(config-keychain)#key [key-id]
R1(config-keychain-key)#cryptographic-algorithm md5
R1(config-keychain-key)#key-string [password]
R1(config-keychain-key)#exit
R1(config-keychain)#exit
! - Enable authentication on interface
R1(config)#interface GigabitEthernet[#/#]
R1(config-if)#ip ospf authentication key-chain [chain-name]
R1(config-if)#exit

! - MD5 authentication on entire area
R1(config)#router ospf [PID]
R1(config-router)#area [area_id] authentication message-digest

!~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

! - HMAC-SHA Authentication on interface, with key chain
R1(config)#key chain [chain-name]
! - Key-ID, algorithm, and password, must match on both ends of the link
R1(config-keychain)#key [key-id]
R1(config-keychain-key)#cryptographic-algorithm hmac-sha-512
R1(config-keychain-key)#key-string [password]
R1(config-keychain-key)#exit
R1(config-keychain)#exit
! - Enable authentication on interface
R1(config)#interface GigabitEthernet[#/#]
R1(config-if)#ip ospf authentication key-chain [chain-name]
R1(config-if)#exit

!~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

! - TTL Security Check, globally
R1(config)#router ospf [PID]
R1(config-router)#ttl all-interfaces hops 0
R1(config-router)#exit

! - TTL Security Check, on interface
R1(config)#interface GigabitEthernet[#/#]
R1(config-if)#ip ospf ttl-security hops 0
R1(config-if)#exit

!~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

! - Redistribution
R1(config)#router ospf [PID]
! - Redistribute directly connected interfaces into OSPF
R1(config-router)#redistribute connected subnets
R1(config-router)#exit

!~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

! - Route Summarization
R1(config)#router ospf [PID]
! - Type 3 Interarea Route Summarization
! - Use subnet mask, not wildcard mask
R1(config-router)#area [AID] range [prefix] [mask]
! - Type 5 External Route Summarization
R1(config-router)#summary-address [prefix] [mask]
R1(config-router)#exit
R1(config)#do show ip ospf database

!~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

! - Filtering Routing Table
! - Filters route from routing table, but not from LSDB
! - Confgure ACL
R1(config)#ip access-list standard [acl_name]
! - Deny specific host or prefix
R1(config-std-nacl)#deny host [ip_address]
R1(config-std-nacl)#permit any
R1(config-std-nacl)#exit

! - Configure distribute list and combine with ACL
R1(config)#router ospf [PID]
! - Inbound distribute-list filtering is used to filter routes from appearing in routing table only (does not filter LSA)
R1(config-router)#distribute-list [acl_name] in

!~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

! - LSA Type 3 Filtering on ABR
! - Specify prefix to filter
R1(config)#ip prefix-list [prefix_list_name] deny [ip_address]/[cidr]
R1(config)#ip prefix-list [prefix_list_name] permit 0.0.0.0/0 le 32

R1(config)#router ospf [PID]
! - Filter specified prefix from any area from entering IN specified area
R1(config-router)#area [AID] filter-list prefix [prefix_list_name] in
! - Filter prefix from getting OUT of a specified area
R1(config-router)#area [AID] filter-list prefix [prefix_list_name] out
R1(config-router)#exit

! - LSA should NOT appear in LSDB
R1(config)#do show ip ospf [PID] | begin Area 3
! - Route to prefix should NOT appear in routing table
R1(config)#do show ip route [ip_address]
R1(config)#do show ip ospf database router [ip_address]

!~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

! - LSA Type 5 Filtering on ASBR
! - ACL + Distribute-List
R1(config)#ip access-list standard [acl_name]
R1(config-std-nacl)#deny host [ip_address]
R1(config-std-nacl)#permit any
R1(config-std-nacl)#exit
R1(config)#router ospf 1
R1(config-router)#distribute-list [acl_name] out
R1(config-router)#exit

! - ACL + Route-Map + Redistribution
R1(config)#ip access-list standard [acl_name]
R1(config-std-nacl)#permit host [ip_address]
R1(config)#route-map CONNECTED_TO_OSPF deny 10
R1(config-route-map)#match ip address [acl_name]
! - Permit everything else
R1(config)#route-map [route_map_name] permit 20
R1(config)#router ospf [AID]
R1(config-router)#redistribute connected subnets route-map [route_map_name]
R1(config-router)#exit

! - Summary Address No-Advertise
R1(config)#router ospf [AID]
R1(config-router)#summary-address [prefix] [mask] not-advertise

!~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

! - Non-Broadcast Network Type over Frame-Relay (Multi-Access)
Hub(config)#interface serial 0/0
Hub(config-if)#ip address [ip_address] [mask]
Hub(config-if)#encapsulation frame-relay
! - 'broadcast' command - router will emulate broadcasts by sending unicast packets to each destination
! - Default priority is 1, making this the DR
Hub(config-if)#ip ospf network broadcast
Hub(config-if)#exit
Hub(config)#router ospf [PID]
Hub(config-router)#network [ip_address] [wildcard] area [AID]
Hub(config-router)#exit

Spoke(config)#interface serial 0/0
Spoke(config-if)#ip address [ip_address] [mask]
Spoke(config-if)#encapsulation frame-relay
Spoke(config-if)#ip ospf network non-broadcast
! - Setting priority to 0 ensures that the spoke does not become a DR
Spoke(config-if)#ip ospf priority 0
Spoke(config-if)#exit
Spoke(config)#router ospf [PID]
Spoke(config-router)#network [ip_address] [wildcard] area [AID]
Spoke(config-router)#exit
Spoke(config)#do show ip ospf neighbor

!~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

! - Broadcast Network Type over Frame-Relay (Multi-Access)
Hub(config)#interface serial 0/0
Hub(config-if)#ip address [ip_address] [mask]
Hub(config-if)#encapsulation frame-relay
! - 'broadcast' command - router will emulate broadcasts by sending unicast packets to each destination
! - Default priority is 1, making this the DR
Hub(config-if)#ip ospf network broadcast
Hub(config-if)#exit
Hub(config)#router ospf [PID]
Hub(config-router)#network [ip_address] [wildcard] area [AID]
Hub(config-router)#exit

Spoke(config)#interface serial 0/0
Spoke(config-if)#ip address [ip_address] [mask]
Spoke(config-if)#encapsulation frame-relay
! - 'broadcast' command - router will emulate broadcasts by sending unicast packets to each destination
Spoke(config-if)#ip ospf network broadcast
! - Setting priority to 0 ensures that the spoke does not become a DR
Spoke(config-if)#ip ospf priority 0
Spoke(config-if)#exit
Spoke(config)#router ospf [PID]
Spoke(config-router)#network [ip_address] [wildcard] area [AID]
Spoke(config-router)#exit
Spoke(config)#do show ip ospf neighbor

!~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

! - Show commands
R1#show ip ospf neighbor
R1#show ip ospf neighbor detail
R1#show ip protocol
R1#show ip route
R1#debug ip ospf packet
R1#show ip ospf interface fastEthernet[#/#]
R1#show ip ospf interface GigabitEthernet 0/1 | begin auth

! - Show LSA types
R1#show ip ospf database

! - Show bandwidth of interface
R1#show interfaces FastEthernet 0/0 | include BW

! - Show OSPF cost of interface
show ip ospf interface FastEthernet 0/0 | include Cost

! - Show interface cost
R1#show ip ospf interface FastEthernet[#/#] | include Cost

! - Show interface bandwidth
R1#show interfaces FastEthernet[#/#] | include BW

! - Show interface hello/dead intervals
R1#show ip ospf interface FastEthernet[#/#] | include intervals

! - Show reference bandwidth
R1#show ip ospf | include Reference

! - See neighborship discovery and adjacency in real-time
R1#debug ip opsf adj

! - Verify passive interfaces
R1#show ip protocols

! - Verify adjacency and DR/BDR election
R1#show ip ospf neighbor

! - Verify Router-ID
R1#show ip protocols | include Router ID

! - Verify LSA Type 3 Filtering
R1#show ip ospf [PID] | begin Area [AID]

################################################################################
################################# Copy/ Paste ##################################
################################################################################
