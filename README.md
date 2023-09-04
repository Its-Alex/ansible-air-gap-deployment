# Ansible airgap deployment

This repo aim to reproduce and test a airgap deployment with ansible.

## Requirements

- [vagrant](https://www.vagrantup.com/)
- [virtualbox](https://www.virtualbox.org/)

## Getting started

First you must launch VMs:

```bash
$ vagrant up
Bringing machine 'admin' up with 'virtualbox' provider...
Bringing machine 'server001' up with 'virtualbox' provider...
==> admin: Importing base box 'bento/ubuntu-22.04'...
==> admin: Matching MAC address for NAT networking...
==> admin: Checking if box 'bento/ubuntu-22.04' version '202303.13.0' is up to date...
==> admin: Setting the name of the VM: admin
==> admin: Clearing any previously set network interfaces...
==> admin: Preparing network interfaces based on configuration...
    admin: Adapter 1: nat
    admin: Adapter 2: hostonly
==> admin: Forwarding ports...
    admin: 22 (guest) => 2222 (host) (adapter 1)
==> admin: Running 'pre-boot' VM customizations...
==> admin: Booting VM...
==> admin: Waiting for machine to boot. This may take a few minutes...
    admin: SSH address: 127.0.0.1:2222
    admin: SSH username: vagrant
    admin: SSH auth method: private key
    admin: Warning: Connection reset. Retrying...
    admin: Warning: Remote connection disconnect. Retrying...
    admin:
    admin: Vagrant insecure key detected. Vagrant will automatically replace
    admin: this with a newly generated keypair for better security.
    admin:
    admin: Inserting generated public key within guest...
    admin: Removing insecure key from the guest if it's present...
    admin: Key inserted! Disconnecting and reconnecting using new SSH key...
==> admin: Machine booted and ready!
...
```

When you're machines are up and ready you can ssh inside it:

```bash
$ vagrant ssh admin
vagrant ssh admin
Welcome to Ubuntu 22.04.2 LTS (GNU/Linux 5.15.0-67-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Mon Sep  4 11:55:26 AM UTC 2023

  System load:  0.0185546875       Processes:             142
  Usage of /:   15.1% of 30.34GB   Users logged in:       0
  Memory usage: 18%                IPv4 address for eth0: 10.0.2.15
  Swap usage:   0%                 IPv4 address for eth1: 192.168.56.10


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Mon Sep  4 11:51:26 2023 from 10.0.2.2
```

Then launch an ansible command inside `/vagrant/ansible`:

```bash
$ sudo su
# cd /vagrant/ansible
# nsible -m ping all
server001 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
# ansible-playbook playbooks/default.yaml
PLAY [all] ******************************************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************************
Monday 04 September 2023  11:56:48 +0000 (0:00:00.006)       0:00:00.006 ******
ok: [server001]

TASK [Distribution] *********************************************************************************************************************
Monday 04 September 2023  11:56:49 +0000 (0:00:01.025)       0:00:01.032 ******
ok: [server001] => {
    "msg": "Ubuntu"
}

TASK [Distribution version] *************************************************************************************************************
Monday 04 September 2023  11:56:49 +0000 (0:00:00.016)       0:00:01.048 ******
ok: [server001] => {
    "msg": "22.04"
}

TASK [Distribution major version] *******************************************************************************************************
Monday 04 September 2023  11:56:49 +0000 (0:00:00.016)       0:00:01.065 ******
ok: [server001] => {
    "msg": "22"
}

PLAY RECAP ******************************************************************************************************************************
server001                  : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

Monday 04 September 2023  11:56:49 +0000 (0:00:00.026)       0:00:01.091 ******
===============================================================================
Gathering Facts ------------------------------------------------------------------------------------------------------------------ 1.03s
Distribution major version ------------------------------------------------------------------------------------------------------- 0.03s
Distribution --------------------------------------------------------------------------------------------------------------------- 0.02s
Distribution version ------------------------------------------------------------------------------------------------------------- 0.02s
```

# License

[MIT](/LICENSE)
