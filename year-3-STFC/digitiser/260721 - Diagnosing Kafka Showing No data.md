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

#### Attempting to download bootstrap 
##### CLI command 
```
mkdir -p /tmp/daq-debug  

curl -v \  
--connect-timeout 10 \  
--max-time 30 \  
-o /tmp/daq-debug/bootstrap.sh \  
http://daqserver.isis.cclrc.ac.uk/new-ubuntu-24/netcfg/bootstrap.sh \  
2>&1 | tee /tmp/daq-debug/bootstrap-download.log  

echo "curl exit status: ${PIPESTATUS[0]}"
```

##### result 
```
root@nibuntu-arm:/# curl -v \4  --connect-timeout 10 \5  --max-time 30 \6  -o /tmp/daq-debug/bootstrap.sh \7  http://daqserver.isis.cclrc.ac.uk/new-ubuntu-24/netcfg/bootstrap.sh \8  2>&1 | tee /tmp/daq-debug/bootstrap-download.log
tee: /tmp/daq-debug/bootstrap-download.log: No such file or directory
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*   Trying 0.0.0.4:80...
  0     0    0     0    0     0      0      0 --:--:--  0:00:09 --:--:--     0* ipv4 connect timeout after 10000ms, move on!
* Failed to connect to 0.0.0.4 port 80 after 10002 ms: Timeout was reached
  0     0    0     0    0     0      0      0 --:--:--  0:00:10 --:--:--     0
* Closing connection
curl: (28) Failed to connect to 0.0.0.4 port 80 after 10002 ms: Timeout was reached
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*   Trying 0.0.0.5:80...
  0     0    0     0    0     0      0      0 --:--:--  0:00:09 --:--:--     0* ipv4 connect timeout after 10000ms, move on!
* Failed to connect to 0.0.0.5 port 80 after 10002 ms: Timeout was reached
  0     0    0     0    0     0      0      0 --:--:--  0:00:10 --:--:--     0
* Closing connection
curl: (28) Failed to connect to 0.0.0.5 port 80 after 10002 ms: Timeout was reached
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*   Trying 0.0.0.6:80...
  0     0    0     0    0     0      0      0 --:--:--  0:00:09 --:--:--     0* ipv4 connect timeout after 10000ms, move on!
* Failed to connect to 0.0.0.6 port 80 after 10002 ms: Timeout was reached
  0     0    0     0    0     0      0      0 --:--:--  0:00:10 --:--:--     0
* Closing connection
curl: (28) Failed to connect to 0.0.0.6 port 80 after 10002 ms: Timeout was reached
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*   Trying 0.0.0.7:80...
  0     0    0     0    0     0      0      0 --:--:--  0:00:09 --:--:--     0* ipv4 connect timeout after 10000ms, move on!
* Failed to connect to 0.0.0.7 port 80 after 10002 ms: Timeout was reached
  0     0    0     0    0     0      0      0 --:--:--  0:00:10 --:--:--     0
* Closing connection
curl: (28) Failed to connect to 0.0.0.7 port 80 after 10002 ms: Timeout was reached
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Host daqserver.isis.cclrc.ac.uk:80 was resolved.
* IPv6: (none)
* IPv4: 130.246.39.152
*   Trying 130.246.39.152:80...
* Connected to daqserver.isis.cclrc.ac.uk (130.246.39.152) port 80
> GET /new-ubuntu-24/netcfg/bootstrap.sh HTTP/1.1
> Host: daqserver.isis.cclrc.ac.uk
> User-Agent: curl/8.5.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Date: Tue, 21 Jul 2026 14:47:35 GMT
< Server: Apache/2.4.6 (Red Hat Enterprise Linux) OpenSSL/1.0.2k-fips mod_fcgid/2.3.9 PHP/5.4.16 SVN/1.7.14 mod_wsgi/3.4 Python/2.7.5
< Last-Modified: Sun, 22 Feb 2026 18:24:03 GMT
< ETag: "20d-64b6dc24e7402"
< Accept-Ranges: bytes
< Content-Length: 525
< Content-Type: application/x-sh
< 
{ [525 bytes data]
100   525  100   525    0     0  98167      0 --:--:-- --:--:-- --:--:--  102k
* Connection #4 to host daqserver.isis.cclrc.ac.uk left intact
#!/bin/sh

SERVER="http://daqserver.isis.cclrc.ac.uk/new-ubuntu-24/netcfg/"

# Download requirements.txt
wget -O /tmp/requirements.txt ${SERVER}requirements.txt

# Create virtual environment if it doesn't exist
if [ ! -d /ni/venv/bin/activate ]; then
    python3 -m venv /ni/venv
fi

# Activate virtual environment
. /ni/venv/bin/activate

# Install requirements
pip install -r /tmp/requirements.txt

# Download and run intelliboot
wget -O /tmp/intelliboot.py ${SERVER}intelliboot.py
cd /tmp/
python3 intelliboot.py "$SERVER"  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*   Trying 0.0.0.8:80...
  0     0    0     0    0     0      0      0 --:--:--  0:00:09 --:--:--     0* ipv4 connect timeout after 10000ms, move on!
* Failed to connect to 0.0.0.8 port 80 after 10002 ms: Timeout was reached
  0     0    0     0    0     0      0      0 --:--:--  0:00:10 --:--:--     0
* Closing connection
curl: (28) Failed to connect to 0.0.0.8 port 80 after 10002 ms: Timeout was reached
```


### Debug - 260722
the correct `content.yaml` file is  not correct. 
when this is updated and the broker IP was corrected, we have super RT back online

# Solution 
There was a typo in both the `content.yaml` and corresponding system configuration files were not correct.  we how have the fact that almost all of the daqs running an acquisition and now we are square on again, the data packets are not showing that they are streaming on grafana. [[260731 - Digitiser Progress Report - Dan Nixon Not seeing Traces]]


the network was not connected to  the same VLAN so i re routed this 
the IPS that aren't ticked off on the spreadsheet should work now

