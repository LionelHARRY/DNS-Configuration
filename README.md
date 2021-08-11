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

### Installation

```sh
sudo apt install bind9
```
We can then check if Bind9 is install with this command:
```sh
named -v
```
We should be able to see the server's name and the release status.


