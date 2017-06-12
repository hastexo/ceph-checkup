# `ceph-checkup` 

## Anible playbooks collecting vital information about Ceph nodes

The playbooks in this repository collect information about a Ceph
cluster in a non-destructive, read-only fashion.

These playbooks require at least Ansible version 2.2.

### Inventory

To use these playbooks, you must first create an Ansible
[inventory](http://docs.ansible.com/ansible/intro_inventory.html) file
(conventionally named `hosts`), optionally using the provided
`hosts.example` file as a starting point.

The inventory file must define two groups, `mon` containing your Mon
nodes, and `osd` containing your OSD nodes. For example, your
inventory file might look like this:

```ini
[mon]
# Mon hosts
ceph-node1
ceph-node2
ceph-node3

[osd]
# OSD hosts
ceph-node4
ceph-node5
ceph-node6
```

You must refer to your nodes by whatever name you use to connect to
them via SSH, including aliases defined with a `Host <name>` entry.

If you need to connect to your Ceph nodes via a jump host, use
[the `ProxyCommand ssh -W` method](https://en.wikibooks.org/wiki/OpenSSH/Cookbook/Proxies_and_Jump_Hosts#Recursively_Chaining_Gateways_Using_stdio_Forwarding).


### Running the playbooks

In order to run the playbook, execute the following command:

```bash
ansible-playbook -i hosts checkup.yml
```

