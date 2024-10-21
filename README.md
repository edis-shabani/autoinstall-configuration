# Ubuntu Autoinstall Configuration
This repository contains an Ubuntu Autoinstall configuration file designed to automate the installation process of Ubuntu systems. The configuration is based on the guidelines provided in the Autoinstall Reference Manual.

## Overview
The autoinstall configuration file is written in YAML format and includes various sections to automate the installation process. Below is a detailed explanation of each section in the configuration file.

### Version
```yaml
version: 1
```

This specifies the version of the autoinstall schema being used. Currently, only version 1 is supported.

### Identity
```yaml
identity:
  hostname: ubuntu-server
  username: ubuntu
  password: $6$rounds=4096$abc123...
```

hostname: Sets the hostname of the system.\
username: Defines the default user account.\
password: Encrypted password for the user account. Use a tool like mkpasswd to generate this.

### Keyboard Configuration

```yaml
keyboard:
  layout: us
  variant: ''
```

layout: Keyboard layout (e.g., us for United States).\
variant: Keyboard variant, if any.

### Locale Settings
```yaml
locale: en_US.UTF-8
```

Sets the system locale to en_US.UTF-8.

### Network Configuration
```yaml
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: true
```

Configures the network interface eth0 to use DHCP for IPv4.

### Storage Configuration
```yaml
storage:
  layout:
    name: lvm
    match:
      size: largest
    volumes:
      - id: root
        size: 100%
        type: ext4
```

Defines the storage layout using LVM with a single volume for the root filesystem.

### User Data
```yaml
user-data:
  disable_root: true
  ssh:
    install-server: true
    allow-pw: false
    authorized-keys:
      - ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQE...
```

disable_root: Disables the root account.\
ssh: Configures SSH settings, including disabling password authentication and adding an authorized key.

### Late Commands
```yaml
late-commands:
  - curtin in-target --target=/target apt-get -y install htop
```

Specifies commands to run late in the installation process. In this example, htop is installed.

## Usage
To use this autoinstall configuration, include the YAML file in your installation media or provide it via a network location accessible during the installation process.

### Autoinstall by way of cloud-config

When providing autoinstall via cloud-init, the autoinstall configuration is provided as **Cloud config data**. This means the file requires a `#cloud-config` header and the autoinstall directives are placed under a top level `autoinstall:` key:

```yaml
#cloud-config
autoinstall:
    version: 1
    ....
```

The configuration has to be in a file named `user-data`. Also, an empty file named `meta-data`  has to be present in the same directory.

When the machine starts, press and hold the following key to launch the GRUB menu:
* on machines with `BIOS` / `MBR`: the left `SHIFT` key
* on machines with `UEFI` / `GPT`: the `ESC` key

Edit the grub menu entry so it points at the root directory of the `user-data` configuration file:

```
linux /casper/vmlinuz autoinstall ds=nocloud-net\;s=http://192.168.10.10:3003/ ---
```



For more details on how to use autoinstall configurations, refer to the [Autoinstall Reference Manual](https://canonical-subiquity.readthedocs-hosted.com/en/latest/reference/autoinstall-reference.html).
