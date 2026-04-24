Enter the terminal, and enter:
`sudo nano /etc/systemd/network/20-end0.network`

change the file to addAdd "ClientIdentifier=mac"

```
[Match]
Name=end0

[Network]
DHCP=ipv4

[DHCPv4]
ClientIdentifier=mac
```

