# DNS-Configuration

## Introduction

The Domain Name System (DNS) is the phonebook of the Internet. Humans access information online through domain names, like nytimes.com or espn.com. Web browsers interact through Internet Protocol (IP) addresses. DNS translates domain names to IP addresses so browsers can load Internet resources. In order to have a DNS server, we will use BIND9.

BIND is a suite of software for interacting with the Domain Name System. Its most prominent component, named, performs both of the main DNS server roles, acting as an authoritative name server for DNS zones and as a recursive resolver in the network.

## Configuration

### But first...

Whe can start by checking the IP address of the machine we are using. With Ubuntu, we can symply use this command : 
```sh
ip addr
```
In our case, we are going to use the wlan0 inet.

Next, check the computer's name and other useful informations:
```sh
hostnamectl status
```

### Installation

```sh
sudo apt install bind9
```
We can then check if Bind9 is install with this command:
```sh
named -v
```
We should be able to see the server's name and the release status.

Now if we go to `/etc/bind`, we can see a couple of files that are going to be useful for configurating a DNS.

### Hosts file

This file translates hostnames to IP addresses. It is located in the `/etc`. The hosts file only contains several lines:
- The first part, by default, contains the hostnames and IP addresses of your localhost and machine. This is the part you will usually modify to make the desired changes.
- The second part has information about IPv6 capable hosts and you will hardly be editing these lines.

Whenever you type an address, your system will check the hosts file for its presence; if it is present there, you will be directed to the corresponding IP. If the hostname is not defined in the hosts file, your system will check the DNS server of your internet to look up for the corresponding IP and redirect you accordingly.



