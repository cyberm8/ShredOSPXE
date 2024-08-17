# ShredOSPXE

Ansible playbook to deploy a PXE server to serve ShredOS.

## ShredOS

[ShredOS](https://github.com/PartialVolume/shredos.x86_64) is a small
Linux distribution that embeds [nwipe](https://github.com/PartialVolume/nwipe),
a tool to wipe disks. ShredOS can be booted via USB or network (PXE).

## Network boot

Two elements are involved : the computer to be purged (the client) and a
third-party machine (the server).

The server hosts three services:

* a DHCP server ;
* a TFTP server ;
* an FTP server.

To avoid polluting the local network with a DHCP server, it is assumed that
the server has a dedicated network interface.

### DHCP server

The client first retrieves a network configuration via the DHCP server.

In addition to the usual configurations (IP address, mask, gateway), the server
provides additional options, including where to download the ShredOS boot image.

### TFTP server

The boot image is stored on a TFTP server.
Once the image has been loaded into RAM, ShredOS starts.

TFTP has a few drawbacks, especially if the server is
running Debian, as the maximum achievable throughput is 4 MB/s.

There seem to be workarounds: running a server that runs a Redhat-based
distribution (never tested) or using [iPXE](https://ipxe.org/) instead of the TFTP server.

### FTP server

The FTP server is not involved during the network boot.

This is a non-essential component, but useful if you want to control ShredOS
configuration and archive PDF reports.

## Ansible

Ansible is a tool for deploying elements on a machine. For example, software,
configuration files, users, services, etc.

To sum up, the aim of this repository is to configure the server.
