molecule_openstack
=========

The role provides a create and delete playbook for creating OpenStack instances in the context of molecule testing. 
The role is designed to be used in the `molecule.yml` file as part of the `provisioner` section.

Requirements
------------

Openstack and Molecule

Role Variables
--------------

molecule.yml (Example)
```molecule.yml
---
(...)
platforms:
    - name: instance-name
      image_name: Ubuntu 20.04 64-bit
(...)
provisioner:
    name: ansible
    playbooks:
        create: <path-to-custom-create-playbook>
        destroy: <path-to-custom-delte-playbook>
(...)
```

Dependencies
------------

No Dependencies

Example Playbook
----------------

requirements.yml
````yaml
---
roles:
  - name: ipa-big.molecule_openstack
    version: "1.0.0"
````

  create.yml
````yaml
---
- name: Create
  hosts: localhost
  connection: local
  gather_facts: false
  no_log: "{{ molecule_no_log }}"
  tasks:
    - name: Create via openstack
      ansible.builtin.include_role:
        name: molecule_openstack
        tasks_from: create.yml
````

destroy.yml
````yaml
---
- name: Destroy
  hosts: localhost
  gather_facts: false
  no_log: "{{ molecule_no_log }}"
  become: no
  tasks:
    # Developer must implement.
    - include_role:
        name: molecule_vsphere
        tasks_from: destroy.yml

    # Mandatory configuration for Molecule to function.
    - name: Populate instance config
      set_fact:
        instance_conf: {}

    - name: Dump instance config
      copy:
        content: |
          # Molecule managed

          {{ instance_conf | to_json | from_json | to_yaml }}
        dest: "{{ molecule_instance_config }}"
        mode: 0600
      delegate_to: localhost
      when: server.changed | default(false) | bool

````

License
-------

BSD

Author Information
------------------

Benjamin Goetz

Fraunhofer IPA
