# Tags
[[digitiser]]
[[ubuntu]]
[[Super MuSR]]
# Questions
- How does `channels` inside `content.yaml` match to what we should observe in the GUI?
# what to tell Andrea - notes:
- after ubuntu 24 update, when we use  `automate.py` we get conformation that we have sent the DAQ121 the updated settings, but when we enter the terminal the files have not been updated. 
# Update Procedure
- the update was done exactly as instructed by your documentation the following firmware and software was installed:
	- Software Version: **9.5.9.1** 
	- Firmware Version: **24.12.12.02** 
- we encountered an issue with getting two IP addresses assigned to one machine, and despite having reserved IPs, they did not remain fixed when they were either power cycled or rebooted. The issue was that MAC was not set as the `Client Identifier` so we made some changes to the network file. 
#### Changes made to the network file: 
`root@nibuntu-arm:/ni/software/landapp# sudo nano /etc/systemd/network/20-end0.network`

```
  GNU nano 7.2                                                                 /etc/systemd/network/20-end0.network                                                                      
[Match]
Name=end0

[Network]
DHCP=yes
DNSDefaultRoute=yes

[DHCP]
ClientIdentifier=mac
UseDNS=yes
UseDomains=yes
```

- `ClientIdentifier=mac` was added in, it was not included in the original `network/20-end0.network` file. 
- This was applied to all digitiser and resolved the double IP address issue and also made them fixed.
# Initial Setup
- full configured all files on `$daqserver` so that  `"pulse_enable":true` and `"enable":true`, for both `super_rt-1-1` ands `sysconfig`
	- `sysconfig`: 
```
                "width": 10,

                "offset": 50,

                "noise_amp":200,

                "pulse_enable":true,

                "enable":true
```
- `super_rt-1-1`
```
            "dgtz.emu.noiseamp": 0.0,

            "dgtz.emu.offset": 2000.0,

            "dgtz.emu.n": 4.0,

            "dgtz.emu.dn": 20,

            "dgtz.emu.enable_pulse": "true",

            "dgtz.emu.enable": "true",

            "dgtz.emu.ch_map_mode": "true"

        },
```

- `ips.yaml`, `isis.daq121.yaml` and `content.yaml` have been updated to contain all of the new IP fixed addresses in super-R80. Below are the headers of these files:
- for `ips.yaml`:
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
  - 130.246.84.62        # MAC: 02:ab:ba:00:22:64
  - 130.246.84.63        # MAC: 02:ab:ba:00:22:65
  - 130.246.84.64        # MAC: 02:ab:ba:00:22:66
  - 130.246.84.65        # MAC: 02:ab:ba:00:22:67
```
- for `isis.daq121.yaml`:
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
    digitizer_5:
      - 130.246.84.62        # MAC: 02:ab:ba:00:22:64
      - 130.246.84.63        # MAC: 02:ab:ba:00:22:65
      - 130.246.84.64        # MAC: 02:ab:ba:00:22:66
      - 130.246.84.65        # MAC: 02:ab:ba:00:22:67
```
- for `content.yaml`:
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

# What works: 
- After this, I remoted into a DAQ to verify that the correct settings had been updated onto the digitiser. For the DAQ with `mac: 02:ab:ba:00:22:37` and IP address `130.246.84.133`:
#### Inside the  web interface terminal:
`root@niubuntu-arm: nano ./ni/config/default.cfg `
```
  GNU nano 7.2                                                                             default.cfg                                                                                       
            "value": "8.2",
            "idx": 0
        }
    ],
    "emulator": {
        "ch": {
            "amp_0": 1000,
            "amp_1": 1000,
            "amp_2": 1000,
            "amp_3": 1000,
            "amp_4": 1000,
            "amp_5": 1000,
            "amp_6": 1000,
            "amp_7": 1000,
            "period_0": 10000,
            "period_1": 10000,
            "period_2": 10000,
            "period_3": 10000,
            "period_4": 10000,
            "period_5": 10000,
            "period_6": 10000,
            "period_7": 10000
        },
        "width": 10,
        "offset": 50,
        "noise_amp": 200,
        "pulse_enable": false,
        "enable": false
    },
    "channel_map": [
        536,
        537,
        538,
        539,
        540,
        541,
        542,
        543
    ]
}
```
- comparing this to what we expect in `content.yaml`:
```
    - mac: 02:ab:ba:00:22:37

      digitizer: 17

      daq : 3

      config: super_rt-1-1.json

      sysconfig : sysconfig.json

      firmware: fw

      channels : [536,537,538,539,540,541,542,543]
```
- So the channel mapping does successfully update, this is true for all digitisers. 
# What does not work:
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
- This means that `automate.py` thinks we have sent the digitiser the updated configuration, with emulation mode on. therefore, if we `shh` into the terminal, on digitizer `130.246.84.141` we should get matching configuration files.

## Verifying Inside DAQ121
- firstly, we enter the web interface, and navigate to terminal. 
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
- Also inside "`sys"
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