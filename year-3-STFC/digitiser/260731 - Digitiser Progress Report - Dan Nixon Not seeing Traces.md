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

- On Grafana, all digitisers have:
	- working timestamp 
	- working acquisiton ru

