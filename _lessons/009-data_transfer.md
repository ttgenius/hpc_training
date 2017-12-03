---
layout: post
title: Kupe Data storage and transfer
---

You will learn how to transfer data to and from Kupe.

### Transferring small amounts of data

For small amounts of data, SSH based utilities such as SecureCopy (`scp`) can be used. `scp` should be readily available on a terminal on Linux and MacOS, or via [WinSCP](https://winscp.net/eng/download.php) and [MobaXterm](https://mobaxterm.mobatek.net) on Windows. Note that two-factor authentication will be required for file transfer sessions.

The lander node filestem is distinct from the Kupe filesystem, and so copying a file from your desktop to Kupe is by default a two-step process.  If you are using an OpenSSH based ssh, which will be the case for most MacOSX and Linux users, then this inconvenience can be avoided by setting up an SSH config file such as:
```
Host kupe01
   User your_username
   Hostname kupe01
   ProxyCommand ssh -W %h:%p lander.nesi.org.nz
```
With that file present at `~/.ssh/config`, the command `ssh kupe01` should take you there directly, jumping across the lander node on the way, and `scp` will do the same for your file transfers.

There is also the problem of having to repeatedly provide both authentication factors every time you use `scp`.  The following method allows passwordless `scp` while you have an `ssh` session open, but it does bring some complications and so is not recommended unless you often use `scp`.  First create the directory `~/.ssh/sockets` on your machine, and then set up your `~/.ssh/config` with:
```
ControlPath ~/.ssh/sockets/%r@%h:%p
Host kupe01
   User your_username
   Hostname kupe01
   ProxyCommand ssh -W %h:%p lander.nesi.org.nz
   ControlMaster auto
   ControlPersist 1
```

For full details on this see https://en.wikibooks.org/wiki/OpenSSH/Cookbook/Multiplexing

### Transferring large amounts of data (500GB or more)


Instructions on how to use [Globus Data Transfer Node (DTN)](../assets/resources/NeSI_DTN_End_User_Globus_2.00.pdf)

[Globus](https://www.globus.org) provides a fast file transfer service that is suitable for large data volumes. Globus requires two endpoints, on at your institution, and on the HPC. Data transfer sessions can be set up and monitored on the Globus webpage. Globus also provides APIs:

 * [Globus Auth](https://docs.globus.org/api/auth/)
 * [Globus Transfer API](https://docs.globus.org/api/transfer/)

There is a [Software Development Kit (SDK) in Python](http://globus-sdk-python.readthedocs.io/en/latest/) to work with the API.

### Automatic transfer between HPC sites

Automatic file space synchronisation between the Wellington and Auckland HPC sites using Spectrum Scale AFM (Active File Management) will make sure that user data will be accessible at both sites.