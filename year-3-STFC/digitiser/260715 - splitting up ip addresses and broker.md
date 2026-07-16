---
tags:
  - note
  - daq121
  - kafka
created: 2026-07-15
---
# Links: 
[[digitiser]]
[[ubuntu]]

# Notes:

## split brokers and ip addresses
-  current mission is to split the broker IPs for all of the digitisers therefore out of the 32 digitisers, we have 11,11,10 to the following ip addresses: 
**Broker: 130.246.84.15 (and 130.246.84.17 and 130.246.84.19)**

## process 
- go onto daqserver 
- make 3 `sysconfig` files under super-rt experiment 
- make them send to the corresponding ip addresses seen above. 
- update `content.yaml` **completed until here**
- power cycle/reset all digitisers, might be worth power cycling and trying to get as many digitisers as possible 
- verify that they are all online and do various DHCP and putty investigation 
- follow up with Anthony 