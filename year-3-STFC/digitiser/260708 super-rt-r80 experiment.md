---
created: 2026-07-08
tags:
  - note
  - daq121
  - super-musr
---
# Links 
[[digitiser]]
[[ubuntu]]
## Notes
error with connecting to the digitisers on the web, this is for all digitisers 
`Error starting acquisition:  Resource temporarily unavailable` error for some DAQs, current DAQs that are not working: 
130.246.84.84 
130.246.84.134 

they get this error: 
```
Connecting to 130.246.84.52
Stop OK
Connecting to 130.246.84.53
Stop OK
Connecting to 130.246.84.111
Error stopping acquisition: Resource temporarily unavailable
Traceback (most recent call last):
  File "C:\supermusr-gui-main\automate.py", line 61, in cmd_stop
    sdk.execute_cmd("stop_acquisition")
  File "C:\supermusr-gui-main\adc120sdk.py", line 75, in execute_cmd
    response = self.socket.recv()
               ^^^^^^^^^^^^^^^^^^
  File "zmq\\backend\\cython\\socket.pyx", line 805, in zmq.backend.cython.socket.Socket.recv
  File "zmq\\backend\\cython\\socket.pyx", line 841, in zmq.backend.cython.socket.Socket.recv
  File "zmq\\backend\\cython\\socket.pyx", line 199, in zmq.backend.cython.socket._recv_copy
  File "zmq\\backend\\cython\\socket.pyx", line 194, in zmq.backend.cython.socket._recv_copy
  File "zmq\\backend\\cython\\checkrc.pxd", line 22, in zmq.backend.cython.checkrc._check_rc
zmq.error.Again: Resource temporarily unavailable
Connecting to 130.246.84.112
```

Error is much more widespread than i initially thought, i am going to power cycle all of them. 

### server  link
`\\daqserver.isis.cclrc.ac.uk\daqserver$\new-ubuntu-24` 
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


- [ ] ask Anthony to update the reserved ID addresses for the new digitisers. 




### daq onboard terminal details 

[[fig-260710-kafka-experiments-terminal-logs1.png]]![[fig-260710-kafka-experiments-terminal-logs1.png]]![[fig-260710-kafka-experiments-terminal-logs2.png]]
This suggests that 

also when trying to searh for files related to kafka: 
```
grep -rnw '.' -e 'kafka'
```

we get the following, then a bunch of invalid searches 
```
root@nibuntu-arm:/# grep -rnw '.' -e 'kafka'
./var/lib/dpkg/status-old:5812: More information about Apache Kafka can be found at http://kafka.apache.org/
./var/lib/dpkg/status-old:5836: More information about Apache Kafka can be found at http://kafka.apache.org/
./var/lib/dpkg/status-old:5860: More information about Apache Kafka can be found at http://kafka.apache.org/
./var/lib/dpkg/status:5812: More information about Apache Kafka can be found at http://kafka.apache.org/
./var/lib/dpkg/status:5836: More information about Apache Kafka can be found at http://kafka.apache.org/
./var/lib/dpkg/status:5860: More information about Apache Kafka can be found at http://kafka.apache.org/
./var/backups/dpkg.status.0:5812: More information about Apache Kafka can be found at http://kafka.apache.org/
./var/backups/dpkg.status.0:5836: More information about Apache Kafka can be found at http://kafka.apache.org/
./var/backups/dpkg.status.0:5860: More information about Apache Kafka can be found at http://kafka.apache.org/
./usr/include/librdkafka/rdkafkacpp.h:141: * @returns 0 if all kafka objects are now destroyed, or -1 if the
./usr/include/librdkafka/rdkafkacpp.h:552: * @brief Returns a human readable representation of a kafka error.
./usr/include/librdkafka/rdkafkacpp.h:1533:   * @brief Polls the provided kafka handle for events.
./usr/include/librdkafka/rdkafkacpp.h:3077:   * to consume messages from the local queue, each kafka message being
./usr/include/librdkafka/rdkafka.h:661: * @brief Returns a human readable representation of a kafka error.
./usr/include/librdkafka/rdkafka.h:3201: * @brief Polls the provided kafka handle for events.
./usr/include/librdkafka/rdkafka.h:3665:            *   kafka partition queue: oldest msg */
./usr/include/librdkafka/rdkafka.h:3667:        -1 /**< Start consuming from end of kafka                              \
./usr/include/librdkafka/rdkafka.h:3700: * to consume messages from the local queue, each kafka message being
./usr/include/librdkafka/rdkafka.h:5205: * @brief Adds one or more brokers to the kafka handle's list of initial
./usr/include/librdkafka/rdkafka.h:5263: *        internal kafka logging and debugging.
./usr/include/librdkafka/rdkafka.h:5360: * Returns 0 if all kafka objects are now destroyed, or -1 if the
./usr/include/librdkafka/rdkafka.h:7292: * @sa http://kafka.apache.org/documentation.html#topicconfigs
./usr/include/librdkafka/rdkafka.h:9533: * @param principal A principal, following the kafka specification.
```

