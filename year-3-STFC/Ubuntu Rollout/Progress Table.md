### Tags
[[Instructions]]
[[ubuntu]]
[[digitiser]]
[[firmware]]
[[Super MuSR]]

**NOTE: UPDATE FINISHED WHEN CORRECT FIRMWARE IS INSTALLED**, CURRENT INSTALLED VERSION - **26.02.19.01** 

### Coordinate convention for DAQ121 - 260424

![[fig-260421-ubuntu24-rollout-layout-for-digitsers.png.jpg]]

| 1                                                   . | .                                                   16 |
| ----------------------------------------------------- | ------------------------------------------------------ |
| 2                                                     | .                                                   17 |
| 3                                                     | .                                                   18 |
| 4                                                     | .                                                   19 |
| 5                                                     | .                                                   20 |
| 6                                                     | .                                                   21 |
| 7                                                     | .                                                   22 |
| 8                                                     | .                                                   23 |
| 9                                                     | .                                                   24 |
| 10                                                    | .                                                   25 |
| 11                                                    | .                                                   26 |
| 12                                                    | .                                                   27 |
| 13                                                    | .                                                   28 |
| 14                                                    | .                                                   29 |
| 15                                                    | .                                                   30 |
|                                                       |                                                        |

---


### Table

| Digitiser ID      | Display IP     | True IP       |     | Update Start | Update Finished | Date   |     |
| ----------------- | -------------- | ------------- | --- | ------------ | --------------- | ------ | --- |
| 02:ab:ba:00:22:18 |                |               |     | y            | y               | 260226 |     |
| 02:ab:ba:00:22:19 |                |               |     | y            | y               | 260226 |     |
| 02:ab:ba:00:22:20 |                |               |     | y            | y               | 260226 |     |
| 02:ab:ba:00:22:21 |                |               |     | y            | y               | 260226 |     |
| 02:ab:ba:00:22:84 |                |               |     | y            | y               | 260226 |     |
| 02:ab:ba:00:22:85 |                |               |     | y            | y               | 260226 |     |
| 02:ab:ba:00:22:86 |                |               |     | y            | y               | 260226 |     |
| 02:ab:ba:00:22:87 |                |               |     | y            | y               | 260226 |     |
| 02:ab:ba:00:22:74 | 130.246.84.7   | 130.246.84.8  |     | y            |                 | 260421 |     |
| 02:ab:ba:00:22:75 | 130.246.84.123 | 130.246.84.10 |     | y            |                 | 260421 |     |
| 02:ab:ba:00:22:76 | 130.246.84.124 | 130.246.84.2  |     | y            |                 | 260421 |     |
| 02:ab:ba:00:22:77 |                |               |     | y            |                 | 260421 |     |
| 02:ab:ba:00:22:7b | 130.246.84.6   | 130.246.84.6  |     | Y            |                 | 260422 |     |
| 02:ab:ba:00:22:18 | 130.246.84.4   | 130.246.84.4  |     |              |                 |        |     |
| 02:ab:ba:00:22:19 | 130.246.84.2   |               |     |              |                 |        |     |
| 02:ab:ba:00:22:20 | 130.246.84.    |               |     |              |                 |        |     |
| 02:ab:ba:00:22:21 |                |               |     |              |                 |        |     |
|                   |                |               |     |              |                 |        |     |

--- 

## TOP LEFT DIGITISER - DIGITISER 1 - pausing for future work
### 02:ab:ba:00:22:74 
- had to use PuTTY and wired protocol. ([[IP  Change contingecy]])
- problems with remote download. 
- switched to "local download" and downloaded the available firmware firmware version **26.02.19.01** 
### 02:ab:ba:00:22:75
- also had to use PuTTY
- unsure if firmware boot will work, attempted to load from server
- **invalid firmware when booted from remote, proceeding with local downloaded firmware**
### 02:ab:ba:00:22:76 
- multiple IP addresses? 
![[fig-260421-ubuntu24-rollout-double-ip-1-mac-add.png]]
### 02:ab:ba:00:22:77
- not booting from its known IP

--- 

## TOP LEFT DIGITISER - DIGITISER 2
### 02:ab:ba:00:22:18 
- password is not working, is this because its already updated? 
- tested two one that was updated, and one that we were unsure of updated state. the password didn't work for both, even though for one of the digitisers, it did work before the update.
- in terminal, we verified that the versions are the same 
![[fig-260421-ubuntu24-rollout-version-when-password-not-working.png]]
- however some do not have a working password and do not have the right firmware versions
### 02:ab:ba:00:22:19
- password did not work 

### 02:ab:ba:00:22:20
- password did not work 
### 02:ab:ba:00:22:21
- password did not work
- worried about man in middle attach, does not let me try and login
![[fig-260421-ubuntu24-rollou-man-in-middle.png]]

## TOP LEFT DIGITISER - DIGITISER 2




--- 

## other digitisers
### 02:ab:ba:00:22:7b
- unsure if firmware boot will work, attempted to load from server
- - **invalid firmware when booted from remote, proceeding with local downloaded firmware** 