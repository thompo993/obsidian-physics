---
tags:
  - note
  - super-musr
  - beamline
created: 2025-11-14
---

# Key result
- As we can see here the overall are shape is comparable, and there is no major changes to tau, therefore we can consider the waveforms unaffected.
- **Therefore the ADA4937 Line driver chip is a best choice to reduce non-linearity in the 3500-4096LSB range, with no major change to the waveforms, and a visual improvement to the PHS.**

# Notes:
As mentioned in [[251009- NI October Beamline Visit]], there were issues regarding some upturn seen at the end of the LSB range on the line driver chips. Some tests were done with the picoquant laser to get a uniform source of light [[250805 - PicoQuant Laser Turn on Procedure]]


## with laser:
- Laser setup to have internal trigger on highest frequency
- Getting PHS with decent scan, 10m scans should suffice
- Unable to setup internal trigger using the Pico scope
![[fig-260806-onenote-transfer-line-driver-8.png]]
We could not get a suitable LSB range to see the upturn. Even increasing gain and therefore PE did not help. 

###  Swap in Line driver chips
Plan:
- Read NumPy binary traces to test the shape of the signals
- Detailed overnight PHS of a source over one of the channels
- Swap the line driver differential op amps.
14/11/25
- CHANGED UNIFIED CALIB FILE
- With the resistors with low values it can cause funny feedback loops and stuff, for current feedback ada4927

Obvious increase in the max LSB from changing gain of op amps:
- Changed unified Calib file offsets accordingly
- Two of the bias were too high, set to 58
- Reflections could be causing software to not work
- half the chips were swapped.

![[fig-260806-onenote-transfer-line-driver-10.png]]
![[fig-260806-onenote-transfer-line-driver-9.png]]
- we see a much higher increase in PE and peak, which is good. 
- the next step is to observe the waveform shape to see if the new ones distort it. 


### Old line drivers
![[fig-260806-onenote-transfer-line-driver-6.png]]

### ADA4937 Line Drivers![[fig-260806-onenote-transfer-line-driver-7.png]]

Threshold height from "wiggles" of the spectra suggest resistors are a bit low, which is expected with low value resistors and the current feedback amplifier.
Eric suggests that we do a small increase of about 50% increase in resistance of the relevant resistor components.
We don't see any significant change in wave shape that cannot be adjusted for. 

### Line driver tests, images
#### Evidence of upturn
![[fig-260806-onenote-transfer-line-driver-1.png]]

#### Layered comparison between line driver chips
![[fig-260806-onenote-transfer-line-driver-4.png]]
![[fig-260806-onenote-transfer-line-driver-5.png]]

#### Peculiar behaviour on one of the channels
The other candidate line driver chip was deemed unsuitable, the ADA4927, turns out it was a current feedback  Current Feedback Differential ADC Driver, so additional adjustments to the resistors and surrounding components is required. The traces were also odd and the offset functioned weirdly. Below is a plot of the offsets of all of the channels, and their associated fits, as we can see the ADA4930 and ADA437chips are similar and work well, which should be the case for offset. However the ADA4927 went AWRY, as seen below. 
![[fig-260806-onenote-transfer-line-driver-11.png]]
