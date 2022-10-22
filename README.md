# Installation of Unmanaged Server

Before any installation. Just run ```sudo apt-get update``` or ```sudo apt update``` to update the Ubuntu/Debian package list.

## Install Apache, MySQL/MariaDB, PHP (LAMP) stack on Ubuntu

First of all, if you doesn't have ```nano``` or ```vim``` editor installed in Lunix. You can install my with commands:

### Install VIM

```install vim command
sudo apt update
sudo apt install vim
```

### Install Nano

```install nano command
sudo apt update
sudo apt install nano
```

## Installation of Firewall

If you doesn't have firewall installed: 
For more: [How To Set Up a Firewall with UFW on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-22-04)

### Install Firewall

```install firewall command
sudo apt update
sudo apt install ufw
```

---

1. Apache Server
2. Second item
3. Third item

## Install Apache Server

```install apache server
sudo apt update
sudo apt install apache2 apache2-utils
```

Now add apache in firewall list, Use ```sudo ufw app list``` to show the list of allowed protocols.

```add apache in firewall list
sudo ufw allow in "Apache"
sudo ufw allow in "Apache Full"
sudo ufw allow in "Apache Secure"
sudo ufw allow in "OpenSSH"
```

Enable the firewall

```text
sudo ufw status
sudo ufw enable
```
