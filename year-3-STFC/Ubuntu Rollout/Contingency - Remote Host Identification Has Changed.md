
When trying to `ssh ubuntu@<ip address>` you may be met with the following message: 

```
PS C:\Users\fzy12567> ssh ubuntu@<ip address>
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
Host key for <ip address> has changed and you have requested strict checking.
Host key verification failed.
```