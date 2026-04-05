# Network Automation – NX-OS EVPN/VXLAN Fabric (Ansible + NetBox)

## Overview

This project automates the generation of Cisco NX-OS configurations for a spine–leaf (Clos) fabric using:

* **Ansible** for orchestration
* **Jinja2** for templating
* **NetBox** as the source of truth

The goal is to move away from static configurations and toward **data-driven network automation**, where device configs are rendered dynamically from structured data.

---

## Key Features

* 🔧 **Automated config generation**

  * Per-device configs generated from NetBox inventory
  * Outputs saved under `generated_configs/`

* **Source of Truth (NetBox)**

  * Device roles (spine / tor)
  * Interface data
  * BGP ASN and OSPF parameters via `local_context_data`

* **Modular Jinja2 Templates**

  * `interfaces.j2`
  * `bgp.j2`
  * `ospf.j2`
  * `vlan.j2`
  * `features.j2`
  * `usernames.j2`

* **Fabric Architecture Support**

  * Spine / Leaf role awareness
  * Underlay (OSPF)
  * Overlay (BGP EVPN)
  * VXLAN / NVE configuration

* **Idempotent Workflow**

  * Removes old configs before generating new ones
  * Ensures clean output per run

---

## Project Structure

```
.
├── ansible.cfg
├── playbook.yml
├── inventories/
│   └── netbox_inv.yml
├── templates/
│   └── nxos/
│       ├── nxos.j2
│       ├── interfaces.j2
│       ├── bgp.j2
│       ├── ospf.j2
│       ├── vlan.j2
│       ├── features.j2
│       └── usernames.j2
├── generated_configs/
└── README.md
```

---

## How It Works

1. **NetBox Inventory**

   * Ansible pulls device + interface data using the NetBox inventory plugin

2. **Jinja2 Rendering**

   * Templates dynamically build configs based on:

     * device role (spine vs tor)
     * interface names and tags
     * local context (BGP ASN, OSPF router ID, etc.)

3. **Config Output**

   * Configs are generated per device:

     ```
     generated_configs/R0-SPINE01.txt
     generated_configs/R0-TOR01.txt
     ```

---

## Example Workflow

Run the playbook:

```bash
ansible-playbook -i inventories/netbox_inv.yml playbook.yml
```

Output:

```bash
generated_configs/
├── R0-SPINE01.txt
├── R0-SPINE02.txt
├── R0-TOR01.txt
└── R0-TOR02.txt
```

---

## Example: Role-Based Logic

```jinja2
{% if device_roles == "spine" %}
template peer VXLAN_LEAF
{% elif device_roles == "tor" %}
template peer VXLAN_SPINE
{% endif %}
```

---

## Technologies Used

* Ansible
* Jinja2
* NetBox
* Cisco NX-OS
* EVPN / VXLAN
* Python (indirect via Ansible ecosystem)

---

## Current Limitations

This project is intentionally scoped as a **lab / learning environment**, and includes:

* Some **hardcoded fabric values** (VLANs, VNIs, multicast groups)
* Limited **data validation**
* Basic **role-based logic**
* No secrets management (for lab simplicity)

---

## Future Improvements

* Move all fabric data (VLANs, VNIs, RP, etc.) into NetBox
* Add input validation and error handling
* Implement secrets management (Vault / environment variables)
* Add config diff + validation stage
* Expand role-based logic (access, trunk, vPC, routed ports)
* CI/CD pipeline integration (GitLab / GitHub Actions)

---

Recommended:

* Use environment variables or Ansible Vault for secrets

---

## Author

**Leonard Blottin III**
Network Automation Engineer

* CISSP | CCNA

---

## Why This Project

This project demonstrates:

* Real-world **network automation patterns**
* Integration with a **source of truth**
* Transition from CLI-based configs → **intent-driven infrastructure**
