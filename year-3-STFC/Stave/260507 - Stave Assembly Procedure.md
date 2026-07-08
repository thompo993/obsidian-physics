---
tags:
  - note
created: 2026-05-07
---
# Tags 
[[Super MuSR]]
[[260222 - Beamline Production Stave Testing]]
[[Poster - Super MuSR Design, Stave Assembly, and Beamline Verification]]
[[Stave]]

# Key Results 


# Mechanics and Assembly 
## Mechanics
No defects were found as a part of the mechanics, everything fit to tolerance and we had no issues with shorting or fouling. However we did have to file some solder down on the PCB, but this was expected. 
![[fig-250506-nmsumsum-figs-exploded-stave.png]]
From Ascending to descending order we have:
- Cooling plate
- Stave PCB
- Module PCBs 
- Alignment Plate 
- Scintillating Tiles
- Compression plates
- Brass Degrader

Physical example of all of the mechanics except the scintillating tiles can be found below: 
![[fig-260507-nmsum-figs-stave mechanics no tiles.JPG]]
## Assembly 
**Note:** Before you start, make sure you get the following information
- the module PID 
- the tile IDs 
### Alignment Board and PCBs
To begin, we select the module and corresponding PCB. We have 3 differing lengths, known as A, B and C, in order of shortest to longest. We place the alignment dowls into the module PCBs:
![[fig-260507-production-stave-testing-alignment.jpg | 300]] ![[fig-260507-production-stave-testing-moda.jpg| 300]] 

We then insert the PCB into the aliment board and screw it into  into place, to the point that they are firmly in placed but not torqued up to the maximum as this could damage the PCB

### Cleaning 
We also ensure to clean all Tiles and SiPMs before insertion, as any dust or debris can hinder the performance of the detector: 
![[fig-260507-production-stave-testing-moda-aligment-pcb-assy-cleaning.jpg | 300]] ![[fig-260507-production-stave-testing-30mm-tile3.jpg | 300]] 
### Scintillating Tiles
The tile insertion procedure begins with selecting 8 tiles that are to be used in the module.  Apply a dab of optical grease such that if forms an ice cream cone like shape. on each stud of the tile. Then carefully insert into the alignment plate and onto the SiPM. 
![[fig-260507-nmsum-figs-moda_tile_arrangement.jpg | 300]] ![[fig-260507-nmsum-figs-mod_assembly.jpg | 300]]
Above we have an example of a fully completed module (left) and a the process of applying the tiles onto the SiPMs. 

### Compression and Wrapping
Once all of the tiles are in place a compression plate is laid over the top of the tiles and secured using aluminium tape, in this case 3M 425 Tape, as this has been confirmed to be pure enough where the proportion of magnetic contaminants within the tape have a negligible effect on MuSR.(3M 431 was also used for the test production staves, but has not been independently verified using a SQUID magnetometer).

![[fig-260507-production-stave-testing-30mm-tiles-compression-plate.jpg | 300]]![[fig-260507-production-stave-testing-tape-7.jpg | 300]]
We then make a cross with the tape, and then wrap around the circumference.  and fold it down to light tight the modules. Note that the corners need to be done with lots of care, this is because you can get extra layers, so you may need to cut some off (ensuring to keep the module light tight).


![[fig-260507-production-stave-testing-tape-5.jpg | 300]] ![[fig-260507-production-stave-testing-tape-2.jpg | 300]]

We then Label the Module with the tile arrangement, PID etc. ![[fig-260507-production-stave-testing-tape-labelled-3.jpg]]
We typically follow the following convention:
Convention:

\<NI PID\>\<MODULE TYPE\>\<TILE CONFIGURATION\>

So for example as per the image above, we have:
- NI PID: 20688 
- Module Type: A 
- Tile Configuration: 005
To make the following configuration code: 20688A005. 

### Full Stave Assembly 
After you have assembled four modules, we are going to put them into the stave. The stave PCB is screwed into the cooling plate, with a thermally conductive layer  over the PCB area. this is to ensure no shorts and sufficient cooling from the cooling pumps. 
After plugging in all of the modules, Light tight with tape and the result is a completed stave! 
![[fig-260507-nmsum-figs-full_stave.JPG]]

# Beamline Results 
