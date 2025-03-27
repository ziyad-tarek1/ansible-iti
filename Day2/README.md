# Ansible Playbook: Configure MOTD on Database Nodes

## Overview
This Ansible playbook updates the **Message of the Day (MOTD)** on database nodes. It retrieves the message from the corresponding **host_vars** or **group_vars** and writes it to the `/etc/motd` file on each target host.

## Directory Structure
```
.
├── group_vars
│   └── database.yaml  # Group variable file for all database nodes
├── host_vars
│   └── db1.yaml       # Host-specific variable file for db1
├── inventory.ini      # Inventory file defining target hosts
└── playbook.yml       # Ansible playbook
```

## Inventory File (inventory.ini)
```ini
[database]
db1 ansible_host=3.239.227.244  
db2 ansible_host=3.239.174.117
```

## Variables Files
### `group_vars/database.yaml`
```yaml
---
message: "This is MOTD From The Slave DB Node"
```

### `host_vars/db1.yaml`
```yaml
---
message: "This is MOTD From The Master DB Node"
```

## Playbook (playbook.yml)
```yaml
---
- name: Add message to /etc/motd
  hosts: all
  become: yes
  tasks:
    - name: Add message in /etc/motd
      ansible.builtin.lineinfile:
        path: /etc/motd
        line: "{{ message }}"

    - name: Show the message variable
      ansible.builtin.debug:
        msg: "The message is: {{ message }}"
```
![image](https://github.com/user-attachments/assets/d5631a96-bf45-483f-8c94-0c3c16c62094)

## Running the Playbook
To execute the playbook, run:
```sh
ansible-playbook -i inventory.ini playbook.yml
```
![image](https://github.com/user-attachments/assets/1fc0a8aa-10fb-4a1a-bffd-53959f23360d)

## Expected Output
- The `/etc/motd` file on each database node will contain the corresponding message from the **host_vars** or **group_vars**.
- The debug task will print the message being set for each node.

### OutPutS:
- DB1 **Master**:

![image](https://github.com/user-attachments/assets/b01bdfd5-8be8-4ed5-b31f-d164bdff9240)

- DB2 **Slave**:

![image](https://github.com/user-attachments/assets/15305dc0-3420-4a71-bd48-617bb75b81cd)


## Notes
- Ensure that Ansible is installed and properly configured.
- The target nodes must have SSH access enabled for Ansible.
- Use the correct `ansible_user` in `inventory.ini` if required.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## Authors

- **Ziyad Tarek**
  - GitHub: [Ziyad Tarek](https://github.com/ziyad-tarek1)
  - Email: ziyadtarek180@gmail.com

