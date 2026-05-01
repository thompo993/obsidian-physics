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
inside the codebase, you need to setup two files: 
- #### `ips.yaml`: 
	- This is the file that contains all the IPs for each of the digitisers in `content.yaml` you should always assert that the number of IPs match the number of MAC addresses. 
	```
	available_ips:
  - 130.246.84.54        # MAC: 02:ab:ba:00:22:18
  - 130.246.84.55        # MAC: 02:ab:ba:00:22:19
  - 130.246.84.56        # MAC: 02:ab:ba:00:22:20
  - 130.246.84.57        # MAC: 02:ab:ba:00:22:21
  - 130.246.84.58        # MAC: 02:ab:ba:00:22:84
  - 130.246.84.59        # MAC: 02:ab:ba:00:22:85
  - 130.246.84.60        # MAC: 02:ab:ba:00:22:86
  - 130.246.84.61        # MAC: 02:ab:ba:00:22:87
	```
- #### `isis.daq121.yaml`:
	- This file I do not know for certain the function of, but I believe it is for mapping the `content.yaml` file with the corresponding MAC addresses (as it has the same hierarchy of experiment $\longrightarrow$ digitiser $\longrightarrow$ IP/MAC ). The structure of this file should look as follows:
	```  
  super_rt:
    digitizer_3:
      - 130.246.84.54   # MAC: 02:ab:ba:00:22:18
      - 130.246.84.55   # MAC: 02:ab:ba:00:22:19
      - 130.246.84.56   # MAC: 02:ab:ba:00:22:20
      - 130.246.84.57   # MAC: 02:ab:ba:00:22:21
    digitizer_4:
      - 130.246.84.58        # MAC: 02:ab:ba:00:22:84
      - 130.246.84.59        # MAC: 02:ab:ba:00:22:85
      - 130.246.84.60        # MAC: 02:ab:ba:00:22:86
      - 130.246.84.61        # MAC: 02:ab:ba:00:22:87
	```


## Execution
