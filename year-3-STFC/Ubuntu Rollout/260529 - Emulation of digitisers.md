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


#### Current Progress 
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

`super_rt-1-1.json` and `sysconfig.json` are in the same location as the channels, which we **know** have been updated, so why hav
were both set to be the same as hifi, which i have been told has worked before. however this did not seem to fix our issues. ls