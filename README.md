# DNS-Configuration

## Introduction

The Domain Name System (DNS) is the phonebook of the Internet. Humans access information online through domain names, like nytimes.com or espn.com. Web browsers interact through Internet Protocol (IP) addresses. DNS translates domain names to IP addresses so browsers can load Internet resources. In order to have a DNS server, we will use BIND9.

BIND is a suite of software for interacting with the Domain Name System. Its most prominent component, named, performs both of the main DNS server roles, acting as an authoritative name server for DNS zones and as a recursive resolver in the network.

## Configuration

### But first ...

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

We need to edit the default part and add a domain name. Here is an example:

```sh
127.0.0.1       localhost
127.0.1.1       host-name.mydomain host-name
192.xxx.x.xx    host-name.mydomain host-name
```

`192.xxx.x.xx` is the IP address we retrieved moments ago.

### Bind

`Bind` files are located in inside `/etc/bind` repository. This repository contains many files used to configure the DNS Server. The first one we are going to configure is `named.conf.option`. We are going to add the following lines :

```sh
    ...
    recursion yes;
    listen-on {192.xxx.x.xx;};
    allow-transfer {none;};

    forwarders {
    192.xxx.x.2;
    };
```

Next file to configure is `named.conf.local` to create forward lookup zone. We can add the following lines:

```sh
zone "my-domain-name.local" IN {
        type master;
        file "/etc/bind/db.my-domain-name.local";
};
//reverse lookup zone
zone "x.xxx.192.in-addr.arpa" IN {
        type master;
        file "/etc/bind/db.x.xxx.192";
};
```
Note: 
- `x.xxx.192` is our IP address in reverse excluding the last two digits.
- `type master` means this is going too be our primary DNS Server.
- `file` indicates the database file the zone will use.

Now we need to create records for forward lookup zone databased and reverse forward lookup zone database. Remebre in `named.conf.local`, we named these files as `db.my-domain-name.local` and `db.x.xxx.192`. We also indicated these files should be inside the `/etc/bind` repository. The easiest way to do so is to copy the db.local and rename it. So let start with `db.my-domain-name.local`.

```sh
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     ns1.my-domain-name.local. root.my-domain-name.local. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.my-domain-name.local.
ns1     IN      A       192.xxx.x.xx
www     IN      A       192.xxx.x.xx
ftp     IN      A       192.xxx.x.xx
@       IN      MX      10      mail
mail    IN      A       192.xxx.x.xx
@       IN      AAAA    ::1
```

14:40
