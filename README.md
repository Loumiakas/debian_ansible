# Debian development environment

This repository contains ansible scripts that sets up the development
environment for the Debian systems.

Currently, notable tasks are:
* Developer user account creation 
* Arduino development environment creation
* Latest vim compilation/installation from source
* Latest tmux compilation/installation from source
* SDCC 3.5.0 compilation/installation from source
* NRF BLE (Bluetooth Low Energy) sniffer command line utilities installation
* RFCat installation
* Dotfiles

## Provisioning with Vagrant

Symbolically link the directory, so it could accessed from the guest VM:
```ruby
config.vm.synced_folder ".", "/vagrant"
config.vm.synced_folder "<PATH_TO_THIS_REPOSITORY>", "/vm_setup"
```

Example ansible provision block for Vagrantfile:
```ruby
# Run Ansible from the Vagrant Host
config.vm.provision "ansible_local" do |ansible|
    ansible.become = true
    ansible.playbook = "/vm_setup/setup.yml"
    ansible.extra_vars = { ansible_python_interpreter:"/usr/bin/python3" }
end
```
Optional settings:
```ruby
  config.vm.provider "virtualbox" do |vb|
    # Set the box name
    vb.name = "Linux"
    # Display the VirtualBox GUI when booting the machine
    vb.gui = true

    # Customize the amount of memory on the VM:
    vb.memory = "4096"
    # Enable USB
    vb.customize ["modifyvm", :id, "--usb", "on"]
    vb.customize ["modifyvm", :id, "--usbehci", "on"]
    #
    # CAPTURE DEVICES
    #
    # YARDStick One
    vb.customize ["usbfilter", "add", "0",
                  "--target", :id,
                  "--name", "YARDStick One",
                  "--vendorid", "0x1d50",
                  "--productid", "0x605b",
                  "--product", "YARD Stick One",
                  "--manufacturer", "Great Scott Gadgets"]
    # HackRF One
    vb.customize ["usbfilter", "add", "1",
                  "--target", :id,
                  "--name", "HackRF One",
                  "--vendorid", "0x1d50",
                  "--productid", "0x6089",
                  "--product", "HackRF One",
                  "--manufacturer", "Great Scott Gadgets"]
    # Ubertooth One
    vb.customize ["usbfilter", "add", "2",
                  "--target", :id,
                  "--name", "Ubertooth One",
                  "--vendorid", "0x1d50",
                  "--productid", "0x6002",
                  "--product", "Ubertooth One",
                  "--manufacturer", "Great Scott Gadgets"]
    # JLink Segger
    vb.customize ["usbfilter", "add", "3",
                  "--target", :id,
                  "--name", "JLink Bluetooth Devboard",
                  "--vendorid", "0x1366",
                  "--productid", "0x1015",
                  "--product", "J-Link",
                  "--manufacturer", "SEGGER"]
    # Arduino Uno
    vb.customize ["usbfilter", "add", "5",
                  "--target", :id,
                  "--name", "Arduino Uno",
                  "--vendorid", "0x2341",
                  "--productid", "0x0043",
                  "--product", "IOUSBHostDevice",
                  "--manufacturer", "Arduino (www.arduino.cc)"]
    # Arduino  Uno / ESP8266 (Chinese)
    vb.customize ["usbfilter", "add", "6",
                  "--target", :id,
                  "--name", "Chinese Device",
                  "--vendorid", "0x1a86",
                  "--productid", "0x7523"]
    # Arduino Mega
    vb.customize ["usbfilter", "add", "7",
                  "--target", :id,
                  "--name", "Arduino Mega",
                  "--vendorid", "0x2341",
                  "--productid", "0x0042",
                  "--product", "IOUSBHostDevice",
                  "--manufacturer", "Arduino (www.arduino.cc)"]
    # ST Link Nucleo
    vb.customize ["usbfilter", "add", "8",
                  "--target", :id,
                  "--name", "ST Link Nucleo",
                  "--vendorid", "0x0483",
                  "--productid", "0x374b",
                  "--product", "STM32 STLink",
                  "--manufacturer", "STMicroelectronics"]
    # USB Bluetooth dongle
    vb.customize ["usbfilter", "add", "9",
                  "--target", :id,
                  "--name", "Bluetooth Dongle",
                  "--vendorid", "0x0a12",
                  "--productid", "0x0001",
                  "--manufacturer", "Cambridge Silicon Radio, Ltd"]
    # Rasberry Pi Pico (Normal Mode)
    vb.customize ["usbfilter", "add", "10",
                  "--target", :id,
                  "--name", "Arduino RaspberryPi Pico [0101]",
                  "--vendorid", "0x2e8a",
                  "--productid", "0x00c0",
                  "--manufacturer", "Arduino"]
    # Rasberry Pi Pico (Bootloader Mode)
    vb.customize ["usbfilter", "add", "11",
                  "--target", :id,
                  "--name", "Raspberry Pi RP2 Boot [0100]",
                  "--vendorid", "0x2e8a",
                  "--productid", "0x0003",
                  "--manufacturer", "Raspberry Pi"]
    # Teensy (Normal Mode)
    vb.customize ["usbfilter", "add", "11",
                  "--target", :id,
                  "--name", "Teensy",
                  "--vendorid", "0x16c0",
                  "--productid", "0x0486",
                  "--product", "Teensyduino RawHID",
                  "--manufacturer", "Teensyduino"]
    # Teensy (Bootloader Mode)
    vb.customize ["usbfilter", "add", "12",
                  "--target", :id,
                  "--name", "Teensy Bootloader",
                  "--vendorid", "0x16c0",
                  "--productid", "0x0478"]
    # ESP32 (Normal Mode)
    vb.customize ["usbfilter", "add", "13",
                  "--target", :id,
                  "--name", "ESP32",
                  "--vendorid", "0x10c4",
                  "--productid", "0xea60",
                  "--product", "CP2102 USB to UART Bridge Controller",
                  "--manufacturer", "Silicon Labs"]
    # STM32 STLink
    vb.customize ["usbfilter", "add", "14",
                  "--target", :id,
                  "--name", "STLink",
                  "--vendorid", "0x0483",
                  "--productid", "0x3748",
                  "--product", "STM32 STLink",
                  "--manufacturer", "STMicroelectronics"]
    # STM32 Blackpill
    vb.customize ["usbfilter", "add", "15",
                  "--target", :id,
                  "--name", "STM32 Blackpill",
                  "--vendorid", "0x0483",
                  "--productid", "0xdf11",
                  "--product", "STM32  BOOTLOADER",
                  "--manufacturer", "STMicroelectronics"]
    # Bus Pirate v4
    vb.customize ["usbfilter", "add", "16",
                  "--target", :id,
                  "--name", "Bus Pirate v4",
                  "--vendorid", "0x04d8",
                  "--productid", "0xfb00",
                  "--product", "Bus Pirate V4",
                  "--manufacturer", "Dangerous Prototypes"]
  end
```
