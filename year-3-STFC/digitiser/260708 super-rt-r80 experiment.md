---
created: 2026-07-08
tags:
  - note
---
# Links 
[[digitiser]]
[[ubuntu]]

# Notes
## daq-event experiment

### Mission briefing: 
A bit later than intended as I ran into issues yesterday, but here are the kafka details for the SuperMuSR test setup.

Broker: 130.246.84.15 (and 130.246.84.17 and 130.246.84.19)
Trace topic: daq-trace
Event topic (if used): daq-event

I'm not sure if the config for the DAQ allows specifying more than one broker address, if not then just use the first one, if it does then use all three.

# First steps 
- some digitisers were not working these were commented out of `ips.yaml` 
- list of effected IPS:
```
# - 130.246.84.148        # MAC: 02:ab:ba:00:22:4e
# - 130.246.84.115        # MAC: 02:ab:ba:00:22:69
# - 130.246.84.116        # MAC: 02:ab:ba:00:22:6a
# - 130.246.84.118        # MAC: 02:ab:ba:00:22:38
# - 130.246.84.122        # MAC: 02:ab:ba:00:22:26
# - 130.246.84.123        # MAC: 02:ab:ba:00:22:27
# - 130.246.84.124        # MAC: 02:ab:ba:00:22:28
```

- updated sysconfig file to take correct ip etc. 
- update content.yaml to say the correct config file. 
- updated daq.isiss


- the gui is showing events 
- inside the terminal the configuration files are correct
- Two most recent digitisers are not working, they dont seem to have the same `tmp` contents, suggests different software or firmware? probably need to ask Ni about this. 

--- 
## 2026-07-10 Progress 

- the datetime monitor are no working, we can see this is the case as it starts working yesterday after jack and dave updated the status packet stuff firmware etc 
![[fig-260710-kafka-experiments-datetime.png]]
- however, despite this the onboard and Grafana status packets monitors are stating that there are no packets  being sent out. **this is the current issue**
*![[fig-260710-kafka-experiments-status-packet-status.png]]*

![[fig-260710-kafka-experiments-onboard-status-packets.png]]

### current next steps ideas 
- message dan nixon asking if he sees events 
	- confirms if grafana/onboard status packet is not reading correctly 
	- unlikely but worth a try 
- dan and jack need to install UDP control to the board 
	- unsure of what this actually does
	- jack asks about forcing them to run, i think i can do this by setting tiles to emulation mode [[260501 - Set digitisers to emulation Mode]]


- [ ] ask anthony to update the reserved ID

