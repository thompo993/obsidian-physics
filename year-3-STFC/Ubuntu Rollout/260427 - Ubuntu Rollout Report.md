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
`ClientIdentifier=mac` to the `.network` file.  Occasionally the file would be empty, but then just write the full config file into the empty file instead. 

## Full Update Protocol:
```
Important: NOTE DOWN THE IP ADDRESS. DISPLAY WILL NOT WORK IN RECOVERY

STEP 1 -- INSTALL RECOVERY IMAGE
--------------------------------------------------------------------------
Login in ssh in the old digitizer

remember. the username for old was ubuntu and password was temppwd

ssh ubuntu@<ip digizier>
enter password: temppwd

wget http://daqserver.isis.cclrc.ac.uk/new-ubuntu-24/upgrade-files/output/daq1x1-bootpart-update.sh -O daq1x1-bootpart-update.sh

chmod +x daq1x1-bootpart-update.sh
sudo ./daq1x1-bootpart-update.sh

STEP 2 -- REBOOT DIGITIZER IN RECOVERY
--------------------------------------------------------------------------
sudo mkdir -p /mnt/bootpartition
sudo mount /dev/mmcblk0p1 /mnt/bootpartition
sudo touch /mnt/bootpartition/recovery.txt
sudo umount /mnt/bootpartition/

sudo reboot

wait 3-4 minutes
with Chrome go to http://<ip digitizer>/osinstall
maybe its CRTL+SHIFT+R to reload the page if you already login in the web interface. Maybe chrome use the cached pages



***************
There is the possibily the digitizer take another ip (SHOULD NOT BUT POSSIBLE)
in that case you will not able to connect to the recovery interface. 
you must connect with the usb, locate the com port on your pc and the configuration is 115200 no parity 8,n,1
enter ip a to see the ip and log to the web interface
***************


under Network update paste the update url

http://daqserver.isis.cclrc.ac.uk/new-ubuntu-24/upgrade-files/isis-ubuntu-nsys-1.13.nsys

press Download and Install and when request red button INSTALL NOW
wait 3-4 minutes
wait the reboot 

STEP 3 -- CONFIGURE DIGITIZER TO BOOT USING THE REMOTE SERVER FW/CONFIG PROVIDER
--------------------------------------------------------------------------
with Chrome go to  http://<ip digitizer>/
login with password password
the system will say that no valid firmware is loaded, this is normal
go to Firmware Settings on the left bar
select remote server
enter url 

http://daqserver.isis.cclrc.ac.uk/new-ubuntu-24/netcfg/bootstrap.sh

verify and save

STEP 4 - REMOVE DOULBE IP ISSUE
--------------------------------------------------------------------------


	


than reboot the digitizer (left bar reboot button)
first boot from network will require 10 minutes in order to install all packages!
web interface will automatically reconnect as soon as the digitizer boot up

login and wait in the homepage until the read flag on the right side of the homepage goes away and the web interfaces says "CONNECTED".
That means that all packages have been install and digitizer is operating correctly
```
