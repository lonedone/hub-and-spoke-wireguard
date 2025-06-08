# WireGuard Hub-and-Spoke VPN Configuration

## Overview

This setup configures a WireGuard VPN using a hub-and-spoke topology.

WireGuard is chosen for its simplicity, performance, and native support on many platforms.

This configuration is based on [this answer](https://web.archive.org/web/20250321011925/https://superuser.com/questions/1801791/route-all-internet-traffic-through-wireguard-peer/1802992#1802992) and uses githubixx.ansible_role_wireguard ansible role.

## How to use

### 1. Install the depenencies

Once you have ansible installed, run this command

`ansible-galaxy role install -r requirements.yml`

### 2. Generate keys and put the values into the variables

Check "Key Generation" section of the following page to generate sets of public and private keys
https://www.wireguard.com/quickstart/

Example command: `wg genkey | tee privatekey | wg pubkey > publickey`

Note: There are different ways to store the keys, but it is better to have a copy of them locally in case of disaster recovery. You may use ansible vault to encrypt them.

The example secret configuration is in the `groups_vars/all/2._secrets_config_example.yml` file.

You need to fill all the private and public keys (note that hub and internet gateway are not specified, they are generated in the role out of the provided private keys)

You may add more or remove clients as you need, just remember to change the network ip address number as it should be unique.
```
  address_number: 10
```

### 3. Initialize the servers

It is tested on the debian/ubuntu mashines

Create non-root user and the correspoding inventory.yml (There is a backbone **inventory_example.yml** inventory for reference)

### 4. Run the playbooks

`ansible-playbook -i inventory.yml wireguard.yml`

### 5. Clients configs

In order to connect to the wireguard you need the client configurations.

They are created in the `./client_configs` directory after the playbook is run.

{name}.yml - are the configs to connect to the hub

{name}_direct.yml - are the configs to connect directly to the internet gateway

## Firewall

You may have to setup Firewall, there is an UFW-based playbook that you may use if your connections are getting blocked on the servers

`ansible-playbook -i inventory.yml firewall.yml`

## Feat

Iran, Russia, Turkey
