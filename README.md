# plzvm

A utility script for working with your Ubuntu Server VMs on your macOS machine.

This tool is based on the set up procedure [described by Lee Mendelowitz](https://leemendelowitz.github.io/blog/ubuntu-server-virtualbox.html).


## Requirements

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* [Ubuntu Server ISO](https://www.ubuntu.com/download/server)
* [macOS](http://www.apple.com/macos/)


## Installation

Using curl:
```
$ curl https://raw.githubusercontent.com/jvs/plzvm/master/plzvm --output /usr/local/bin/plzvm
$ chmod +x /usr/local/bin/plzvm
```

Or using git:
```
$ git clone https://github.com/jvs/plzvm.git
$ cd plzvm
$ ./plzvm install
```

To uninstall:
```
$ plzvm uninstall
```


## Commands

* plzvm create \<name> \<iso> [disksizeMB]
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


## Examples

### Starting and stopping VMs:

* `plzvm start` - Starts your default VM.
* `plzvm stop` - Stops your default VM.
* `plzvm start foo` - Starts the VM named "foo".
* `plzvm stop foo` - Stops the VM named "foo".
* `plzvm open` - Runs the VM in a GUI window.


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


## License

[MIT License](https://github.com/jvs/plzvm/blob/master/LICENSE) (c) 2017 jvs
