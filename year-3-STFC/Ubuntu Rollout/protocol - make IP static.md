Enter the terminal, and enter:
```
sudo nano /etc/systemd/network/20-end0.network
```

Write the file as: 
```
[Match]
Name=end0

[Network]
DHCP=yes
DNSDefaultRoute=yes

[DHCP]
ClientIdentifier=mac
UseDNS=yes
UseDomains=yes
```


Reboot. 

Verify there is only one ip address:

```
networkctl status end0 --no-pager | sed -n '1,80p'
ip -4 addr show dev end0
```

