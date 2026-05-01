# Tags: 
[[digitiser]]
[[Super MuSR]]

# Procedure 

This document details how to setup any number of digitisers to emulation mode
## Prerequisites 

### On $daqserver
Inside of `\\daqserver.isis.cclrc.ac.uk\daqserver$\new-ubuntu-24\netcfg` There are two files that need to be updated: 
- #### `content.yaml`: 
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
- #### `super_rt-1-1.json`:
	- This file is an example name of the general configuration you want to set all of your digitisers to. For example for the case of `super_rt-1-1` you navigate to the experiment folder of the same name and find the configuration file in there. 
	- This file is the same as any other configuration file you would load directly into the GUI, the difference is that this one will be automatically on the DAQ without any need to press "apply", or load the configuration in any other way. 
```
{
    "dgtz": {
        "dgtz.send_delay": 5000,
        "dgtz.pre": 0.1,
        "dgtz.post": 32,
        "dgtz.trg_delay": 0,
        "lemo_mode": {
            "dgtz.lemo.mode:0": "in_50",
            "dgtz.lemo.mode:1": "in_50"
        },
        "lemo_source": {
            "dgtz.lemo.source:0": "t0_out",
            "dgtz.lemo.source:1": "t0_out"
        },
        "sync_outmode": {
            "dgtz.sync.outmode:0": "gnd",
            "dgtz.sync.outmode:1": "gnd",
            "dgtz.sync.outmode:2": "gnd",
            "dgtz.sync.outmode:3": "gnd",
            "dgtz.sync.outmode:4": "gnd",
            "dgtz.sync.outmode:5": "gnd",
            "dgtz.sync.outmode:6": "gnd",
            "dgtz.sync.outmode:7": "gnd"
        },
        "emu": {
            "amp": {
                "dgtz.emu.amp:0": 1000.0,
                "dgtz.emu.amp:1": 1100.0,
                "dgtz.emu.amp:2": 1200.0,
                "dgtz.emu.amp:3": 1300.0,
                "dgtz.emu.amp:4": 1400.0,
                "dgtz.emu.amp:5": 1500.0,
                "dgtz.emu.amp:6": 1600.0,
                "dgtz.emu.amp:7": 1700.0
            },
### REST OF THIS CONFIGURATION FILE REMOVED FOR BREVITY ###
```

### Inside Codebase 


## Execution
