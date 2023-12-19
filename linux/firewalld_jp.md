# [[firewalld]]

Firewalld provides a dynamically managed firewall with support for network/firewall zones that define the trust level of network connections or interfaces.

## Features

- Complete D-Bus API
- IPv4, IPv6, bridge and ipset support
- IPv4 and IPv6 NAT support
- Firewall zones
- Predefined list of zones, services and icmptypes
- Simple service, port, protocol, source port, masquerading, port forwarding, icmp filter, rich rule, interface and source address handlig in zones
- Simple service definition with ports, protocols, source ports, modules (netfilter helpers) and destination address handling
- Rich Language for more flexible and complex rules in zones
- Timed firewall rules in zones
- Simple log of denied packets
- Direct interface
- Lockdown: Whitelisting of applications that may modify the firewall
- Automatic loading of Linux kernel modules
- Integration with Puppet
- Command line clients for online and offline configuration
- Graphical configuration tool using gtk3
- Applet using Qt5

## Applications and libraries that support firewalld

- [NetworkManager](https://wiki.gnome.org/Projects/NetworkManager)
- [libvirt](http://libvirt.org/)
- [podman](https://podman.io/)
- [docker](http://docker.com/) (iptables backend only)
- [fail2ban](http://www.fail2ban.org/)

---

## Default Configuration of firewalld Zones

|Zone name|Default configuration|
|---|---|
|trusted|Allow all incoming traffic.|
|home|Reject incoming traffic unless related to outgoing traffic or matching the ssh, mdns, ipp-client, samba-client, or dhcpv6-client predefined services.|
|internal|Reject incoming traffic unless related to outgoing traffic or matching the ssh, mdns, ipp-client, samba-client, or dhcpv6-client predefined services (same as the home zone to start with).|
|work|Reject incoming traffic unless related to outgoing traffic or matching the ssh, ipp – client, or dhcpv6 – client predefined services.|
|public|Reject incoming traffic unless related to outgoing traffic or matching the ssh or dhcpv6 – client predefined services. The default zone for newly added network interfaces|
|external|Reject incoming traffic unless related to outgoing traffic or matching the ssh predefined service. Outgoing 1Pv4 traffic forwarded through this zone is masqueraded to look like it originated from the 1Pv4 address of the outgoing network interface.|
|dmz|Reject incoming traffic unless related to outgoing traffic or matching the ssh predefined service.|
|block|Reject all incoming traffic unless related to outgoing traffic.|
|drop|Drop all incoming traffic unless related to outgoing traffic (do not even respond with ICMP errors).|

## Firewalld Commandline reference

| firewall -cmd Commands                       | Explanation                                                                                                                                                                       |
| -------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| --get-default-zone                            | Query the current default zone.                                                                                                                                                   |
| --set-default-zone=[ZONE]                     | Set the default zone. This changes both the runtime and the permanent configuration.                                                                                              |
| --get-zones                                   | List all available zones.                                                                                                                                                         |
| --get-services                                | List all predefined services.                                                                                                                                                     |
| --get-active-zones                            | List all zones currently in use (have an interface or source tied to them), along with their interface and source information.                                                    |
| --add-source=[CIDR] [ –zone=[ZONE]            | Route all traffic coming from the IP address or network/netmask [CIDR] to the specified zone. If no –zone= option is provided, the default zone will be used.                     |
| --remove-source=[CIDR] [ –zone=[ZONE]         | Remove the rule routing all traffic coming from the IP address or network/netmask [CIDR] from the specified zone. If no –zone= option is provided, the default zone will be used. |
| --add-interface=[INTERFACE] [ –zone=[ZONE]    | Route all traffic coming from [INTERFACE] to the specified zone. If no –zone= option is provided, the default zone will be used.                                                  |
| --change -interface=[INTERFACE] [–zone=[ZONE] | Associate the interface with [ZONE] instead of its current zone. If no –zone= option is provided, the default zone will be used.                                                  |
| --list-all [–zone=[ZONE]]                     | Listallconfiguredinterfaces, sources, services, and ports for [ZONE]. If no –zone= option is provided, the default zone will be used.                                             |
| --list-all-zones                              | Retrieve all information for all zones (interfaces, sources, ports, services, etc.).                                                                                              |
| --add-service=[SERVICE]                       | Allow traffic to [SERVICE]. If no –zone= option is provided, the default zone will be used.                                                                                       |
| --add-port=[PORT/PROTOCOL]                    | Allow traffic to the [PORT/ PROTOCOL] port(s). If no –zone= option is provided, the default zone will be used.                                                                    |
| --remove-service=[SERVICE]                    | Remove [SERVICE] from the allowed list for the zone. If no –zone= option is provided, the default zone will be used.                                                              |
| --remove-port=[PORT/PROTOCOL]                 | Remove the [PORT/PROTOCOL] port(s) from the allowed list for the zone. If no –zone= option is provided, the default zone will be used.                                            |
| --reload                                      | Drop the runtime configuration and apply the persistent configuration.                                                                                                            |

---

## Command Examples

```bash
firewall-cmd --reload    # To reload the permanent rules without interrupting existing persistent connections
```

### list details of default and active zones

```bash
firewall-cmd --get-default-zone
firewall-cmd --get-active-zones
firewall-cmd --list-all
```

### add/remove interfaces to zones

To add interface “eth1” to “public” zone.

```bash
firewall-cmd --zone=public --change-interface=eth1
```

### list/add/remove services to zones

To add “samba and samba-client” service to a specific zone. You may include, “permanent” flag to make this permanent change.

```bash
firewall-cmd --zone=public --add-service=samba --add-service=samba-client --permanent
```

### list and Add ports to firewall

```bash
firewall-cmd --list-ports
firewall-cmd --zone=public --add-port=5000/tcp
```


### Allow all IPv4 traffic from host 192.0.2.0.

```bash
sudo firewall-cmd --zone=public --add-rich-rule 'rule family="ipv4" source address=192.0.2.0 accept'
```

### Deny IPv4 traffic over TCP from host 192.0.2.0 to port 22.

```bash
sudo firewall-cmd --zone=public --add-rich-rule 'rule family="ipv4" source address="192.0.2.0" port port=22 protocol=tcp reject'
```

### Allow IPv4 traffic over TCP from host 192.0.2.0 to port 80, and forward it locally to port 6532.

```bash
sudo firewall-cmd --zone=public --add-rich-rule 'rule family=ipv4 source address=192.0.2.0 forward-port port=80 protocol=tcp to-port=6532'
```

### Forward all IPv4 traffic on port 80 to port 8080 on host 198.51.100.0 (masquerade should be active on the zone).

```bash
sudo firewall-cmd --zone=public --add-rich-rule 'rule family=ipv4 forward-port port=80 protocol=tcp to-port=8080 to-addr=198.51.100.0'
```

### To list your current Rich Rules in the public zone:

```bash
sudo firewall-cmd --zone=public --list-rich-rules
```

### Remove rich rule

```bash
sudo firewall-cmd --zone=public --remove-rich-rule 'rule rulename'
```

### Remove  rich rule - Allow all IPv4 traffic from host 192.0.2.0.

```bash
sudo firewall-cmd --zone=public --remove-rich-rule 'rule family="ipv4" source address=192.0.2.0 accept'
```
