# Tags: 
[[digitiser]]
[[Super MuSR]]
# Dan Prompt
I can already see that that Andrea mapped all SuperRT DAQ’s to use the same config file! You can therefore edit (make a copy first) just one file and they will all take it.

Try and get them all into “Channel Mapping Emulation mode”. You will need to visually validate them with the multigui

we are using `super_rt-1-1.json`


we have 92 mac adressess already setup. 

- For some reasons IP address http://130.246.84.54/ is not working, so i will recovery boot and find out what is happening


# Bug
```
Connecting to 130.246.84.101
Error starting acquisition:  Resource temporarily unavailable 
```

```
Connecting to DAQ 0 at IP 130.246.84.98...
DAQ 0 is not running
DAQ 0 - sn: 0  section: 0
SW-VER: 9.5.9.1 (Feb 19 2026)  -- FPGA-VER: 605164034
Connecting to DAQ 1 at IP 130.246.84.99...
DAQ 1 is not running
DAQ 1 - sn: 15042  section: 1
SW-VER: 9.5.9.1 (Feb 19 2026)  -- FPGA-VER: 605164034
Connecting to DAQ 2 at IP 130.246.84.100...
DAQ 2 is not running
DAQ 2 - sn: 0  section: 2
SW-VER: 9.5.9.1 (Feb 19 2026)  -- FPGA-VER: 605164034
Connecting to DAQ 3 at IP 130.246.84.101...
DAQ 3 is not running
Traceback (most recent call last):
  File "C:\supermusr-gui-main\multidaq_gui.py", line 1300, in <module>
    s = sdk.get_parameter("dgtz.info.section")
        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\supermusr-gui-main\adc120sdk.py", line 60, in get_parameter
    self.socket.send(bytes(json.dumps(request), 'utf-8'))
  File "C:\Users\fzy12567\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\LocalCache\local-packages\Python311\site-packages\zmq\sugar\socket.py", line 696, in send
    return super().send(data, flags=flags, copy=copy, track=track)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "zmq\\backend\\cython\\socket.pyx", line 742, in zmq.backend.cython.socket.Socket.send
  File "zmq\\backend\\cython\\socket.pyx", line 789, in zmq.backend.cython.socket.Socket.send
  File "zmq\\backend\\cython\\socket.pyx", line 255, in zmq.backend.cython.socket._send_copy
  File "zmq\\backend\\cython\\socket.pyx", line 250, in zmq.backend.cython.socket._send_copy
  File "zmq\\backend\\cython\\checkrc.pxd", line 28, in zmq.backend.cython.checkrc._check_rc
zmq.error.ZMQError: Operation cannot be accomplished in current state
PS C:\supermusr-gui-main> 
```