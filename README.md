# Configuring OSPF with Ansible

Ansible playbook that configures OSPF on six Cisco IOL routers across three areas, starting from a blank lab. It brings up the interfaces, configures the IP addresses, enables OSPF on each interface, and configures the OSPF process, including the router ID and passive interfaces.

This repo accompanies the [Configuring OSPF with Ansible](https://www.packetswitch.co.uk/ansible/) post, part of the Ansible for Network Engineers series on [packetswitch.co.uk](https://www.packetswitch.co.uk).

## Lab

- Six Cisco IOL routers (`r1` to `r6`)
- Area 0 - `r1`, `r2`, `r3`, `r4`
- Area 1 - `r1`, `r3`, `r5`
- Area 2 - `r2`, `r4`, `r6`
- Built with Containerlab, using the topology file in this repo

```bash
sudo containerlab deploy -t ospf_lab.clab.yml
```

## Requirements

- Python 3.12
- Ansible
- paramiko

Using `uv`:

```bash
uv sync
source .venv/bin/activate
```

Or using pip:

```bash
python3 -m venv venv
source venv/bin/activate
pip install ansible paramiko
```

## Project Structure

```
├── ansible.cfg
├── inventory.yml
├── group_vars/
│   └── routers.yml      # Connection settings
├── host_vars/
│   └── r1.yml ... r6.yml # Interfaces, IPs, areas, router ID
└── ospf.yml             # Playbook
```

## Usage

Run in check mode first to see what will change:

```bash
ansible-playbook ospf.yml --check -v
```

Then apply the config:

```bash
ansible-playbook ospf.yml
```

Verify on any router:

```
show ip ospf neighbor
show ip route ospf
```

## Note

The credentials in `group_vars/routers.yml` are in plain text for lab use only. Use Ansible Vault in production.