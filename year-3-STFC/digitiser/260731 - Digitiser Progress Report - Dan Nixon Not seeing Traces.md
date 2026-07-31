---
tags:
  - note
  - daq121
  - super-musr
created: 2026-07-22
---
# Links: 
[[260708 super-rt-r80 experiment]]
# Notes:
## Problem
Dan Nixon is not seeing traces on his end.  He is only seeing `DAQ_ID: 33` sending any information

## Progress
- we have got `automate.py` to work and successfully start and stop all digitisers [[260429 - Emulation of digitisers]]
- All digitisers are collecting the correct file from the DAQ Sever `\\daqserver.isis.cclrc.ac.uk\daqserver$\new-ubuntu-24\netcfg`
	with the following parameters: 
	```
	"kafka": {
        "enabled": true,
        "brokers": "130.246.84.15:9092",
        "topic": "daq-trace",
        "acls": false,
        "username": "user",
        "password": "user",
        "properties": {
            "compression.type": "none"
        }
    },
    "kafka_events": {
        "enabled": true,
        "brokers": "130.246.84.15:9092",
        "topic": "daq-event"
	```
	and 
	```
	kafka": {
        "enabled": true,
        "brokers": "130.246.84.15:9092",
        "topic": "daq-trace",
        "acls": false,
        "username": "user",
        "password": "user",
        "properties": {
            "compression.type": "none"
        }
    },
    "kafka_events": {
        "enabled": true,
        "brokers": "130.246.84.15:9092",
        "topic": "daq-event"
	```

- On [Grafana](http://te7gull.te.rl.ac.uk:3000/d/c9529eb7-a525-4e95-9f61-f49adb2ebef5/dev-dash-dev-daq121-monitoring?orgId=1&from=now-6h&to=now&timezone=browser&var-Instrument=MUSR&refresh=30s), most digitisers have:
	- working timestamp 
	- working acquisition running 
	- working status packets 
	- average count rate is on for most digitiser

- Dave, Piers and I have been monitoring network on Wireshark, nothing showing up from IPs we think are working 
- All digitisers are labelled and given `DAQ_ID` between 0 and 127.
	despite the DAQ that Dan Nixon has seen as working having its `DAQ_ID` changed, it is still only DAQ_ID 33 that is showing up
