sudo nano /etc/systemd/network/20-end0.network

```
[Match]
Name=end0

[Network]
DHCP=ipv4

[DHCPv4]
ClientIdentifier=mac
```
