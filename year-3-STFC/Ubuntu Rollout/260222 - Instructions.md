### Tags: 
[[ubuntu]]
[[digitiser]]
[[firmware]]
[[Super MuSR]]
[[260429 - Progress Table]]

Important: NOTE DOWN THE IP ADDRESS. DISPLAY WILL NOT WORK IN RECOVERY
### STEP 1 -- INSTALL RECOVERY IMAGE
--------------------------------
Login in ssh in the old digitizer

remember. the username for old was ubuntu and password was temppwd

ssh ubuntu@ip\<digitizier\>
enter password: temppwd

wget http://daqserver.isis.cclrc.ac.uk/new-ubuntu-24/upgrade-files/output/daq1x1-bootpart-update.sh -O daq1x1-bootpart-update.sh
chmod +x daq1x1-bootpart-update.sh
sudo ./daq1x1-bootpart-update.sh

### STEP 2 -- REBOOT DIGITIZER IN RECOVERY
--------------------------------------
sudo mkdir -p /mnt/bootpartition
sudo mount /dev/mmcblk0p1 /mnt/bootpartition
sudo touch /mnt/bootpartition/recovery.txt
sudo umount /mnt/bootpartition/

sudo reboot
wait 3-4 minutes
with Chrome go to http:// \<ip digitizer\>/osinstall
maybe its CRTL+SHIFT+R to reload the page if you already login in the web interface. Maybe chrome use the cached pages



**There is the possibly the digitizer take another IP (SHOULD NOT BUT POSSIBLE)**
**in that case you will not able to connect to the recovery interface.** 
**you must connect with the USB, locate the com port on your pc and the configuration is 115200 no parity 8,n,1**
**enter IP a to see the IP and log to the web interface please see [[260429 - Contingency - IP  Change]]


under Network update paste the update url

http://daqserver.isis.cclrc.ac.uk/new-ubuntu-24/upgrade-files/isis-ubuntu-nsys-1.10.nsys

press Download and Install and when request red button INSTALL NOW
wait 3-4 minutes
wait the reboot 

STEP 3 -- CONFIGURE DIGITIZER TO BOOT USING THE REMOTE SERVER FW/CONFIG PROVIDER
--------------------------------------------------------------------------
with Chrome go to  http:// \<ip digitizer\>/
login with password password
the system will say that no valid firmware is loaded, this is normal
go to Firmware Settings on the left bar
select remote server
enter url http://daqserver.isis.cclrc.ac.uk/new-ubuntu-24/netcfg/bootstrap.sh
verify and save
than reboot the digitizer (left bar reboot button)
first boot from network will require 10 minutes in order to install all packages!
web interface will automatically reconnect as soon as the digitizer boot up

login and wait in the homepage until the read flag on the right side of the homepage goes away and the web interfaces says "CONNECTED".
That means that all packages have been install and digitizer is operating correctly

