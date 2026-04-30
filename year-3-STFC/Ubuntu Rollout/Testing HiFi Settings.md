## Testing HiFi Settings
To test if the config settings from HiFi (know to have worked) will make the GUI do channel mapped pulses, the exact settings were copied from `hifi` too `super_rt`. We then boot a random GUI to prevent interference from previous experimentation confounding the results. The IPs selected were:

```
daq_ips:

  # DAQ 0 - Master (controlla stave e base)

  daq0: "130.246.84.58"

  

  # DAQ 1 - Slave

  daq1: "130.246.84.59"

  # DAQ 2 - Slave

  daq2: "130.246.84.60"

  # DAQ 3 - Slave

  daq3: "130.246.84.61"
```

The GUI boots, and the following steps and results were observed: 
- Run was started $\longrightarrow$ we see nothing 
- Reload $\longrightarrow$ apply $\longrightarrow$ run started $\longrightarrow$ we see nothing 
 **trigger is changed to periodic, then above steps were retaken**
- we see electrical noise, but no emulated traces. 
- `emu.enable.pulse=true` $\longrightarrow$ `emu.enable=true` $\longrightarrow$ `emu_ch_map=true` $\longrightarrow$ We get emulated traces that are in order!
 [[fig-260429-ubuntu-emulation-sequential-ch-mappping-neg-pol.png]]
 ![[fig-260429-ubuntu-emulation-sequential-ch-mappping-neg-pol.png]]
 The expected channel mapping for these 32 channels are as follows, we are using digitiser 4, with the corresponding IPs and mac addresses: 
 ```
 digitizer_4:

      - 130.246.84.58        # MAC: 02:ab:ba:00:22:84

      - 130.246.84.59        # MAC: 02:ab:ba:00:22:85

      - 130.246.84.60        # MAC: 02:ab:ba:00:22:86

      - 130.246.84.61        # MAC: 02:ab:ba:00:22:87
 ```

these mac addresses correspond to the following channel mapping: 
```
- mac: 02:ab:ba:00:22:84

      digitizer: 4

      daq : 0

      config: super_rt-1-1.json

      sysconfig : sysconfig.json

      firmware: fw

      channels : [96,97,98,99,100,101,102,103]

  

    - mac: 02:ab:ba:00:22:85

      digitizer: 4

      daq : 1

      config: super_rt-1-1.json

      sysconfig : sysconfig.json

      firmware: fw

      channels : [104,105,106,107,108,109,110,111]

  

    - mac: 02:ab:ba:00:22:86

      digitizer: 4

      daq : 2

      config: super_rt-1-1.json

      sysconfig : sysconfig.json

      firmware: fw

      channels : [112,113,114,115,116,117,118,119]

  

    - mac: 02:ab:ba:00:22:87

      digitizer: 4

      daq : 3

      config: super_rt-1-1.json

      sysconfig : sysconfig.json

      firmware: fw

      channels : [120,121,122,123,124,125,126,127]
```

This does not match with our observed trace locations. However it is at least of merit that they are all sequential in our GUI.  As a result I am stuck and unsure of where to proceed next. I have a strange mix of the digitiser terminal seeming like it has read something from `$daqserver` but, not the configuration files, maybe it is pulling these settings from a different experiment on the server, for example one of the `supermusr_experiment` runs? 
