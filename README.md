# Trying OpenNebula

This repository is made for a lacture of Nuvem on UnB.
The goal is to install a Open Nebula cloud tool in a 
environment for testing cloud computing system.
The steps of the project are:

  1. Install a virtualization tool;
  2. Install a cloud tool;
  3. Install a service on cloud;

## Authors

```
  1. Bryan Fernandes
  2. Paulo Tada
```

## Requirements

### Nested Virtualization

The host machine must be enable for nested KVM virtualization. This
allows the host to create a VM inside another VM witch is proper for
this project.

Check if the nested KVM Kernel parameter is enable:
```
  $ cat /sys/module/kvm_intel/parameters/nested
```

If returns 'Y' then your ready to go. If returns 'N' then temporarily
removethe KVM intel Kernel module, enable nested virtualization to be
persistent across reboots and add the Kernel module back:
```
  $ rmmod kvm-intel
  $ sh -c "echo 'options kvm-intel nested=y' >> /etc/modprobe.d/dist.conf"
  $ modprobe kvm-intel
```

Then check again if the nested KVM Kernel parameter is enable:
```
  $ cat /sys/module/kvm_intel/parameters/nested
```

## Installation Guide

The chosen tools for this project:

  1. VirtualBox for virtualization environment;
  2. OpenNebula for cloud computing management;

## VirtualBox

## OpenNebula

The OpenNebula architecture requires a 'frontend' server use
for monitoring virtualization, storage images and network management,
and 'nodes' which has all the virtual machines on the cloud.

The 'frontend' machines needs the dependecies:

```
$ yum install opennebula-server \
opennebula-sunstone \
opennebula-node-kvm \
opennebula-flow \
opennebula-gate

$ /var/local/tutorial/configure_tutorial.sh

$ echo oneadmin:opennebula > /var/lib/one/.one/one_auth
```

If alls occurred fine, you can start the server:

```
$ service opennebula start

$ service opennebula-sunstone start

$ service libvirtd restart
```

Then switch to oneadmin user:

```
$ su - oneadmin

$ oneflow-server start

$ onegate-server start
```

Before you continue setting up the 'frontend' machine, you need
to setting up 'node' machine hypervisor:

```
$ ssh root@node

$ yum install opennebula-node-kvm

$ servie libvirtd restart
```

Now you can continue on the 'frontend node'. Execute the commands as
oneadmin from now on.

```
$ ssh-keyscan node1 node2 > ~/.ssh/know_hosts
```

You can test with:

```
$ ssh node1
$ ssh node2
```

On this point you have the frontend 

