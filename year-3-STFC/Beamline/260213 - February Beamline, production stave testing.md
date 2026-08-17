---
tags:
  - note
created: 2026-08-17
---
# Links: 

# Notes:

## Preparation
- the tiles were arranged and labelled to give us a record of which tiles are in which location on the beamline. The naming convention is: 
#### Stave tile config ID naming convention

**_ _ _ _ _ A _ _ _**

"NI PID" "Module Type" "Tile Configuration"


For example, a module **C** with the stave NI PID **20692** and it is the 9th unique arrangement of tileswith this layout, the arrangement would be: 
							 **20692C009**

Here is an example setup of a stave: 
![[fig-260213-prd-stave-testing-2.jpg]]
With this tile arrangement, the correct labelling should be: 

| 210mm | 105mm | 63mm | 43mm | 30mm | 30mm | 30mm | 30mm |
| ----- | ----- | ---- | ---- | ---- | ---- | ---- | ---- |
| 1     | 2     | 14   | 17   | 42   | 62   | 84   | 93   |
| 2     | 3     | 15   | 18   | 52   | 70   | 85   | 94   |
| 3     | 4     | 117  | 19   | 54   | 76   | 89   | 98   |
| 4     | 6     | 118  | 22   | 60   | 83   | 91   | 99   |

For the correct layout, see the stave tracking spreadsheet in
```
"\\isis\Shares\MuonDevelopment\2025_BenThompson\Shared\Stave Testing\module_configuration_log.xlsx"
```
And the documentation of each run that was conducted over the course of the weekend is found here [[260213 - February Beamline, production stave testing]]


## Apply calibration tool
### Purpose
The purpose of the "apply calibration to Configuration" GUI is too allow the adjustment of HV values and the combining of configuration files (what is changed in the Multi-GUI, triggering, pulser, thresholds, etc) and combine it with a calibration file for a given stave setup (denoted by stave serial number)

### Booting
Load apply_calibration_gui.py using terminal or running normally. No extra arguments
### Usage
![[fig-260213-prd-stave-testing-3.png]]