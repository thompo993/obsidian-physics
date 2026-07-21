---
tags:
  - note
  - daq121
  - super-musr
created: 2026-07-21
---
# Links: 
[[260708 super-rt-r80 experiment]]
# Notes:
## Problem
- When loading Grafana, we are seeing a "no data screen"
- This has started occurring after one of three things 
	- Jack and Dave installing some new things 
	- Anthony tuning on the server 
	- Freddie tuning the server
- What happened is that when i power cycled them to get a new `sysconfig.json` file, and they all effectively failed to get the firmware from the network, 
- I then installed a lot of the firmware manually, the all read as "connected" but when we load kafka we see no data. 
- In fact, it is not even showing as an experiment option, have to go through history to get it to work, had to use this [link](http://te7gull.te.rl.ac.uk:3000/d/c9529eb7-a525-4e95-9f61-f49adb2ebef5/dev-dash-dev-daq121-monitoring?orgId=1&from=now-6h&to=now&timezone=browser&var-Instrument=SUPER-RT&refresh=30s)
![[fig-260721-kafka-no-data-screenshot-of-grafana.png|300]]![[fig-260721-kafka-no-data-screenshot-of-daq.png |300]]

## Diagnosis: 
- Experiment is not visible on Grafana 
- DAQ is not taking the config from DAQSERVER. 
![[fig-260721-kafka-no-data-screenshot-of-daq-terminal-tmp-folder.png | 500]]
- This is concerning, when we try and reboot the firmware, we get an invalid firmware error. 
- Inside the terminal, we get the following text: 
- This is out of my leuge of knowledge 