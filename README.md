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

The playbooks use
[Ansible’s `become` method](http://docs.ansible.com/ansible/become.html)
for privilege escalation. This means that you need _not_ be able to
ssh into your Ceph nodes directly as `root`, but the user you connect
as must be able the _become_ `root` once logged in, via `sudo`. If the
user you connect as needs to provide a password in order to use
`sudo`, add the `-K` option to be prompted for that password:

```bash
ansible-playbook -i hosts -K checkup.yml
```

## Variables

You may want to override the following variables:

* `ceph_config_directory` — if you keep your configuration in a
  directory other than `/etc/ceph`
* `ceph_log_directory` — if your Ceph daemons write their logs to a
  directory different from `/var/log/ceph`
* `bond_interfaces` — if you want the playbooks to check the bonding
  state of interfaces other than `bond0` and `bond1`.

## Supported platforms

The playbook tries to be as platform-agnostic as possible, but is
being tested regularly on Ubuntu 16.04.

## License

   Copyright 2016-17 hastexo Professional Services GmbH

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
