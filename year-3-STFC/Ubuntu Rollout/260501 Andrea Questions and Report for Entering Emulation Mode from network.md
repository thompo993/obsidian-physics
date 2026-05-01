
# Questiosn:
# what to tell Andrea - notes:
- after ubuntu 24 update, when we use  `automate.py` we get conformation that we have sent the DAQ121 the updated settings, but when we enter the terminal the files have not been updated. 
## Example: 130.246.84.141   MAC: 02:ab:ba:00:22:53
### Sending Configuration to digitizer: 
- inside  `ips.yam1` we setup a single testing IP: 
```
available_ips:
  - 130.246.84.141        # MAC: 02:ab:ba:00:22:53
```
- Then inside the codebase, we enter 
```
PS C:\supermusr-gui-main> python.exe .\automate.py ips.yaml --configure "\\daqserver.isis.cclrc.ac.uk\daqserver$\super_rt\config\super_rt-1-1.json"
```
- The output in the terminal is as follows: 
```
Loaded IPs: ['130.246.84.141']
Connecting to 130.246.84.141
Unable to stop acquisition: {'code': -1, 'message': 'acquisition is not running'}
set_parameter called with cmd='dgtz.send_delay', value=5000, index=0
set_parameter called with cmd='dgtz.pre', value=0.1, index=0
set_parameter called with cmd='dgtz.post', value=24, index=0
set_parameter called with cmd='dgtz.trg_delay', value=0, index=0
set_parameter called with cmd='dgtz.lemo.mode', value=in_50, index=0
set_parameter called with cmd='dgtz.lemo.mode', value=in_50, index=1
set_parameter called with cmd='dgtz.lemo.source', value=t0_out, index=0
set_parameter called with cmd='dgtz.lemo.source', value=t0_out, index=1
set_parameter called with cmd='dgtz.sync.outmode', value=gnd, index=0
set_parameter called with cmd='dgtz.sync.outmode', value=gnd, index=1
set_parameter called with cmd='dgtz.sync.outmode', value=gnd, index=2
set_parameter called with cmd='dgtz.sync.outmode', value=gnd, index=3
set_parameter called with cmd='dgtz.sync.outmode', value=gnd, index=4
set_parameter called with cmd='dgtz.sync.outmode', value=gnd, index=5
set_parameter called with cmd='dgtz.sync.outmode', value=gnd, index=6
set_parameter called with cmd='dgtz.sync.outmode', value=gnd, index=7
set_parameter called with cmd='dgtz.emu.amp', value=500.0, index=0
set_parameter called with cmd='dgtz.emu.amp', value=600.0, index=1
set_parameter called with cmd='dgtz.emu.amp', value=700.0, index=2
set_parameter called with cmd='dgtz.emu.amp', value=800.0, index=3
set_parameter called with cmd='dgtz.emu.amp', value=900.0, index=4
set_parameter called with cmd='dgtz.emu.amp', value=1000.0, index=5
set_parameter called with cmd='dgtz.emu.amp', value=1100.0, index=6
set_parameter called with cmd='dgtz.emu.amp', value=1200.0, index=7
set_parameter called with cmd='dgtz.emu.delay', value=50, index=0
set_parameter called with cmd='dgtz.emu.delay', value=50, index=1
set_parameter called with cmd='dgtz.emu.delay', value=50, index=2
set_parameter called with cmd='dgtz.emu.delay', value=50, index=3
set_parameter called with cmd='dgtz.emu.delay', value=50, index=4
set_parameter called with cmd='dgtz.emu.delay', value=50, index=5
set_parameter called with cmd='dgtz.emu.delay', value=50, index=6
set_parameter called with cmd='dgtz.emu.delay', value=50, index=7
set_parameter called with cmd='dgtz.emu.noiseamp', value=0.0, index=0
set_parameter called with cmd='dgtz.emu.offset', value=2000.0, index=0
set_parameter called with cmd='dgtz.emu.n', value=4.0, index=0
set_parameter called with cmd='dgtz.emu.dn', value=20, index=0
set_parameter called with cmd='dgtz.emu.enable_pulse', value=true, index=0
set_parameter called with cmd='dgtz.emu.enable', value=true, index=0
set_parameter called with cmd='dgtz.emu.ch_map_mode', value=true, index=0
set_parameter called with cmd='dgtz.wavemode', value=analog, index=0
```

- As we can see, we have applied `cmd='dgtz.emu.enable_pulse', value=true, index=0`, `cmd='dgtz.emu.enable', value=true, index=0`, `cmd='dgtz.emu.ch_map_mode', value=true, index=0`. 
- This means that `automate.py` thinks we have sent the digitiser the updated command, therefore, if we `shh` into the terminal, on digitizer `130.246.84.141`

## Verifying Inside DAQ121
- firstly, we enter the web interface, and navigate to terminal. 
- Then we navigate to the terminal 
`root@nibuntu-arm:/ni/software/landapp# cd ../../../tmp/`
-  now we check inside the config files: 
`nano daqconfig.json`
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
                "dgtz.emu.amp:0": 1000,
                "dgtz.emu.amp:1": 1000,
                "dgtz.emu.amp:2": 1000,
                "dgtz.emu.amp:3": 1000,
                "dgtz.emu.amp:4": 1000,
                "dgtz.emu.amp:5": 1000,
                "dgtz.emu.amp:6": 1000,
                "dgtz.emu.amp:7": 1000
            },
            "dgtz.emu.period": 500,
            "dgtz.emu.noiseamp": 1000,
            "dgtz.emu.offset": 2000,
            "dgtz.emu.enable_pulse": "false",
            "dgtz.emu.enable": "false"
        },
"dgtz.wavemode": "analog"
},
```
- As we can see the `"dgtz.emu.enable_pulse": "false"` and `"dgtz.emu.enable": "false"`, not what we observed `automate.py` claiming to send to this. 
- When we boot `supermusrgui.py` and press start, we do not see the emulated traces, so this method does not work. 

## Workaround Method 
- as this method is not working, I have made a workaround that does work, but is more laborious and not the desired outcome. 
### Use `configure_multiple_digitizers.py` instead
- once again update `ips.yam1` to a single testing IP: 
```
available_ips:
  - 130.246.84.141        # MAC: 02:ab:ba:00:22:53
```
- The next step is to update the file that loads the default configuration inside of this GUI, this file is `parameters.json` inside this file we make the following changes:
#### original `parameters.json` File:
```
"emu": {
            "amp": {"command":"dgtz.emu.amp", "count":8, "value": 1000, "type": "float"},
            "delay": {"command":"dgtz.emu.delay",  "count":8, "value": 50, "type": "float"},
            "noiseamp": {"command":"dgtz.emu.noiseamp",  "value": 1000, "type": "float"},
            "offset": {"command":"dgtz.emu.offset",  "value": 2000, "type": "float"},
            "n": {"command":"dgtz.emu.n",  "value": 20, "type": "float"},
            "dn": {"command":"dgtz.emu.dn",  "value": 20, "type": "float"},
            "enable_pulse": {"command":"dgtz.emu.enable_pulse", "value": "false", "type": "list", "values": ["true", "false"]},
            "enable": {"command":"dgtz.emu.enable", "value": "false", "type": "list", "values": ["true", "false"]},
            "ch_map_mode": {"command":"dgtz.emu.ch_map_mode", "value": "false", "type": "list", "values": ["true", "false"]}
        },
```

#### Updated `parameters.json` File, for entering emulation mode:
```
"emu": {
            "amp": {"command":"dgtz.emu.amp", "count":8, "value": 1000, "type": "float"},
            "delay": {"command":"dgtz.emu.delay",  "count":8, "value": 50, "type": "float"},
            "noiseamp": {"command":"dgtz.emu.noiseamp",  "value": 1000, "type": "float"},
            "offset": {"command":"dgtz.emu.offset",  "value": 2000, "type": "float"},
            "n": {"command":"dgtz.emu.n",  "value": 20, "type": "float"},
            "dn": {"command":"dgtz.emu.dn",  "value": 20, "type": "float"},
            "enable_pulse": {"command":"dgtz.emu.enable_pulse", "value": "true", "type": "list", "values": ["true", "false"]},
            "enable": {"command":"dgtz.emu.enable", "value": "true", "type": "list", "values": ["true", "false"]},
            "ch_map_mode": {"command":"dgtz.emu.ch_map_mode", "value": "true", "type": "list", "values": ["true", "false"]}
        },
```

The only changes we have made are:
```
"enable_pulse": {"command":"dgtz.emu.enable_pulse", "value": "true", "type": "list", "values": ["true", "false"]},
            "enable": {"command":"dgtz.emu.enable", "value": "true", "type": "list", "values": ["true", "false"]},
            "ch_map_mode": {"command":"dgtz.emu.ch_map_mode", "value": "true", "type": "list", "values": ["true", "false"]}
        },
```

- Once this file has been updated boot the  `configure_multiple_digitizers.py` GUI: 
`PS C:\supermusr-gui-main> python .\configure_multiple_digitizers.py ` 
- this will launch the GUI, and from here 