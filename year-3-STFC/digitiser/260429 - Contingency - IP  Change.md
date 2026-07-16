---
tags:
  - note
  - daq121
created: 2026-04-29
---
### Tags
[[ubuntu]]
[[digitiser]]
[[firmware]]
[[super-musr]]
[[260429 - Progress Table]]

PASSWORD ONCE IP HAS SETUP 
USER: zynq 
PASSWORD: zynq
## Protocol
When you sudo reboot, this should be the terminal screen:
![[fig-260421-ubuntu24-rollout-terminal-post-sudo-rbt.png]]
however, this will not work when you enter the IP into google chrome as per [[260222 - Instructions]]. This means that the IP has likely changed, and this means the display IP is now wrong. 

Firstly, make sure you install PuTTY: 
https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

1) Connect your USB to Mini USB wire to the digitiser that is displaying the wrong IP.
2) Boot your "Device Manager"
	- navigate too "Ports"
	- Press Serial Port COM4 (if the COM4 is different I don't think this matters as its just the identifier, please check and google if it is different)
![[fig-260421-ubuntu24-rollout-device-manager.png]]
3) Choose the settings such that they are identical too the screenshot, the instructions say to set bits per second too 115200, but this can be set in PuTTY, so if it doesn't set correctly in the Device manager, just make sure to do it in PuTTY. 
![[fig-260421-ubuntu24-rollout-device-manager-2.png]]
4) Open PuTTY and and select the following settings - of course if your COM4 is COM1, ensure they match. Press Open!
![[fig-260421-ubuntu24-rollout-PuTTY-settings.png]]
5) enter "ip a" and this should be the output, as we can see the IP is visible as "130.246.84.8", which is different from our original IP, "130.246.84.7". 
![[fig-260421-ubuntu24-rollout-PuTTY-Terminal.png]]6) Proceed with the regular instructions, but using with Chrome go to:
http:// \< NEW ip digitizer\>/osinstall
