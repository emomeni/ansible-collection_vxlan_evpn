🏭 jiholland.overlay
====================

Configure overlay for multisite VXLAN-EVPN fabric on Cisco Nexus platform.

Requirements
------------

💿 [Cisco NXOS Collection](https://galaxy.ansible.com/cisco/nxos)

Role Variables
--------------

defaults/main.yml:
- overlay_rid_if
- overlay_rid_ip
- overlay_vtep_if
- overlay_bgp_asn
- overlay_bgp_neighbors
- overlay_rmap_host_svi_name
- overlay_rmap_host_svi_tag
- overlay_anycast_gw_mac
- overlay_global_mcast_group_l2

Example - hostvars/SPINE-1.yml:
```YAML
---
# Hostvars for spine

network_role: spine

rid_if: loopback0
rid_ip: 10.250.250.30

bgp_asn: 65001
bgp_neighbors:
  - name: LEAF-1
    ip: 10.250.250.32
  - name: LEAF-2
    ip: 10.250.250.33
```
Example - hostvars/LEAF-1.yml:
```YAML
---
# Hostvars for leaf

network_role: leaf

rid_if: loopback0
rid_ip: 10.250.250.30

vtep_if: loopback1

bgp_asn: 65001
bgp_neighbors:
  - name: SPINE-1
    ip: 10.250.250.30
  - name: SPINE-2
    ip: 10.250.250.31
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
    - role: jiholland.vxlan_evpn.verify
```
License
-------

BSD

Author Information
------------------

Jørn Ivar Holland
