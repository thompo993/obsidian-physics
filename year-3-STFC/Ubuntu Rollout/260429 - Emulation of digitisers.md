[[digitiser]]
[[Super MuSR]]
# Goal:
To setting up all of the digitisers such that from `multigui.py` it is possible to setup all square, emulated pulses at a sepcific point in time, current progress is that `content.yaml` is configured, and square waves have been observed in the `multigui.py` but the mapping is not correct.
[[fig-260429-ubuntu-emulation-prog1.png]]
![[fig-260429-ubuntu-emulation-prog1.png]]
- they are pulsing, at a trigger rate of 5hz (periodic trigger)
# Cause of error 
- mapping is not correct, so it is likely not pulling from the correct  `content.yaml` folder. However the `.yaml` file is showing the LSB range of the pulses is in the correct range, but just not the correct IP addresses we are expecting values of about 800, when we observe 500
	- unsure of how to select the correct experiment such that correct `.yaml` file is selected, this may not be coded into the `multigui.py` yet? 
		- changing the IPs that are selected changes the range, for:
```
daq_ips:

  # DAQ 0 - Master (controlla stave e base)

  daq0: "130.246.84.162"

  

  # DAQ 1 - Slave

  daq1: "130.246.84.163"

  # DAQ 2 - Slave

  daq2: "130.246.84.164"

  

  # DAQ 3 - Slave

  daq3: "130.246.84.165"
```

- the results in lsb range of the same shape, but instead at 
- there **is** a experiment tab in the regular GUI, but still need to figure it our 
[[fig-260429-ubuntu-emulation-prog2.png]]
![[fig-260429-ubuntu-emulation-prog2.png]]
As we can see here, the LSB of the peaks are much higher, around 3000  LSB, this is not we expect either as expected LSB for this rang is ~770.


## Discussion with Dan, steps to take 
- boot a GUI that i have not meddled with and see what happens when you press "start"
	- nothing happens 
- boot a GUI where i have not meddled and see what happens when you press "reload"
	- nothing happens 
- boot a GUI where i have not meddled and see what happens when you press "emu -> ch_map_mode=True "
	- it turns on, but we don't see any analogue pulses, this suggests that its me turning on the settings in the GUI that caused this effect.
- look into `boot all` scripts. in codebase
	- `Super MuSR Configuration Multi GUI` has alot of promise, seems to be the same 

- big list of ips found, could we not be working because this list is not updated?
	- update the super rt part of this file. 
	- create a copy called `isis.daq121-before-290426` so if it all goes wrong we can recover this. 
- check no duplicate IPs or mac adresses

**checking for the ‘NetBoot’ files on the DAQ itself, to see if the configuration is obvious**
- This is promising, we find a default config file inside "ni" 
[[fig-260429-ubuntu-emulation-default-conf-on-daq.png]]
![[fig-260429-ubuntu-emulation-default-conf-on-daq.png]]
We see in this folder a CHANNEL MAP AND OPTION FOR PULSERS, THIS IS IMPORTANT
[[fig-260429-ubuntu-emulation-default-conf-on-daq-2.png]]
![[fig-260429-ubuntu-emulation-default-conf-on-daq-2.png]]
This is for http://130.246.84.104/terminal, which according to its serial number of 02:ab:ba:00:22:6e, should have the following channel map:
![[fig-260429-ubuntu-emulation-default-conf-on-daq-check-chmap.png]]
**IT MATCHES!**

It seems that updating the `isis.daq121` and `content.yaml` has caused the digitiser to map properly. tomorrow, check for a few more random configurations, and then we can update this conf files so that it makes all of them pulse. (`"pulse_enable": false`)


### Double check

#### Mac Address and channel mapping
```
    - mac: 02:ab:ba:00:22:37

      digitizer: 17

      daq : 3

      config: super_rt-1-1.json

      sysconfig : sysconfig.json

      firmware: fw

      channels : [536,537,538,539,540,541,542,543]
```
#### IP Address and terminal 
```
    digitizer_17:

      - 130.246.84.130    # MAC: 02:ab:ba:00:22:34

      - 130.246.84.131    # MAC: 02:ab:ba:00:22:35

      - 130.246.84.132    # MAC: 02:ab:ba:00:22:36

      - 130.246.84.133    # MAC: 02:ab:ba:00:22:37
```

#### Inside the terminal, under default config
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

This file is located in `root@niubuntu-arm:/ni/config/default.cfg `

Now we just need to update this!

### This file has the correct channel mapping, how do i get this mode to enable on loading GUI/always be on??

When you apply a setting in "configure all digitisers" and you boot a digitiser, it gets overidden by booting the GUI, however pressing reload changes this, showing us that we are infact applying this setting. here is the question
- how do we observed if data is being produced without opening the GUI 
- why is reloading the hifi settings still not working. 
- Dan did not use this 
	- mention of a start all and stop all python files, i cannot see these anywhere. 


# Automatic Emulation Mode Current progress:

- Inside the terminal of the DAQ121s the channel mapping is the same as inside `content.yaml`. For example, inside the `content.yaml` file we have: 
```
    - mac: 02:ab:ba:00:22:37

      digitizer: 17

      daq : 3

      config: super_rt-1-1.json

      sysconfig : sysconfig.json

      firmware: fw

      channels : [536,537,538,539,540,541,542,543]
```

Then for the same mac address, inside the terminal on `default.cfg` the DAQ we get:

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

As we can see `"channel_map:"` is getting pulled from the sever and updated onto our digitisers. This is true for digitisers that I added to the `super_rt` list long after it was last used, so it is not old channel mapping causing this effect, it really has updated them for all digitisers. 

However, what we have not been able to do is get `"pulse_enable": false` or `enable": false` to be equal to `true`. I have tried updating  `super-rt-1-1`, and `sysconfig`. We know that this is what the DAQs pull from as inside `content.yaml` we see the following:
```
    - mac: 02:ab:ba:00:22:97

      digitizer: 31

      daq : 3

      config: super_rt-1-1.json

      sysconfig : sysconfig.json

      firmware: fw

      channels : [984,985,986,987,988,989,990,991]
```

`super_rt-1-1.json` and `sysconfig.json` are in the same location as the channels, which we **know** have been updated, so why have the configs not updated? 

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

## editing `default.conf` effect
- overwritten "`default.conf`" inside of OP `130.246.84.62`
- updating just one of these files does not help.
- when pressing `reload` on a file that never been tampered with, we extract "`ch_map_mode_on=true`"  This is interesting. . . 
- if this setting is restored, and set as the default conf, then why is the pulse and emu enabled mode staying as false?
- This must not be the cause or the file that is being used for setting up emulation mode

# `paramters.json` files
**IMPORTANT NOTE, THE PARAMATER FILES WERE SAWPPED BACK TO HOW THEY WERE ORGINALLY**
## Comparison Summary Table:
| Feature                   | `multidaq_gui.py`                                                      | `configure_multiple_digitizers.py` | `supermusrgui.py`                                                      |
| ------------------------- | ---------------------------------------------------------------------- | ---------------------------------- | ---------------------------------------------------------------------- |
| JSON Config File          | `parameters_multidaq.json`                                             | `parameters.json`                  | `parameters.json`                                                      |
| IP Configuration          | Hardcoded (4 fixed DAQs)                                               | `ips.yaml` (dynamic count)         | `ips.yaml` (single DAQ via CLI)                                        |
| Number of DAQs            | 4 (fixed)                                                              | Dynamic (from YAML)                | 1 (via CLI)                                                            |
| Parameter Tree            | Yes                                                                    | Yes                                | Yes                                                                    |
| Default Values Source     | JSON `"value"` field                                                   | JSON `"value"` field               | JSON `"value"` field                                                   |
| Real-time Acquisition     | Yes (3 threads/DAQ)                                                    | No                                 | Yes (3 threads/DAQ)                                                    |
| Data Plotting             | 4 tabs (Scope, A, Dark, T)                                             | Optional (commented out)           | 4 tabs (Scope, A, Dark, T)                                             |
| Housekeeping (HK)         | Yes (thread per DAQ)                                                   | No                                 | Yes (HK thread)                                                        |
| Hardware Query at Boot    | No                                                                     | Yes (section, SN, versions)        | Yes (section, SN, versions)                                            |
| Apply Button Behavior     | All 4 DAQs                                                             | All connected DAQs                 | Single DAQ                                                             |
| Configuration Commands    | `configure_dgtz`, `configure_base`, `configure_hv`, `configure_staves` | `configure_dgtz`, `configure_base` | `configure_dgtz`, `configure_base`, `configure_hv`, `configure_staves` |
| Master/Shared Param Logic | Yes                                                                    | No                                 | No                                                                     |
| Calibration Tab           | No                                                                     | No                                 | Yes (file or ethernet)                                                 |
| Save/Load Settings        | Yes                                                                    | Yes                                | Yes                                                                    |
| Channel Selection UI      | 8 checkboxes/DAQ (32 total)                                            | No                                 | No                                                                     |
| Status Bar Metrics        | Yes (rate, HV, busy %)                                                 | No                                 | Yes (rate, HV, busy %)                                                 |
| HV Control Button         | Yes (per DAQ)                                                          | No                                 | Yes (stave power)                                                      |
| Boot Parameter Groups     | `dgtz`, `in`, `trg`, `stave`, `base`, `mp`, `sw_process`               | All from `parameters.json`         | All from `parameters.json`                                             |
| Thread Management         | Multi-threaded (4×DAQ × 3 threads)                                     | Single-threaded (config only)      | Single-threaded (1 DAQ)                                                |

## Multi GUI
- it is discovered that. `multidaq_gui.py` pulls from a config file called `parameter_multidaq.json`, as a result the following code snippets were changed 

### parameter updates for emu mode - mutliGUI:
#### `parameters_multidaq.json`
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

        }
```
#### `parameters_multidaq-non-emu-mode-260501.json`.
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

#### summary 
As we can see above, we have overwritten the default values to enable emulation, ch mapping and the pulse,  and a non emu version of the parameters file was saved to `parameters_multidaq-non-emu-mode-260501.json`. 
#### workflow for multiGUI
Boot → Load `parameters_multidaq.json` 
     → Create 4 SDK instances (hardcoded IPs)
     → Initialize parameter tree with default values
     → User modifies GUI
     → Click "Apply" → `program_settings()`
     → Sends commands to ALL 4 DAQs
	 → Execute: `configure_dgtz, configure_base, configure_hv, configure_staves`
     → Click "Start" → Acquisition begins

## Single GUI
- this GUI pulls from `parameters.json` for emulation mode, the following code was changed inside this file 
### parameter updates for emu mode - `supermusrgui.py` and : `configure_multiple_digitizers.py`
#### `parameters.json`
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

#### `parameters-non-emu-mode-260501.json`
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

#### Summary 
As we can see above, we have overwritten the default values to enable emulation, ch mapping and the pulse,  and a non emu version of the parameters file was saved to `parameters-non-emu-mode-260501.json`. 


# varying different pulse height values in different locations to see if the mapping is correct. 
- as the values in time of the trace are not directly mapped numerically to the "`channels`" segment of `content.yaml`, we will investigate if channel mapping is true based on the amplitude of these signals 


# discussion with dan 
- we do **not** want to use the configure all to apply the settings, we want it to get it from the network boot. 
- although the way i have figured out above using configure all works, this is NOT what we want to do.  We want it too pull from the network. 
## Things too look into:
- why we do not have the correct updated config in the RAM of the
- what is the \_x that is appended to `.json` used for
	- **answered:** this for creating a copy 

# `automate.py`

This Python script is a **command-line automation tool** that connects to one or more digitizer devices over **TCP/IP** and executes commands such as starting/stopping acquisition, configuring hardware, resetting devices, and reading version info.

---

## Main Flow

### 1) Initialization
- Loads a YAML file (e.g. `ips.yaml`) that contains the **IP addresses** of the digitizer devices to control.

### 2) Command Processing
- Accepts a **command-line option** (for example `--start`, `--configure`, etc.).
- Some commands may also take **optional arguments** (such as a configuration file path).

### 3) Device Loop
- Iterates through each IP address from the YAML file.
- For each device:
  - Opens a TCP/IP connection
  - Executes the requested command
  - Moves to the next device

---

## Key Functions

### `cmd_configure(sdk, json_file)`
This is the main configuration entry point. It configures the digitizer by:

- Stopping any active acquisition
- Loading parameters from a JSON configuration file (via `load_json_and_process()`)
- Executing the configuration sequence, typically including:
  - `configure_dgtz`
  - `configure_base`
  - `configure_hv`
  - `configure_staves`

---

### `set_parameter(sdk, cmd, value, index)`
- Sends an individual parameter change to the device.
- Typically used internally by the JSON-processing/config pipeline to apply settings one at a time.

---

### `process_json()`
- Recursively parses the JSON configuration structure.
- Extracts **command/value pairs** (and related metadata like indices if applicable).
- Produces a set of device-setting actions that are later sent to the digitizer.

---

## Other Supported Commands / Features

In addition to configuration, the script supports other automation actions such as:

- `--start` / `--stop` acquisition
- `--reset` device state
- `--version` reporting
- Spectrum saving / data capture modes
- Emulation modes
- Conversion utilities

---
## example usage 
```
python automate.py ips.yaml --configure config.json
python automate.py --start
python automate.py --save_spectrum_amplitude output.txt
```


# Using `automate.py` to update the files 
we used the above  commands in the example usage, and unfortunately found the following: 
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
```
- so from this we can see that we have set: 
	```
	set_parameter called with cmd='dgtz.emu.enable_pulse', value=true, index=0
	set_parameter called with cmd='dgtz.emu.enable', value=true, index=0
	set_parameter called with cmd='dgtz.emu.ch_map_mode', value=true, index=0
	```
so our digitiser terminal should re registering this. Inside the command line, post reboot: 
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
```

even though it says its applied the settings, it has not updated, the file even says it was updated today, so we know something happened in the reboot, it just did not  take the settings we wanted 
when booting the GUI, this did not update either. 

# what to tell Andrea - notes:
See the following report:
[[260501 Andrea Questions and Report for Entering Emulation Mode from network]]



# fixed bug, wrong DAQ server i was putting the folders into 

**correct folder location is `\\daqserver.isis.cclrc.ac.uk\daqserver$\new-ubuntu-24\netcfg`**  
## to fix this:
- update `content.yaml` in correct folder location
- update the super rt files to be correct. 