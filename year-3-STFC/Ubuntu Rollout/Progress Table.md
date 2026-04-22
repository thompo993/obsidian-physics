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

| Digitiser ID                   | Display IP     | True IP       | software version | firmware version | Update Start | Update Finished | Date   |
| ------------------------------ | -------------- | ------------- | ---------------- | ---------------- | ------------ | --------------- | ------ |
| **LEGACY**                     |                |               |                  |                  |              |                 |        |
| 02:ab:ba:00:22:18              |                |               |                  |                  | y            | y               | 260226 |
| 02:ab:ba:00:22:19              |                |               |                  |                  | y            | y               | 260226 |
| 02:ab:ba:00:22:20              |                |               |                  |                  | y            | y               | 260226 |
| 02:ab:ba:00:22:21              |                |               |                  |                  | y            | y               | 260226 |
| 02:ab:ba:00:22:84              |                |               |                  |                  | y            | y               | 260226 |
| 02:ab:ba:00:22:85              |                |               |                  |                  | y            | y               | 260226 |
| 02:ab:ba:00:22:86              |                |               |                  |                  | y            | y               | 260226 |
| 02:ab:ba:00:22:87              |                |               |                  |                  | y            | y               | 260226 |
| **NEW ATTEMPTS - DIGITISER 1** |                |               |                  |                  |              |                 |        |
| 02:ab:ba:00:22:74              | 130.246.84.7   | 130.246.84.8* | 10.0.0.3         | 26.02.19.01      | y            |                 | 260421 |
| 02:ab:ba:00:22:75              | 130.246.84.123 | 130.246.84.10 | 10.0.0.3         | 26.02.19.01      | y            |                 | 260421 |
| 02:ab:ba:00:22:76              | 130.246.84.124 | 130.246.84.2* | 10.0.0.3         | 26.02.19.01      | y            |                 | 260421 |
| 02:ab:ba:00:22:77              | 130.246.84.122 | ????          | ???              | ???              | y            |                 | 260421 |
| **digitiser 2**                |                |               |                  |                  |              |                 |        |
| 02:ab:ba:00:22:18              | 130.246.84.4   | 130.246.84.4  | 10.0.0.3         | 26.02.19.01      | y            |                 | 260422 |
| 02:ab:ba:00:22:19              | 130.246.84.2   | 130.246.84.2  | 10.0.0.3         | 26.02.19.01      |              |                 | 260422 |
| 02:ab:ba:00:22:20              | 130.246.84.3   | 130.246.84.3  | 9.5.9.1          | 24.12.12.02      |              |                 | 260422 |
| 02:ab:ba:00:22:21              | 130.246.84.12  | 130.246.84.12 | 9.5.9.1          | 24.12.12.02      |              |                 | 260422 |
|                                |                |               |                  |                  |              |                 |        |
| **Digitiser 3**                |                |               |                  |                  |              |                 |        |
| 02:ab:ba:00:22:84              | 130.246.84.7   | 130.246.84.7  | **9.5.9.1**      | **24.12.12.02**  |              |                 |        |
| 02:ab:ba:00:22:85              | 130.246.84.20  | 130.246.84.20 | **9.5.9.1**      | **24.12.12.02**  |              |                 |        |
| 02:ab:ba:00:22:86              | 130.246.84.70  | 130.246.84.70 | ???              | ???              |              |                 |        |
| 02:ab:ba:00:22:87              | 130.246.84.37  | 130.246.84.37 | **9.5.9.1**      | **24.12.12.02**  |              |                 |        |
|                                |                |               |                  |                  |              |                 |        |
|                                |                |               |                  |                  |              |                 |        |
|                                |                |               |                  |                  |              |                 |        |
|                                |                |               |                  |                  |              |                 |        |
|                                |                |               |                  |                  |              |                 |        |
|                                |                |               |                  |                  |              |                 |        |
|                                |                |               |                  |                  |              |                 |        |
|                                |                |               |                  |                  |              |                 |        |
|                                |                |               |                  |                  |              |                 |        |
|                                |                |               |                  |                  |              |                 |        |

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
### Digitiser 10
- DAQ 4, accidently skipped and completed 
- TRUE IP is http://130.246.84.28/firmware
---




## other digitisers
### 02:ab:ba:00:22:7b
- unsure if firmware boot will work, attempted to load from server
- - **invalid firmware when booted from remote, proceeding with local downloaded firmware** 