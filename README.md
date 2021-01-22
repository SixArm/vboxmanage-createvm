# Create a VirtualBox virtual machine for our preferred systems

Syntax:

    vboxmanage-createvm [options] <vm path> <iso path>

Options:

  * --ostype= operating system type (default: autodetect via VM name)

  * --cpus= cpu count (default: 1)

  * --memory= memory size in megabytes (default: 1024)

  * --vram= video RAM size in megabytes (default: 128)

  * --storage= storage size in megabytes (default: 10240)

  * --hostname= (default: my.example.com)
 
  * --username= (default: user)

  * --password= (default: secret)

  * --post-install-command= (example: "sudo apt-get upgrade")


## Examples

Here are some popular examples that use long descriptive VM names.

For the VM name, we favor this naming convention:

  * operating system, such as "debian" or "ubuntu"
   
  * version, such as "10.7.0" or "20.04"
  
  * flavor, such as "desktop" or "server" (optional)
  
  * architecture, such as "amd64" or "x86-64"

Example for Debian network installation:

    vboxmanage-createvm \
    $HOME/vm/debian-10.7.0-amd64 \
    $HOME/iso/debian-10.7.0-amd64-netinst.iso

Example for Ubuntu desktop:

    vboxmanage-createvm Ubuntu_64 \
    $HOME/vm/ubuntu-20.04-desktop-amd64 \
    $HOME/iso/ubuntu-20.04-desktop-amd64.iso

Example for Fedora server DVD installation:

    vboxmanage-createvm \
    $HOME/vm/fedora-32.1.6-server-x86-64 \
    $HOME/iso/Fedora-Server-dvd-x86_64-32-1.6.iso \

Example for 2 CPUs, 2 gigabytes of memory, 20 gigabytes of storage:

    vboxmanage-createvm -c 2 -m 2048 -s 20480 …


## Paths

The VM path is typically within your existing VirtualBox
virtual machine path, because this program will create the
disk drive file within that directory.

The ISO path can be wherever you have the ISO file.


## Conventions

Conventions for ease of use:

  * The operating system type is automatically guessed 
    if the VM path basename begins with Debian, Fedora, Ubuntu.
    You can override this by using `-o`.

  * The command that will run post-install is automatically set
    if the `ostype` begins with Debian, Fedora, Ubuntu.
    You can override this by using `-c`.


## Purpose

The purpose of this command is to do a simple setup
for new users, akin to a tutorial that is easy to change.

This command is not attempting to be a full featured
installation such as with all settings configurable via
command line options.

We welcome constructive feedback.


## Settings

This program uses our preferred settings, and you can
edit this program to use the settings that you want. 

This program uses our organization's configuration:

  * Operating system current version

  * United States English

  * UTC time zone


## Related commands

List vms:

    vboxmanage list vms

Delete a vm:

    vboxmanage unregistervm <uuid> --delete

Remove a vm that is inaccessible:

    vboxmanage unregistervm <uuid>

List drives:

    vboxmanage list hdds

Delete a drive:

    vboxmanage closemedium <uuid> --delete


## Formats for disks

We typically use these formats:

  * VMDK: VMWare uses VMDK as the default disk image format.
    Multipe VMDK versions and variations exist, so it’s important
    to understand which one you’re using and where it can be used.

  * VDI: VirtualBox uses VDI as the default disk image format.
    VDI is not compatible with VMWare


## Encryption

Question: Are there performance and/or security advantages to using 
the VirtualBox Disk Encryption over OS disk encryption in the VM?

Answer: As I understand it, a good point of the VirtualBox encryption is 
you can easily change your mind, encrypt a VM that isn't or decrypt a VM
which is, and use the result with VirtualBox. Making a decrypted image 
from an Ubuntu LUKS-encrypted one and vice-versa is likely possible but 
would be more complicated.

Also, with the VirtualBox encryption you can store the encryption 
passphrase in the VB config outside of the VM, so you can boot the VM
without having to enter a decryption passphrase. Of course you have to
keep the VB config safe to avoid disclosure of the passphrase.

Another benefit of using VirtualBox encryption is that you can securely
save the VM state to resume later. Encrypted VMs have their state file 
encrypted as well. In contrast, if you use Ubuntu full-disk encryption 
features of the guest, then saving the VM state will effectively leak 
the encryption key to the host's storage in the state file.

See https://superuser.com/questions/1445735/rtualbox-disk-encryption-vs-ubuntu-vm-disk-encryption


## How to encrypt

To support VirtualBox Disk Encryption of the virtual machine, you need 
to install VirtualBox Extension Pack, available at the VirtualBox site.

The Extension Pack is not included by default, because it can contain 
system level software that could be potentially harmful to your system.

The version of Extension Pack needs to match your VirtualBox version.
So in case of installation issues, try to shut down all your VMs, then
then upgrade your VirtualBox.

After you install Extension Pack, the encrypt operation can use the
command-line interface with this syntax:

    VBoxManage encryptmedium "uuid|filename" \
    --newpassword "file|-" \
    --cipher "cipher id" \
    --newpasswordid "id"

See https://superuser.com/questions/1072752/how-to-encrypt-vm-box-via-vboxmanage


## Other resources

https://spin.atomicobject.com/2013/06/03/ovf-virtual-machine/

https://github.com/jedi4ever/veewee


## Tracking

* Command: vboxmanage-createvm
* Version: 2.7.1
* Created: 2018-10-20
* Updated: 2021-01-22T19:16:34Z
* License: GPL-2.0-only
* Contact: Joel Parker Henderson (https://joelparkerhenderson.com)
