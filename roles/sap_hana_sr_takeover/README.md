<!-- BEGIN Title -->
# sap_hana_sr_takeover
<!-- END Title -->

## Description
<!-- BEGIN Description -->
This role can be used to ensure, control and change SAP HANA System Replication.
<!-- END Description -->

## Dependencies
<!-- BEGIN Dependencies -->
`sap_hana_sid` and `sap_hana_instance_number` may already be set or used in other roles.
<!-- END Dependencies -->

<!-- BEGIN Prerequisites -->
## Prerequisites
This role requires a SAP HANA System Replication configuration, e.g. using the community.sap_install.sap_ha_install_hana_hsr role.
<!-- END Prerequisites -->

## Execution
<!-- BEGIN Execution -->
<!-- END Execution -->
### Example
<!-- BEGIN Execution Example -->
You have `hana1` and `hana2` configured for SAP HANA system replication (HSR) with HANA SID `RHE` and HANA instance number `00`.
The following playbook ensures that `hana1` is the primary system and `hana2` is the secondary system.

The role fails if `hana1` is not configured for system replication (mode: none in `hdbnsutil -sr_state`).
`hana1` needs to be either the primary system, or the secondary system in sync state.

The role does not change the configuration if `hana1` is already primary system, and `hana2` is secondary system.
The role ensures that the secondary system is started

```yaml
- name: Ensure hana1 is primary
  hosts: hanas
  become: true
  tasks:
    - name: Switch to hana1
      ansible.builtin.include_role:
        name: community.sap_operations.sap_hana_sr_takeover
      vars:
        sap_hana_sr_takeover_primary: hana2
        sap_hana_sr_takeover_secondary: hana1
        sap_hana_sr_takeover_sitename: "DC01"
        sap_hana_sid: "RHE"
        sap_hana_instance_number: "00"
```

<!-- END Execution Example -->
<!-- BEGIN Role Tags -->
<!-- END Role Tags -->
<!-- BEGIN Further Information -->
<!-- END Further Information -->

## License
<!-- BEGIN License -->
Apache 2.0
<!-- END License -->

## Maintainers
<!-- BEGIN Maintainers -->
- SAP LinuxLab
<!-- END Maintainers -->

## Role Variables
<!-- BEGIN Role Variables -->
### sap_hana_sr_takeover_primary
- **Required**
- _Type:_ `string`

Server to become the primary server.

### sap_hana_sr_takeover_secondary
- **Required**
- _Type:_ `string`

Server to register as secondary. The role can be run twice if more than one secondary is needed by looping over this variable.

### sap_hana_sr_takeover_sitename
- **Required**
- _Type:_ `string`

Name of the site being registered as secondary.

### sap_hana_sr_takeover_rep_mode
- **Required**
- _Type:_ `string`
- _Default:_ `sync`

Select SAP HANA system replication mode. Options: `sync`, `syncmem`, `async`.

### sap_hana_sr_takeover_hsr_oper_mode
- **Required**
- _Type:_ `string`
- _Default:_ `logreplay`

Select SAP HANA system replication operation mode. Options: `delta_datashipping`, `logreplay`, `logreplay_readaccess`.

### sap_hana_sr_takeover_handshake
- **Optional**
- _Type:_ `boolean`
- _Default:_ `false`

Run takeover with handshake for a planned system maintenance scenario.

### sap_hana_sid
- **Required**
- _Type:_ `string`

Select SAP HANA SID.

### sap_hana_instance_number
- **Required**
- _Type:_ `string`

Select SAP HANA instance number
