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
**I am not familiar with this kind of work, so asked AI:**



find /ni -maxdepth 4 -type f \  

-printf '%TY-%Tm-%Td %TH:%TM:%TS %s %p\n' \  

2>/dev/null | sort  

```  

  

Record firmware checksums:  

  

```bash  

find /ni/firmware -maxdepth 3 -type f \  

-exec sha256sum {} \; \  

2>/dev/null  

```  

  

Run the same commands on a working DAQ121.  

  

Compare:  

  

- Directory structure  

- Firmware filenames  

- File sizes  

- Checksums  

- Symlink targets  

- File ownership  

- File permissions  

- Installed packages  

- Enabled systemd services  

- Configuration paths  

- Software versions  

- Firmware versions  

  

A direct comparison with a working DAQ121 is likely to identify the problem faster than repeatedly reinstalling firmware.  

  

---  

  

## Phase 2: Check TCP Port 5557  

  

Check whether anything is listening on the expected bridge port:  

  

```bash  

ss -lntp | grep ':5557'  

```  

  

An alternative command is:  

  

```bash  

lsof -nP -iTCP:5557 -sTCP:LISTEN  

```  

  

### Expected Result  

  

A bridge process should be listening on one of the following:  

  

```text  

127.0.0.1:5557  

```  

  

or:  

  

```text  

0.0.0.0:5557  

```  

  

### If There Is No Output  

  

List failed services:  

  

```bash  

systemctl --failed  

```  

  

Search for likely DAQ-related services:  

  

```bash  

systemctl list-units --type=service --all | \  

grep -Ei 'bridge|daq|kafka|land|packet|stream|zmq|firmware'  

```  

  

Search the service definitions and configuration for port `5557`:  

  

```bash  

grep -RIn \  

--exclude-dir=proc \  

--exclude-dir=sys \  

--exclude-dir=dev \  

'5557' \  

/etc /ni /opt /usr/lib/systemd /lib/systemd \  

2>/dev/null  

```  

  

This should identify:  

  

- The service that owns port `5557`  

- The service executable  

- The configuration file  

- The expected bind address  

- Any command-line arguments  

  

Inspect the relevant service:  

  

```bash  

systemctl status <bridge-service> --no-pager -l  

```  

  

Inspect detailed service state:  

  

```bash  

systemctl show <bridge-service> \  

-p ActiveState \  

-p SubState \  

-p Result \  

-p ExecMainStatus \  

-p ExecMainCode  

```  

  

Inspect recent service logs:  

  

```bash  

journalctl -u <bridge-service> -b --no-pager -n 300  

```  

  

Inspect warnings and errors from the current boot:  

  

```bash  

journalctl -b -p warning..alert --no-pager  

```  

  

Search the current boot logs for relevant terms:  

  

```bash  

journalctl -b --no-pager | \  

grep -Ei '5557|bridge|firmware|config|kafka|zmq|exception|failed|invalid'  

```  

  

### Result Interpretation  

  

#### Service does not exist  

  

The deployment may be incomplete. Check whether the bridge package or systemd unit was omitted during the manual installation.  

  

#### Service exists but is disabled  

  

The package may have been installed without enabling the service.  

  

Check:  

  

```bash  

systemctl is-enabled <bridge-service>  

```  

  

#### Service failed with a configuration error  

  

The missing or invalid `sysconfig.json` or `daqconfig.json` may be preventing startup.  

  

#### Service failed with a firmware or model error  

  

Compare the installed firmware bundle and metadata against a working DAQ121.  

  

#### Service is active but port 5557 is absent  

  

The process may be:  

  

- Listening on a different port  

- Listening on a different interface  

- Stuck before opening the socket  

- Running a child process that subsequently failed  

- Using a different configuration file than expected  

  

#### Service is repeatedly restarting  

  

Inspect the first startup error in the journal. Later messages may only report repeated restart attempts.  

  

---  

  

## Phase 3: Locate and Validate the Configuration Files  

  

Search for the expected configuration files:  

  

```bash  

find /ni /etc /var /tmp -type f \  

\( -name 'sysconfig.json' -o -name 'daqconfig.json' \) \  

-ls \  

2>/dev/null  

```  

  

Search the logs for configuration activity:  

  

```bash  

journalctl -b --no-pager | \  

grep -Ei 'sysconfig|daqconfig|DAQSERVER|download|HTTP|TLS|certificate|DNS'  

```  

  

Search files and service definitions for references to DAQSERVER and the JSON files:  

  

```bash  

grep -RIn \  

--exclude-dir=proc \  

--exclude-dir=sys \  

--exclude-dir=dev \  

'DAQSERVER\|sysconfig.json\|daqconfig.json' \  

/etc /ni /opt /usr/lib/systemd /lib/systemd \  

2>/dev/null  

```  

  

For every configuration file found, record:  

  

```bash  

stat /path/to/sysconfig.json  

stat /path/to/daqconfig.json  

```  

  

Inspect the beginning of each file:  

  

```bash  

head -n 30 /path/to/sysconfig.json  

head -n 30 /path/to/daqconfig.json  

```  

  

Validate the JSON syntax:  

  

```bash  

python3 -m json.tool /path/to/sysconfig.json >/dev/null  

```  

  

```bash  

python3 -m json.tool /path/to/daqconfig.json >/dev/null  

```  

  

Check whether the commands return successfully.  

  

A downloaded file may exist but still be unusable because the file is:  

  

- Empty  

- Truncated  

- Invalid JSON  

- An HTML error page  

- A proxy error response  

- A DAQSERVER error message  

- Configuration for the wrong hardware model  

- Configuration for the wrong instrument  

  

---  

  

## Phase 4: Test Access to DAQSERVER  

  

First identify the exact DAQSERVER hostname, endpoint, and port from:  

  

- Service definitions  

- Existing configuration  

- Startup scripts  

- Logs  

- A known-good DAQ121  

  

Test DNS resolution:  

  

```bash  

getent hosts <daqserver-hostname>  

```  

  

Check the route:  

  

```bash  

ip route get <daqserver-ip>  

```  

  

Test the destination port:  

  

```bash  

nc -vz <daqserver-hostname> <port>  

```  

  

Request the configuration endpoint:  

  

```bash  

curl -v --connect-timeout 5 '<exact-config-url>'  

```  

  

If the endpoint uses HTTPS:  

  

```bash  

curl -vk --connect-timeout 5 '<exact-config-url>'  

```  

  

Use `-k` only for diagnosis. Do not leave certificate verification disabled as a permanent fix.  

  

Check that:  

  

- DNS resolves the correct server.  

- The route uses the expected interface.  

- The default gateway is correct.  

- The destination port is reachable.  

- The endpoint returns HTTP `200`.  

- The endpoint does not return HTTP `404`, `500`, or a redirect.  

- The returned content is JSON.  

- The server recognises the device model.  

- The server recognises the device serial number.  

- The server recognises the MAC address.  

- The device is assigned to `SUPER-RT`.  

- The device clock is correct for TLS validation.  

  

Relevant device identity values are:  

  

```text  

Device model: DAQ121  

Serial number: 14949  

IP address: 130.246.84.155  

Fabric MAC: 02:ab:ba:00:22:59  

Instrument: SUPER-RT  

```  

  

### DAQSERVER-Side Investigation  

  

Inspect DAQSERVER logs around:  

  

```text  

17 July 2026 at approximately 13:42 UTC  

```  

  

Search for requests involving:  

  

```text  

130.246.84.155  

14949  

02:ab:ba:00:22:59  

SUPER-RT  

DAQ121  

```  

  

Check for:  

  

- HTTP 500 responses  

- Unknown device errors  

- Missing instrument assignments  

- Template-generation failures  

- Database connectivity errors  

- Permission errors  

- Invalid device model errors  

- Missing firmware manifest errors  

- Authentication failures  

- TLS errors  

- Incorrect environment selection  

  

If the reported `500` is an actual HTTP response, capture the associated DAQSERVER exception. The HTTP 500 could be the original cause of the missing configuration and all subsequent symptoms.  

  

---  

  

## Phase 5: Verify Firmware Compatibility  

  

Do not rely only on the management page showing **Loaded**.  

  

The **Loaded** status proves that the firmware was accepted far enough to run. It does not prove that the firmware exposes the interfaces expected by the local bridge.  

  

On the affected DAQ and a working DAQ121, collect:  

  

```bash  

find /ni/firmware -maxdepth 3 -type f \  

-exec sha256sum {} \;  

```  

  

```bash  

find /ni/software -maxdepth 4 -type f \  

-printf '%M %u:%g %s %p\n'  

```  

  

```bash  

systemctl list-unit-files | \  

grep -Ei 'bridge|daq|packet|stream|zmq'  

```  

  

Compare:  

  

- DAQ121 model identifier  

- FPGA firmware version  

- Digitizer firmware version  

- Firmware checksums  

- Firmware package filenames  

- Firmware manifest  

- Firmware signatures  

- `landapp` version  

- Bridge version  

- Service unit files  

- Service environment files  

- Symlink targets  

- Executable permissions  

- Configuration ownership  

- Configuration permissions  

  

Pay particular attention to whether the installed files belong to the correct user and group:  

  

```bash  

find /ni -maxdepth 4 -printf '%M %u:%g %p\n' 2>/dev/null  

```  

  

Check whether important executables retain execute permissions:  

  

```bash  

find /ni -maxdepth 4 -type f -perm /111 -ls 2>/dev/null  

```  

  

Do not continue applying different firmware packages until the approved version combination and checksums have been identified.  

  

---  

  

## Phase 6: Verify T0 and Local Packet Generation  

  

The management interface reports:  

  

```text  

T0 counter: 0  

T0 rate: 0.00 Hz  

```  

  

Determine whether T0 is expected for the current test state.  

  

Check:  

  

- Whether the T0 source is connected  

- Whether the expected clock source is internal or external  

- Whether the DAQ should generate housekeeping packets without T0  

- Whether event streaming requires a valid T0 signal  

- Whether configuration is required before T0 counters become active  

  

After correcting the bridge service, confirm that port `5557` is available:  

  

```bash  

ss -lntp | grep ':5557'  

```  

  

Monitor the relevant logs:  

  

```bash  

journalctl -f -u <bridge-service> -u landapp  

```  

  

The repeated `HK poll error` messages should stop after the bridge becomes available.  

  

Check network counters:  

  

```bash  

ip -s link  

sleep 10  

ip -s link  

```  

  

Look for increasing transmit and receive counters.  

  

If the packet destination is known, perform a short packet capture:  

  

```bash  

tcpdump -ni any host <destination-ip>  
```  
If the expected destination port is known:  
```bash  
tcpdump -ni any port <stream-port>  

```  

  

The purpose of the capture is to determine whether packets leave the DAQ. Avoid retaining payloads containing experiment data unless authorised.  

  

---  

  

## Phase 7: Verify Kafka Independently of Grafana  

  

Use a host with Kafka command-line tools. 
List broker and topic metadata: 
```bash  

kcat -b <broker-list> -L  

```  
Consume a small sample from the expected topic: 
```bash  
kcat \  
-b <broker-list> \  
-t <topic-name> \  
-C \  
-o -10 \  
-e \  
-c 10  

```  
Check:  
- Whether the expected topic exists  
- Whether the topic name changed
- Whether the DAQ is publishing to a development topic  
- Whether new offsets are being created  
- Whether messages contain the expected instrument name  
- Whether producer authentication is failing  
- Whether TLS settings changed  
- Whether Kafka ACLs changed  
- Whether broker addresses changed  
- Whether Schema Registry compatibility changed  
- Whether downstream consumers are rejecting messages 
- Whether consumer lag is increasing  
- Whether the instrument field changed in spelling, case, or formatting 
Compare:  
1. `SUPER-RT`  
2. A working instrument  
3. Historical `SUPER-RT` data from before 17 July 2026  
### Interpretation  
#### No DAQ messages in Kafka  
Continue investigating the DAQ, bridge, network publisher, producer configuration, authentication, and network path.  
#### Messages are present in Kafka  
Investigate the component that consumes Kafka data and writes to the monitoring database or Grafana data source.  
#### Messages exist but use a different instrument label  
Correct the DAQ configuration or update the relevant mapping.  
#### messages exist but timestamps are incorrect  
Check:  
```bash  
date -u  
timedatectl  
```  
Incorrect timestamps can place data outside the Grafana dashboard time range.  

---  
## Phase 8: Check the Downstream Consumer  
If Kafka contains current messages but Grafana still shows no data, inspect the consumer or ingestion layer.  
Check: 
- Consumer service state  
- Consumer group lag  
- Database connection  
- Schema errors  
- Deserialisation errors  
- Rejected messages  
- Timestamp conversions  
- Instrument-name mapping  
- Retention settings  
- Development versus production environment selection  
Example service checks:  
```bash  

systemctl --failed  

```  
```bash  

systemctl list-units --type=service --all | \  

grep -Ei 'consumer|ingest|kafka|metrics|monitor'  

```  
```bash  
journalctl -u <consumer-service> -b --no-pager -n 300  
```  
Search for relevant errors:  

```bash  

journalctl -b --no-pager | \  

grep -Ei 'kafka|consumer|schema|deserialize|SUPER-RT|database|timeout|failed'  

```  
---  
## Phase 9: Inspect Grafana  
Only investigate Grafana after confirming whether data reaches the configured data source.  
### Dashboard Variable 
The dashboard URL includes: 
```text  
var-Instrument=SUPER-RT  
```  
The fact that `SUPER-RT` must be forced through a historical URL suggests that the dashboard variable query is no longer returning the instrument.  
  
In Grafana:  
1. Open **Dashboard settings**.  
2. Open **Variables**.  
3. Select the `Instrument` variable.  
4. Inspect the variable query.  
5. Preview the returned values.  
6. Check which data source supplies the values.  
7. Determine whether only instruments with recent data are returned.  
8. Check the selected time range.  
9. Check whether `SUPER-RT` is stored under a different label.  
Possible alternative labels include:  
```text  
SUPER-RT  
SUPER_RT  
SUPER RT  
super-rt  
```  

  

### Panel Query  

  

For an affected panel:  

  

1. Open the panel menu.  

2. Select **Inspect**.  

3. Select **Query**.  

4. Review the request and response.  

5. Confirm the selected data source.  

6. Copy the query into **Explore**.  

7. Remove the instrument filter temporarily.  

8. Expand the time range.  

9. Compare the query against a working instrument.  

  

Check whether:  

  

- The query returns zero rows.  

- The query contains an incorrect instrument filter.  

- The data source changed during server tuning.  

- The dashboard points to a development database.  

- The timestamp is outside the selected range.  

- A dashboard transformation removes the returned data.  

- The variable contains an old cached value.  

  

### Grafana Data Source  

  

Use **Save & Test** for the configured data source.  

  

Check Grafana server logs while refreshing an affected panel:  

  

```bash  

journalctl -u grafana-server -f  

```  

  

If Grafana uses file-based logs:  

  

```bash  

tail -f /var/log/grafana/grafana.log  

```  

  

The Grafana dashboard loading successfully but returning **No data** makes a total Grafana outage unlikely.  

  

---  

  

# 8. Fast Diagnostic Decision Tree  

  

```text  

Does a process listen on 127.0.0.1:5557?  

│  

├── No  

│ │  

│ ├── Is the bridge service installed?  

│ │ ├── No  

│ │ │ └── The deployment or package installation is incomplete.  

│ │ └── Yes  

│ │  

│ ├── Is the service active?  

│ │ ├── No  

│ │ │ └── Inspect the journal for missing configuration,  

│ │ │ dependency, permission, firmware, or model errors.  

│ │ └── Yes  

│ │  

│ └── Check for:  

│ ├── Wrong bind address  

│ ├── Wrong port  

│ ├── Failed child process  

│ ├── Incorrect service configuration  

│ └── Software and firmware incompatibility  

│  

└── Yes  

│  

├── Do the landapp HK errors stop?  

│ ├── No  

│ │ └── Check the bridge protocol and version compatibility.  

│ └── Yes  

│  

├── Are packets leaving the DAQ?  

│ ├── No  

│ │ └── Check publisher destination, T0 and DAQ configuration.  

│ └── Yes  

│  

├── Do messages reach Kafka?  

│ ├── No  

│ │ └── Check routing, firewall, producer configuration,  

│ │ authentication, TLS and Kafka ACLs.  

│ └── Yes  

│  

├── Does the downstream store contain current records?  

│ ├── No  

│ │ └── Check the Kafka consumer, schema and ingestion service.  

│ └── Yes  

│  

└── Inspect Grafana variables, data source and panel queries.  

```  

  

---  

  

# 9. Immediate Next Actions  

  

The following commands should provide the highest-value information first:  

  

```bash  

ss -lntp | grep ':5557'  

```  

  

```bash  

systemctl --failed  

```  

  

```bash  

systemctl list-units --type=service --all | \  

grep -Ei 'bridge|daq|land|packet|stream|zmq|kafka'  

```  

  

```bash  

journalctl -b --no-pager | \  

grep -Ei '5557|sysconfig|daqconfig|firmware|invalid|failed'  

```  

  

```bash  

find /ni /etc /var /tmp -type f \  

\( -name 'sysconfig.json' -o -name 'daqconfig.json' \) \  

-ls \  

2>/dev/null  

```  

  

Then:  

  

1. Identify the process that should own port `5557`.  

2. Inspect the service logs for that process.  

3. Confirm whether `sysconfig.json` and `daqconfig.json` exist.  

4. Validate the configuration files.  

5. Test DAQSERVER directly from the DAQ.  

6. Check DAQSERVER logs for an HTTP 500 or unknown-device error.  

7. Compare the affected DAQ with a working DAQ121.  

8. Compare firmware and software checksums.  

9. Confirm packet transmission from the DAQ.  

10. Confirm current messages in Kafka.  

11. Inspect the downstream consumer.  

12. Inspect Grafana only after the upstream path has been verified.  

  

---  

  

# 10. Suggested Evidence Collection Template  

  

## DAQ Identity  

  

```text  

Instrument:  

Device model:  

Serial number:  

IP address:  

MAC address:  

Software version:  

Landpage version:  

FPGA version:  

Digitizer version:  

```  

  

## Configuration State  

  

```text  

sysconfig.json path:  

sysconfig.json modified time:  

sysconfig.json valid JSON:  

daqconfig.json path:  

daqconfig.json modified time:  

daqconfig.json valid JSON:  

DAQSERVER URL:  

DAQSERVER HTTP response:  

```  

  

## Bridge State  

  

```text  

Bridge service name:  

Installed:  

Enabled:  

Active:  

Service result:  

ExecMainStatus:  

Port 5557 listening:  

Bind address:  

First startup error:  

```  

  

## Packet State  

  

```text  

T0 counter:  

T0 rate:  

RX bandwidth:  

TX bandwidth:  

Destination IP:  

Destination port:  

Packets visible in tcpdump:  

```  

  

## Kafka State  

  

```text  

Broker:  

Topic:  

Latest offset:  

Latest message timestamp:  

Instrument value:  

Producer authentication errors:  

Consumer lag:  

```  

  

## Grafana State  

  

```text  

Data source:  

Instrument variable query:  

SUPER-RT returned by variable query:  

Panel query returns records:  

Latest record timestamp:  

Dashboard time range:  

```  

  

---  

  

# 11. Working Conclusion  

  

The pivotal technical issue is:  

  

```text  

No process appears to be accepting connections on  

tcp://127.0.0.1:5557  

```  

  

The Grafana **No data** screen and missing experiment option are likely downstream symptoms.  

  

The investigation should focus first on why the local bridge service is absent or failing. The most probable reasons are:  

  

- Missing DAQ configuration  

- DAQSERVER HTTP 500 or configuration-generation failure  

- Incomplete manual installation  

- Firmware/software compatibility mismatch  

- Incorrect permissions or paths  

- Missing service dependencies  

  

Once the bridge is listening on port `5557`, confirm that:  

  

1. `landapp` stops reporting `HK poll error`.  

2. DAQ network transmit counters increase.  

3. Kafka receives current messages.  

4. The downstream monitoring store receives current records.  

5. `SUPER-RT` reappears in the Grafana variable list.  

6. Grafana panels display current DAQ information.  

  

---  

  

# 12. Useful Links  

  

## Experiment Notes  

  

`260708 super-rt-r80 experiment`  

  

## Grafana Dashboard  

  

```text  

http://te7gull.te.rl.ac.uk:3000/d/c9529eb7-a525-4e95-9f61-f49adb2ebef5/dev-dash-dev-daq121-monitoring?orgId=1&from=now-6h&to=now&timezone=browser&var-Instrument=SUPER-RT&refresh=30s  

```  

  

## General Grafana Troubleshooting  

  

- [Grafana data-source troubleshooting](https://grafana.com/docs/grafana/latest/datasources/troubleshooting/)  

- [Grafana general troubleshooting](https://grafana.com/docs/grafana/latest/troubleshooting/)