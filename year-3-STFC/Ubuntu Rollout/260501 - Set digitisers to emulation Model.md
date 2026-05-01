# Tags: 
[[digitiser]]
[[Super MuSR]]

# Procedure 

This document details how to setup any number of digitisers to emulation mode
## Prerequisites 

### On $daqserver
Inside of `\\daqserver.isis.cclrc.ac.uk\daqserver$\new-ubuntu-24\netcfg` There are two files that need to be updated: 
- #### `content`: 
	- This file contains the experiment name, the MAC address of the digitisers assigned to the experiment, and the channels for channel mapping. 
```
  super_rt:
    - mac: 02:ab:ba:00:22:18
      digitizer: 3
      daq : 0
      config: super_rt-1-1.json
      sysconfig : sysconfig.json
      firmware: fw
      channels : [64,65,66,67,68,69,70,71]

    - mac: 02:ab:ba:00:22:19
      digitizer: 3
      daq : 1
      config: super_rt-1-1.json
      sysconfig : sysconfig.json
      firmware: fw
      channels : [72,73,74,75,76,77,78,79]
    - mac: 02:ab:ba:00:22:20
      digitizer: 3
      daq : 2
      config: super_rt-1-1.json
      sysconfig : sysconfig.json
      firmware: fw
      channels : [80,81,82,83,84,85,86,87]
```
- #### `super_rt-1-1`:
	- This 
### Inside Codebase 


## Execution
