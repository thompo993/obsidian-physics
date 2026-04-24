Enter the terminal, and enter:
`sudo nano /etc/systemd/network/20-end0.network`

change the file to add "ClientIdentifier=mac".

```
[Match]
Name=end0

[Network]
DHCP=ipv4

[DHCPv4]
ClientIdentifier=mac
```

Reboot: 
`sudo systemctl restart systemd-networkd`

Verify there is only one ip address:networkctl status end0 --no-pager | sed -n '1,80p'
ip -4 addr show dev end0