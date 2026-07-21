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
- we usually see the sysconfig and daqconfig in this folder. 
- Inside the terminal, we get the following text: 
```
2026-07-17T13:42:51+00:00 nibuntu-arm landapp[426]: 2026/07/17 13:42:51 Device Model: DAQ121

2026-07-17T13:42:51+00:00 nibuntu-arm landapp[426]: 2026/07/17 13:42:51 NI Home: /ni

2026-07-17T13:42:51+00:00 nibuntu-arm landapp[426]: 2026/07/17 13:42:51 Firmware Path: /ni/firmware

2026-07-17T13:42:53+00:00 nibuntu-arm landapp[426]: 2026/07/17 13:42:53 HK poll error: failed to connect to bridge at tcp://localhost:5557: zmq4: could not dial to "tcp://localhost:5557" (retry=250ms): dial tcp 127.0.0.1:5557: connect: connection refused

2026-07-17T13:42:56+00:00 nibuntu-arm landapp[426]: 2026/07/17 13:42:56 HK poll error: failed to connect to bridge at tcp://localhost:5557: zmq4: could not dial to "tcp://localhost:5557" (retry=250ms): dial tcp 127.0.0.1:5557: connect: connection refused

2026-07-17T13:42:58+00:00 nibuntu-arm landapp[426]: 2026/07/17 13:42:58 HK poll error: failed to connect to bridge at tcp://localhost:5557: zmq4: could not dial to "tcp://localhost:5557" (retry=250ms): dial tcp 127.0.0.1:5557: connect: connection refused

2026-07-17T13:43:01+00:00 nibuntu-arm landapp[426]: 2026/07/17 13:43:01 HK poll error: failed to connect to bridge at tcp://localhost:5557: zmq4: could not dial to "tcp://localhost:5557" (retry=250ms): dial tcp 127.0.0.1:5557: connect: connection refused

2026-07-21T11:09:56+00:00 nibuntu-arm landapp[426]: 2026/07/21 11:09:56 Terminal WebSocket connection established

2026-07-21T11:09:56+00:00 nibuntu-arm landapp[426]: 2026/07/21 11:09:56 Terminal resized to 189x43

2026-07-21T11:15:11+00:00 nibuntu-arm landapp[426]: 2026/07/21 11:15:11 WebSocket read error: websocket: close 1005 (no status)
```

### AI Consultation 
I am not familiar with this kind of work, so asked co-pilot (GPT 5.6) for a a report on debugging steps [[260721 - Diagnosing Kafka copilot ideas]]

#### 4.4 software and firmware may be incompatible 
The DAQ management interface reports on a working digitiser: 
The DAQ management interface reports:
- **Software version:** `9.5.9.1`
- **Build date:** `Feb 19 2026 20:35:06`
- **Ethernet:** Up
- **IP address:** `130.246.53.161`
- **Fabric MAC:** `02abba002244`
- **EEPROM:** Valid
- **Landpage version:** `2026.2.19.1/2026.2.19.1`
- **FPGA firmware version:** `24.12.12.02`
- **FPGA firmware model:** `00.00.01.20`
- **Digitizer firmware version:** `24.12.12.02`
- **Digitizer model:** `00.00.01.20`
- **FPGA firmware status:** Loaded
- **Digitizer status:** Running
- **Clock source:** Internal, OK
- **FPGA temperature:** `48.2°C`
- **CPU core temperature:** `42.6 °C`
- **Rack temperature:** `38.8°C`

- we have a different firmware and software version so we want is 2026.02.22.1 
- but we cannot get this version, as when we reboot it doesn't work. 

