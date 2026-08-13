---
tags:
  - meeting
created: 2026-07-10
---
# Links: 
[[super-musr]]
[[digitiser]]
[[260708 super-rt-r80 experiment]]
# Agenda: 
- How the delivery and testing is going 
 - How to benchmark the processes that need to run on server to know if that server can be NDX machine.
-  Discuss about upcoming DAE loading tests. (Dan nixon tests, compression etc)
-  Recap SuperMuSR installation timeline. (first muons Feb 2027).
-  September beam tests - Andrea come RAL?
-  Support and maintenance of system. NI knowledge transfer to ISIS DSG.
# Minutes:

## Delivery and testing
### Timescales 

| Cylce         | Start         | End           |
| ------------- | ------------- | ------------- |
| 2026/02       | cancelled     | cancelled     |
| 2026/03       | 2026-09-15    | 2026-10-23    |
| 2026/04       | 2026-11-17    | 2026-12-18    |
| 2026/05       | 2027-02-02    | 2027-03-25    |
| long shutdown | long shutdown | long shutdown |
| 2027/01       | 2027-11-16    |               |
- 4 days to test everything 
	- we are going to do PHS 
	- Pulsers 
- 2 staves fixed, switch modules on the same stave.
- does this need to be pre calibrated we need to assign have of modules to one each module 
- 3.5hrs per stave 
- for 32 modules this is 16 days work according to Andrea
- to run in parallel we need new chiller and maybe a new light box (can we fit two in the current one?
- everything is being made sept-nov 
- everything is nicely spread out
- 4 days of beamtime in total 
- need real muon data for PHS etc
-  dan estimates 4 staves per day on beam assuming they are fully calibrated
- delivery issues potentially with the short staves, high lead time $\to$ will not arrive in time 
- lots of uncertainty 
- dan to circulate plan when he knows it 


## How to benchmark the processes that need to run on server to know if that server can be NDX machine.
- tests on a server 
- for gain stabilization
- benchmark the amount of compute required for this 
- can this compute live on NDX? 
- haven't got round to it yet 
- look at this in September
- andrea says for this we need the final software. 
- feburary software version needs an updated, should be part of the system to control all the experiments 
- start work on this now. 
- need one stave to determine how much compute it uses up. 
- when we do a full set of channel fittings of the HV scan it takes on a standard i5 computer takes 20s. we should expect that this procedure fits 7x the number of spectra compare to what we need in real time. to fit the 32 channels we currently take 5-10s 
- this is an decrease in time from last discussion 
- dan states we need to meet requirements of an HV update every 5 or so minutes.
- if you can adjust every 5 minutes, it is difficult to stabilise a PID 
- if we want loop control that has a time constant of minutes, as it is easy to make it accidentally make it oscillatory. need to determine the timescale of temperature changes 
- could this be done during cooling tests
- Andrea thinks this is a complex issue 
- PI vs lookup table, PI is better choice? this is becuase as can account for age of SiPMs
- do we need a more powerful NDX machine do we go through dan Nixon or Freddie
- very definitely this is a set of control parameters, therefore the control group is the correct group to this. we don't want to go to a different group
- Andrea suggesting keeping the stave to develop C++ code for updating this to maximise speed and efficiency 
- **when Andrea is next here, we are going to setup a stave to test this**
- this experiment are on the timescales of day 

## Alberto update 
- modules and staves, 16 shipped 
- everything shipped by NI by the end of July
- this is plenty delivered ISIS will not catch up 
- last two DAQs have they been sent? NO, they are finalizing tests 

## support and maintenance
- NI will share a schematic of all of the components and associated documentation to assist with maintenance and support 
- Alberto will share this 
- this is what josh wants, and ideally in hand ASAP. 
- this is what R8 wants. 
- peirs and R8 want some test firmware. 
	- NI has this for the digitizer 
	- also testing firmware and python scripts, very comprehensive according to Andrea
	- ISIS needs a board that is used for this, NI will give for free.
	- how will this be shared? $\to$ sent in a format that can be downloaded, or renew git expiry. 
	- for dan only: i have the DAQ setup instructions noted, and a comprehensive guide for various bugs, but cannot hurt to have one from NI. 
- when is ISIS going to receive the documentation? 
	- 1 week for everything - Alberto 
	- Andrea says it will take him 1 hour, lol 
	- test firmware already on the new digitizers, Dave wants it in sci compiler 
	- Dave wants to be able to test a whole one, just for the sake of learning and understanding
	- Andrea says this takes half a day. we can call and show this!


## other

- fuse on one DAQ the digitizer says its over temperature check the fuse. 
- ben to check the fuse this afternoon. 
- dan nixon wants to share kafka load over different brokers, and different IPs










