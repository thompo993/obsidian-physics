
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

The way to get around this is as follows, it is because someone has logged into the digitiser before you. This is SSH telling you:

“I’ve connected This is SSH telling you: “I’ve connected to `130.246.84.70` before, and the server’s **host key** I’m seeing now is **different** from the one saved in your `known_hosts` file.” 

That can be totally normal (server rebuilt, OS reinstalled, SSH keys regenerated, IP now points to a different machine), **or** it can indicate a **man‑in‑the‑middle attack**. SSH blocks the connection when strict checking is on. we know it is not a man in the middle attack, as we are on ethernet and 