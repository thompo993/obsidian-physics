---
tags:
  - note
  - daq121
  - kafka
created: 2026-07-03
---
# Tags 
[[ubuntu]]
[[digitiser]]

# GRAFANA DASHBOARD FOR SUPER-RT
http://te7gull.te.rl.ac.uk:3000/d/c9529eb7-a525-4e95-9f61-f49adb2ebef5/dev-dash-dev-daq121-monitoring?orgId=1&from=now-1h&to=now&timezone=browser&var-Instrument=SUPER-RT&refresh=5s

# Notes
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

Error is much more widespread than i initially thought, 

The IPS are **missing** from Grafana, i think a power cycle may fix this. 
no duplicates, no dulpicate mac 
power cylce caused the 130.246.84.84 to come back online on grafana
does it now

It now works. 

# solution 
power cycle the DAQ that is at fault and esnure all ethernet cables are properly connected 