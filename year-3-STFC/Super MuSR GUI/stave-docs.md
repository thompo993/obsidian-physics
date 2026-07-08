---
tags:
  - note
created: 2026-05-01
---
# Stave User Guide
# Tags
[[Super MuSR]]
[[GUI]]
## Sensor module 
The sensor map is the following
![Sensor Map](module-sensor-map.png)
Please note all sensor A,B,C are identical

** DO NOT POWER SENSOR MODULE A WITH HV HIGHER THAN 59.5V, OR SENSOR MODULE B/C HIGHER THAN 63V **


** SENSOR A HV MUST BE APPLIED ONLY WHEN THE STAVE LOW VOLTAGE IS ON **



## Channel mapping 
 
#### Channel mapping 
| DIGITIZER | LEFT | RIGHT | HV LEFT | HV RIGHT |
| --------- | ---- | ----- | ------- | -------- |
| 0         | 0    | 4     | 0*      | 4        |
| 1         | 1    | 5     | 1       | 5        |
| 2         | 2    | 6     | 2       | 6        |
| 3         | 3    | 7     | 3       | 7        |
| 4         | 8    | 12    | 8       | 12       |
| 5         | 9    | 13    | 9       | 13       |
| 6         | 10   | 14    | 10      | 14       |
| 7         | 11   | 15    | 11      | 15       |

 this number is plus 16 for section 1, plus 32 for section 2 and plus 48 for section 3

 #### Preamp mapping
| SECTION | LEFT | RIGHT |
| ------- | ---- | ----- |
| 0       | 1    | 0     |
| 1       | 3    | 2     |
| 2       | 5    | 4     |
| 3       | 7    | 6     |

## Stave mapping

While the connection to the digitizer is the following
![Stave Map](stavemap.png)
