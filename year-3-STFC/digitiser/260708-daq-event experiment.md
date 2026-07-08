---
created: 2026-07-08
tags:
  - note
d:
---
# Links 
[[digitiser]]
[[ubuntu]]

# Notes
## daq-event experiment

### Mission briefing: 
A bit later than intended as I ran into issues yesterday, but here are the kafka details for the SuperMuSR test setup.

Broker: 130.246.84.15 (and 130.246.84.17 and 130.246.84.19)
Trace topic: daq-trace
Event topic (if used): daq-event

I'm not sure if the config for the DAQ allows specifying more than one broker address, if not then just use the first one, if it does then use all three.

# First steps 
- some digitisers were not working these were commented out of `ips.yaml` 
- list of effected IPS:
```

```