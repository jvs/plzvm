# plzvm

A utility script for working with VirtualBox VMs on your macOS machine.

## Purpose

The point of **plzvm** is simply to make working with VMs a little easier.

You can use **plzvm** to create virtual machines, run them in "headless" mode,
and ssh into them from the command line. This allows you to use your favorite
terminal application on your Mac, and avoid the VirtualBox UI altogether.

You can also use **plzvm** for port forwarding. For example:
```
plzvm map 8888 to DevServer:80
```
This command forwards port 8888 on your Mac to port 80 on your VM. So if you
have a web server running on port 80 on your VM, you can view it in your Mac's
web browser using `localhost:8888`. (Note: To use this command, instead of
saying "DevServer", you'd use whatever you named your VM.)

There are a few more helpful commands, which you can read about in this document.
Additionally, the command `plzvm help` displays the usage information.


## Requirements

* [macOS](http://www.apple.com/macos/)
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* An ISO file. Some places where you can get one:
  * [Ubuntu Server ISO](https://www.ubuntu.com/download/server)
  * [Fedora Server ISO](https://getfedora.org/en/server/download/)
  * [Debian Images](https://www.debian.org/distrib/netinst)


## Installation

You can install **plzvm** using curl:
```
curl https://raw.githubusercontent.com/jvs/plzvm/master/plzvm --output /usr/local/bin/plzvm
chmod +x /usr/local/bin/plzvm
```

Or you can use git:
```
git clone https://github.com/jvs/plzvm.git
./plzvm/plzvm install
```

To uninstall **plzvm**:
```
plzvm uninstall
```


## Commands

* plzvm create ... (described below)
* plzvm delete \<name>
* plzvm start [name]
* plzvm stop [name]
* plzvm ssh [vm|user@vm]
* plzvm list vms
* plzvm list ports [vm]
* plzvm map \<port1> to [vm:]\<port2>
* plzvm unmap \<vm|localhost>:\<port>
* plzvm open [name]
* plzvm help


### plzvm create

Here's the command to create a new VM:
```
plzvm create <name>
               --iso <isopath> (required)
               [--ostype <ostype>] (optional)
               [--saveto <directory>] (default: $PWD)
               [--hdd <size>] (default: 32gb)
               [--ram <size>] (default: 1gb)
               [--vram <size>] (default: 128mb)
               [--sshport <port>] (optional)
```

Arguments:

* `plzvm create <name>`
  + `<name>` is the name of your VM.
  + For example, `plzvm create MyDevServer ...`
  + This argument is required.
* `--iso <isopath>`
  + This specifies the path to your ISO file.
  + For example, `--iso ~/Downloads/ubuntu-16.04.1-server-amd64.iso`
  + This argument is required.
* `--ostype <ostype>`
  + This indicates the type of OS you're installing.
  + For example, `--ostype Ubuntu_64`.
  + To see a list of supported OS types, run the command `vboxmanage list ostypes`.
  + This argument is optional.
* `--saveto <directory>`
  + This selects the directory where **plzvm** should save your VM's VDI file.
  + For example, `--saveto ~/vms` would save the VDI file in your ~/vms directory.
  + The default location is the current working directory.
* `--hdd <size>`
  + This is the size of your VM's hard disk.
  + Specify the size with an integer immediately followed by "gb" or "mb"
    (no spaces, case insensitive).
  + For example, `--hdd 64gb`.
  + The default value is 32gb.
* `--ram <size>`
  + This is the size of your VM's RAM.
  + For example, `--ram 2gb`.
  + The default value is 1gb.
* `--vram <size>`
  + is the size of your VM's video memory.
  + For example, `--vram 256mb`.
  + The default value is 128mb.
* `--sshport <port>`
  + This specifies the port your Mac should forward to port 22 on your VM.
  + For example, `--sshport 2222` would let you use `ssh -p 2222 localhost` to
    ssh into your VM.
  + (Or you could simply use `plzvm ssh <name>` to ssh into the VM.)
  + If this argument is omitted, then **plzvm** does not create a forwarding
    rule for port 22.


## Examples

### Creating a new VM:

```
plzvm create devserver \
    --iso ~/Downloads/ubuntu-16.04.1-server-amd64.iso \
    --ostype Ubuntu_64 \
    --saveto ~/vms \
    --hdd 64gb \
    --ram 2gb \
    --vram 128mb \
    --sshport 2222
```

This command creates a new VM called "devserver" using an Ubuntu ISO file from
your Downloads directory. It saves a file called "devserver.vdi" in your home
folder's "vms" directory.

The new virtual machine has 64 GB of RAM, 128 MB of video memory, and it has a
64 GB hard drive.

The setting `--sshport 2222` means that you are able to ssh into this machine
using port 2222 on your Mac's port. In order to connect to your VM using ssh,
must first install an ssh server on the VM.

In Ubuntu, the commands are:
```
sudo apt-get update
sudo apt-get install -y openssh-server
```
(These commands need to be run on the VM.)

Once ssh is installed on your VM, you can start it, ssh into it, and stop it
with these commands:
```
plzvm start devserver
plzvm ssh user@devserver
plzvm stop devserver
```
(In this case, you'd use your VM's name instead of "devserver" and you'd use
your account name on the VM instead of "user".)


### Starting and stopping VMs:

* `plzvm start` - Starts your default VM.
* `plzvm stop` - Stops your default VM.
* `plzvm start foo` - Starts the VM named "foo".
* `plzvm stop foo` - Stops the VM named "foo".
* `plzvm open` - Runs the VM in a GUI window.


#### Your default VM:

Your "default" VM is the first VM that appears when you run `plzvm list vms`.
(So if you only have one VM, then that's your default.)


### Connecting to VMs:

* `plzvm ssh` - Connects to your default VM as $USER.
* `plzvm ssh foo` - Connects to the VM named "foo" as $USER.
* `plzvm ssh alice@foo` - Connects to the VM named "foo" as the user "alice".


### Listing VMs and mapped ports:

* `plzvm list vms` - Lists your available VMs.
* `plzvm list ports` - Lists the mapped ports on all your VMs.
* `plzvm list ports foo` - Lists the mapped ports on the VM named "foo".


### Mapping and unmapping ports:

* `plzvm map 8080 to 80` - Maps your Mac's port 8080 to port 80 on your default VM.
* `plzvm map 2525 to foo:25` - Maps your Mac's port 2525 to port 25 on the VM named "foo".
* `plzvm unmap localhost:8080` - Unmaps your Mac's port 8080 from one of your VM's ports.
* `plzvm unmap foo:25` - Unmaps one of your Mac's ports from port 25 on the VM named "foo".


## Acknowledgments

**plzvm** is based on [an article by Lee Mendelowitz](https://leemendelowitz.github.io/blog/ubuntu-server-virtualbox.html).


## License

[MIT License](https://github.com/jvs/plzvm/blob/master/LICENSE) (c) 2017 jvs
