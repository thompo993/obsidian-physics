# Summary 
All Digitiser except the prototype digitiser in the R80 Server room have been updated to firmware version **24.12.12.02** and software version **9.5.9.1**. A fix was implemented to prevent double IP address issues from occurring in the future. 

# Double IP Address issue: 
## Cause: 
Due to an issue between Linux clients and Microsoft DHCP servers caused by Linux sending the DHCP server what's called a "Client ID" and not the MAC address, causing two IPs to be assigned to one machine. 

## Fix:
Needed to edit the `.network` file to make sure it was using MAC Address and not Client ID. The following changes were made:
```
sudo nano /etc/systemd/network/20-end0.network
```
And the file was changed from:
```
[Match]
Name=end0

[Network]
DHCP=yes
DNSDefaultRoute=yes

[DHCP]
UseDNS=yes
UseDomains=yes
```
To:
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
`ClientIdentifier=mac` to the `.network` file.  Occasionally the file would be empty, but then just write the full config file into the empty file and this works.