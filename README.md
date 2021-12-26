# Debian development environment

This repository contains ansible scripts that sets up the development
environment for the Debian systems.

Currently, notable added features are:
* User account creation 
* Arduino developement environment
* Latest vim compilation/install from source
* Latest tmux compilation/install from source
* NRF BLE (Bluetooth Low Energy) sniffer command line utilities
* SDCC 3.5.0 compilation/install from source
* RFCat installation
* Dotfiles

### Provisioning with vagrant

Add the following block into Vagrantfile:

```ruby
# Run Ansible from the Vagrant Host
config.vm.provision "ansible_local" do |ansible|
    ansible.become = true
    ansible.playbook = "/vm_setup/setup.yml"
    ansible.extra_vars = { ansible_python_interpreter:"/usr/bin/python3" }
end
```

