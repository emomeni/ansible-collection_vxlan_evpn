🏭 jiholland.underlay
=====================

Configure underlay for multisite VXLAN-EVPN fabric on Cisco Nexus platform.
- OSPF routing.
- P2P L3 links.
- PIM Sparse with Anycast RP.
```YAML
                       OSPF AREA 0.0.0.0
       + - - - - - - - - - - - - - - - - - - - - - - -
       |         PIM Anycast RP (Loopback254)         |
       + - - - - - - - - - - - - - - - - - - - - - - -
       +--------------------+    +--------------------+
       |      SPINE-1       |    |      SPINE-2       |
       | RID: 10.250.250.30 |    | RID: 10.250.250.31 |
       +--------------------+    +--------------------+
                | |                       | |
           +-+--+-+-----------------------+ |
           | |  +---------Loopback0---------+-----+-+
           | |                                    | |
+--------------------+                            | |
|       LEAF-1       |                 +------------+-------+
| RID: 10.250.250.32 |                 |       LEAF-2       |
+--------------------+                 | RID: 10.250.250.33 |
                                       +--------------------+
```
Requirements
------------

💿 [Cisco NXOS Collection](https://galaxy.ansible.com/cisco/nxos)

Role Variables
--------------

Example - hostvars/SPINE-1.yml:
```YAML
---
# Hostvars for spine

network_role: spine

rid_if: loopback0
rid_ip: 10.250.250.30
rid_mask: 32

pim_desc: PIM ANYCAST RP
pim_if: loopback254
pim_ip: 10.254.254.30  # Anycast RP address.
pim_mask: 32
pim_local_ip: 10.254.254.1
pim_remote_ip: 10.254.254.2

fabric_interfaces:
 - name: Ethernet1/1
 - name: Ethernet1/2
 - name: Ethernet1/3
 - name: Ethernet1/4
```
Example - hostvars/LEAF-1.yml:
```YAML
---
# Hostvars for leaf

network_role: leaf

rid_if: loopback0
rid_ip: 10.250.250.32
rid_mask: 32
rid_tag: 54321

vtep_if: loopback1
vtep_ip: 10.254.250.32
vtep_mask: 32
vtep_tag: 54321

fabric_interfaces:
 - name: Ethernet1/51
 - name: Ethernet1/52
```
Example Playbook
----------------
```YAML
---
- name: Build VXLAN-EVPN fabric.
  hosts: "{{ target }}"
  gather_facts: false

  roles:
    - role: jiholland.vxlan_evpn.vpc
      when: vpc_domain is defined
    - role: jiholland.vxlan_evpn.underlay
    - role: jiholland.vxlan_evpn.overlay
    - role: jiholland.vxlan_evpn.dci
    - role: jiholland.vxlan_evpn.host_segments
    - role: jiholland.vxlan_evpn.verify
```
License
-------

BSD

Author Information
------------------

Jørn Ivar Holland
