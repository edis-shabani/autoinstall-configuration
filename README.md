Ubuntu Autoinstall Configuration
This repository contains an Ubuntu Autoinstall configuration file designed to automate the installation process of Ubuntu systems. The configuration is based on the guidelines provided in the Autoinstall Reference Manual.

Configuration File
The autoinstall configuration file can be found here.

Explanation of Configuration
Overview
The autoinstall configuration file is written in YAML format and includes various sections to automate the installation process. Below is a detailed explanation of each section in the configuration file.

Version
version: 1

This specifies the version of the autoinstall schema being used. Currently, only version 1 is supported.

Identity
identity:
  hostname: ubuntu-server
  username: ubuntu
  password: $6$rounds=4096$abc123...

hostname: Sets the hostname of the system.
username: Defines the default user account.
password: Encrypted password for the user account. Use a tool like mkpasswd to generate this.
Keyboard Configuration
keyboard:
  layout: us
  variant: ''

layout: Keyboard layout (e.g., us for United States).
variant: Keyboard variant, if any.
Locale Settings
locale: en_US.UTF-8

Sets the system locale to en_US.UTF-8.

Network Configuration
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: true

Configures the network interface eth0 to use DHCP for IPv4.

Storage Configuration
storage:
  layout:
    name: lvm
    match:
      size: largest
    volumes:
      - id: root
        size: 100%
        type: ext4

Defines the storage layout using LVM with a single volume for the root filesystem.

User Data
user-data:
  disable_root: true
  ssh:
    install-server: true
    allow-pw: false
    authorized-keys:
      - ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQE...

disable_root: Disables the root account.
ssh: Configures SSH settings, including disabling password authentication and adding an authorized key.
Late Commands
late-commands:
  - curtin in-target --target=/target apt-get -y install htop

Specifies commands to run late in the installation process. In this example, htop is installed.

Usage
To use this autoinstall configuration, include the YAML file in your installation media or provide it via a network location accessible during the installation process.

For more details on how to use autoinstall configurations, refer to the Autoinstall Reference Manual.