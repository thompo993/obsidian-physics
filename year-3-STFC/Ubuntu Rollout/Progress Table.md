### Tags
[[Instructions]]
[[ubuntu]]
[[digitiser]]
[[firmware]]
[[Super MuSR]]

### Notes: 
- **UPDATE FINISHED WHEN CORRECT FIRMWARE IS INSTALLED**, CURRENT INSTALLED VERSION - **26.02.19.01** 
- the first three digitisers are jumbled, and some have a double IP issue, we will return to these 3 later (digitisers 1,2 and 3)
#### Current protocol
- ALL STEPS UP TOO THE FIRMWARE UPDATE STEP ARE BEING TAKEN
- This is to: 
	- prevent dual IP error, seems to creep up on occasions when firmware is rebooted
	- no valid firmware bug, waiting on Andrea
### Coordinate convention for DAQ121 - 260424

![[fig-260421-ubuntu24-rollout-layout-for-digitsers.png.jpg]]

| 1                      RETURN                       . | .                                                   16 |
| ----------------------------------------------------- | ------------------------------------------------------ |
| 2                      RETURN                       . | .                                                   17 |
| 3                      RETURN                       . | .                                                   18 |
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

| Digitiser ID                           | Display IP     | True IP          | software version | firmware version | Update Start | Update Finished | Date   |
| -------------------------------------- | -------------- | ---------------- | ---------------- | ---------------- | ------------ | --------------- | ------ |
| **LEGACY**                             |                |                  |                  |                  |              |                 |        |
| 02:ab:ba:00:22:18                      |                |                  |                  |                  | y            | y               | 260226 |
| 02:ab:ba:00:22:19                      |                |                  |                  |                  | y            | y               | 260226 |
| 02:ab:ba:00:22:20                      |                |                  |                  |                  | y            | y               | 260226 |
| 02:ab:ba:00:22:21                      |                |                  |                  |                  | y            | y               | 260226 |
| 02:ab:ba:00:22:84                      |                |                  |                  |                  | y            | y               | 260226 |
| 02:ab:ba:00:22:85                      |                |                  |                  |                  | y            | y               | 260226 |
| 02:ab:ba:00:22:86                      |                |                  |                  |                  | y            | y               | 260226 |
| 02:ab:ba:00:22:87                      |                |                  |                  |                  | y            | y               | 260226 |
| **NEW ATTEMPTS - DIGITISER 1 SN15214** |                |                  |                  |                  |              |                 |        |
| 02:ab:ba:00:22:74                      | 130.246.84.7   | 130.246.84.8*    | 10.0.0.3         | 26.02.19.01      | y            |                 | 260421 |
| 02:ab:ba:00:22:75                      | 130.246.84.123 | 130.246.84.10    | 10.0.0.3         | 26.02.19.01      | y            |                 | 260421 |
| 02:ab:ba:00:22:76                      | 130.246.84.124 | 130.246.84.2*    | 10.0.0.3         | 26.02.19.01      | y            |                 | 260421 |
| 02:ab:ba:00:22:77                      | 130.246.84.122 | ????             | ???              | ???              | y            |                 | 260421 |
| **digitiser 2 - SN13965**              |                |                  |                  |                  |              |                 |        |
| 02:ab:ba:00:22:18                      | 130.246.84.4   | 130.246.84.4     | 10.0.0.3         | 26.02.19.01      | y            |                 | 260422 |
| 02:ab:ba:00:22:19                      | 130.246.84.2   | 130.246.84.2     | 10.0.0.3         | 26.02.19.01      | y            |                 | 260422 |
| 02:ab:ba:00:22:20                      | 130.246.84.3   | 130.246.84.3     | 9.5.9.1          | 24.12.12.02      | y            |                 | 260422 |
| 02:ab:ba:00:22:21                      | 130.246.84.12  | 130.246.84.12    | 9.5.9.1          | 24.12.12.02      | y            |                 | 260422 |
|                                        |                |                  |                  |                  |              |                 |        |
| **Digitiser 3 SN17117**                |                |                  |                  |                  |              |                 |        |
| 02:ab:ba:00:22:84                      | 130.246.84.7   | 130.246.84.7     | **9.5.9.1**      | **24.12.12.02**  | y            |                 |        |
| 02:ab:ba:00:22:85                      | 130.246.84.20  | 130.246.84.20    | **9.5.9.1**      | **24.12.12.02**  | y            |                 |        |
| 02:ab:ba:00:22:86                      | 130.246.84.70  | 130.246.84.70    | ???              | ???              | y            |                 |        |
| 02:ab:ba:00:22:87                      | 130.246.84.37  | 130.246.84.37    | **9.5.9.1**      | **24.12.12.02**  | y            |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
| **Digitiser 4 SN14966**                |                |                  |                  |                  |              |                 |        |
| 02:ab:ba:00:22:64                      | 130.246.84.62  | 130.246.84.62    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:65                      | 130.246.84.4   | 130.246.84.63/04 | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:66                      | 130.246.84.64  | 130.246.84.64    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:67                      | 130.246.84.65  | 130.246.84.65    | needs install    | needs install    |              |                 |        |
| **Digitiser 5 SN15256**                |                |                  |                  |                  |              |                 |        |
| 02:ab:ba:00:22:78                      | 130.246.84.66  | 130.246.84.66    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:79                      | 130.246.84.67  | 130.246.84.67    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:7a                      | 130.246.84.68  | 130.246.84.68    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:7b                      | 130.246.84.69  | 130.246.84.69    | 10.0.0.3         | 26.02.19.01      |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
| **Digitiser 6 SN14895**                |                |                  |                  |                  |              |                 |        |
| 02:ab:ba:00:22:48                      | 130.246.84.70  | 130.246.84.70    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:49                      | 130.246.84.71  | 130.246.84.71    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:4a                      | 130.246.84.72  | 130.246.84.72    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:4b                      | 130.246.84.73  | 130.246.84.73    | needs install    | needs install    |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
| **Digitiser 7 SN17155**                |                |                  |                  |                  |              |                 |        |
| 02:ab:ba:00:22:A8                      | 130.246.84.74  | 130.246.84.74    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:A9                      | 130.246.84.75  | 130.246.84.75    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:AA                      | 130.246.84.76  | 130.246.84.76    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:AB                      | 130.246.84.77  | 130.246.84.77    | needs install    | needs install    |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
| **Digitiser 8 SN17153**                |                |                  |                  |                  |              |                 |        |
| 02:ab:ba:00:22:A4                      | 130.246.84.78  | 130.246.84.78    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:A5                      | 130.246.84.79  | 130.246.84.79    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:A6                      | 130.246.84.80  | 130.246.84.80    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:A7                      | 130.246.84.81  | 130.246.84.81    | needs install    | needs install    |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
| **Digitiser 9 SN17143**                |                |                  |                  |                  |              |                 |        |
| 02:ab:ba:00:22:9C                      | 130.246.84.82  | 130.246.84.82    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:9D                      | 130.246.84.83  | 130.246.84.83    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:9E                      | 130.246.84.84  | 130.246.84.84    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:9F                      | 130.246.84.85  | 130.246.84.85    | needs install    | needs install    |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
| Digitiser 10 SN17148                   |                |                  |                  |                  |              |                 |        |
| 02:ab:ba:00:22:A0                      | 130.246.84.86  |                  | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:A1                      | 130.246.84.87  |                  | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:A2                      | 130.246.84.88  |                  | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:A3                      | 130.246.84.29  |                  | 9.5.9.1          | 24.12.12.02      |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
| Digitiser 11 SN17124                   |                |                  |                  |                  |              |                 |        |
| 02:ab:ba:00:22:98                      | 130.246.84.90  | 130.246.84.90    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:99                      | 130.246.84.91  | 130.246.84.91    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:9a                      | 130.246.84.92  | NOT WORKING      | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:9b                      | 130.246.84.93  | 130.246.84.93    | needs install    | needs install    |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
| Digitiser 12 SN17119                   |                |                  |                  |                  |              |                 |        |
| 02:ab:ba:00:22:8c                      | 130.246.84.94  | 130.246.84.94    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:9d                      | 130.246.84.95  | 130.246.84.95    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:9e                      | 130.246.84.96  | 130.246.84.96    | needs install    | needs install    |              |                 |        |
| 02:ab:ba:00:22:9f                      | 130.246.84.97  | 130.246.84.97    | needs install    | needs install    |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
| Digitiser 12 -  SN15212                |                |                  |                  |                  |              |                 |        |
| 02:ab:ba:00:22:70                      | 130.246.84.98  | 130.246.84.98    |                  |                  |              |                 |        |
| 02:ab:ba:00:22:71                      | 130.246.84.99  | 130.246.84.99    |                  |                  |              |                 |        |
| 02:ab:ba:00:22:72                      | 130.246.84.100 | 130.246.84.100   |                  |                  |              |                 |        |
| 02:ab:ba:00:22:73                      | 130.246.84.101 | 130.246.84.101   |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
| Digitiser 12 -  SN15203                |                |                  |                  |                  |              |                 |        |
| 02:ab:ba:00:22:6c                      | 130.246.84.102 | 130.246.84.102   |                  |                  |              |                 |        |
| 02:ab:ba:00:22:6d                      | 130.246.84.103 | 130.246.84.103   |                  |                  |              |                 |        |
| 02:ab:ba:00:22:6e                      | 130.246.84.104 | 130.246.84.104   |                  |                  |              |                 |        |
| 02:ab:ba:00:22:6f                      | 130.246.84.105 | 130.246.84.105   |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |
|                                        |                |                  |                  |                  |              |                 |        |

--- 

## TOP LEFT DIGITISER - DIGITISER 1 - pausing for future work
### 02:ab:ba:00:22:74 
- had to use PuTTY and wired protocol. ([[Contingency - IP  Change]])
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
- **THIS IS NOT THE CASE PASSOWRD AND UPDATES ARE INDEPENDENT**
### 02:ab:ba:00:22:19
- password did not work 
-  appears to be updated 

### 02:ab:ba:00:22:20
- password did not work 
- not updated 
### 02:ab:ba:00:22:21
- password did not work
- not updated
- worried about man in middle attack, does not let me try and login

#### Man in the Middle Example: 
![[fig-260421-ubuntu24-rollou-man-in-middle.png]]

--- 
## TOP LEFT DIGITISER - DIGITISER 3

terminal attempt for all 4 digitisers, either password doesn't work or they think there is a MIM attack,  **CHECK ALL FIRMWAREs**

```
PS C:\Users\fzy12567> ssh ubuntu@130.246.84.7
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ED25519 key sent by the remote host is
SHA256:5XW8LPuxco7/ExE84bIvoVcGe00ecx+53kmIqT5TMC0.
Please contact your system administrator.
Add correct host key in C:\\Users\\fzy12567/.ssh/known_hosts to get rid of this message.
Offending ED25519 key in C:\\Users\\fzy12567/.ssh/known_hosts:13
Host key for 130.246.84.7 has changed and you have requested strict checking.
Host key verification failed.
PS C:\Users\fzy12567> ssh ubuntu@130.246.84.20
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ED25519 key sent by the remote host is
SHA256:MkFUFRx5yu3bhMH9d6tuWAZPB7wvyRt8tdH4ARY/b/E.
Please contact your system administrator.
Add correct host key in C:\\Users\\fzy12567/.ssh/known_hosts to get rid of this message.
Offending ED25519 key in C:\\Users\\fzy12567/.ssh/known_hosts:6
Host key for 130.246.84.20 has changed and you have requested strict checking.
Host key verification failed.
PS C:\Users\fzy12567> ssh ubuntu@130.246.84.70
ubuntu@130.246.84.70's password:
Permission denied, please try again.
ubuntu@130.246.84.70's password:
Permission denied, please try again.
ubuntu@130.246.84.70's password:
PS C:\Users\fzy12567>
PS C:\Users\fzy12567> ssh ubuntu@130.246.84.37
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ED25519 key sent by the remote host is
SHA256:iBoYdKa50wM77/3zv2yiX41Fz0M9zm91HvzyWALHdJs.
Please contact your system administrator.
Add correct host key in C:\\Users\\fzy12567/.ssh/known_hosts to get rid of this message.
Offending ED25519 key in C:\\Users\\fzy12567/.ssh/known_hosts:12
Host key for 130.246.84.37 has changed and you have requested strict checking.
Host key verification failed.
PS C:\Users\fzy12567>
```


| 02:ab:ba:00:22:84 | 130.246.84.7  |
| ----------------- | ------------- |
| 02:ab:ba:00:22:85 | 130.246.84.20 |
| 02:ab:ba:00:22:86 | 130.246.84.70 |
| 02:ab:ba:00:22:87 | 130.246.84.37 |
### 02:ab:ba:00:22:84 

--- 

### Digitiser 4 
- DAQ 1 was fine, kept the IP 
- DAQ 2 was not fine (02:ab:ba:00:22:65)
	- double IP ISSUE AGAIN
![[fig-260421-ubuntu24-rollout-double-ip-1-mac-add-2.png]]


--- 

## digitiser 5: SN15256
- all seem to be going fine except DAQ4, which is not even letting me boot putty, or login
- had this issue earlier, troubleshoot later
- DAQ 4 was used for a random test earlier to see if all ips were static, so it was already updated. so its at the same step as the others

--- 

## digitiser 6 SN14895:

- DAQ 1 had the same error with the Man in the middle attack being suggested.  (see [[#Man in the Middle Example]]) 
- **FIX FOUND: [[Contingency - Remote Host Identification Has Changed]]**
- proceeded as normal with digitiser 6, DAQ1









--- 
### Digitiser 10
- DAQ 4, accidently skipped and completed 
	- **02abba0022a3** 
	- 
- TRUE IP is http://130.246.84.28/firmware
---


