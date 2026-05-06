# Tags: 
[[digitiser]]
[[Super MuSR]]

# procedure:
![[fig-260429-ubuntu-emulation-mapping-scope-example.png]]
We take the middle values for the time hist values from the scope, this is an example of ch0, of four separate digitisers. 
```
Loaded IP addresses from ips_multidaq.yaml
  DAQ 0: 130.246.84.90
  DAQ 1: 130.246.84.94
  DAQ 2: 130.246.84.98
  DAQ 3: 130.246.84.102
Connecting to DAQ 0 at IP 130.246.84.90...
DAQ 0 - sn: 0  section: 0
SW-VER: 9.5.9.1 (Feb 19 2026)  -- FPGA-VER: 605164034
Connecting to DAQ 1 at IP 130.246.84.94...
DAQ 1 - sn: 0  section: 0
SW-VER: 9.5.9.1 (Feb 19 2026)  -- FPGA-VER: 605164034
Connecting to DAQ 2 at IP 130.246.84.98...
DAQ 2 - sn: 0  section: 0
SW-VER: 9.5.9.1 (Feb 19 2026)  -- FPGA-VER: 605164034
Connecting to DAQ 3 at IP 130.246.84.102...
DAQ 3 - sn: 15057  section: 0
SW-VER: 9.5.9.1 (Feb 19 2026)  -- FPGA-VER: 605164034
```


Weirdly, 126 of the last section 0, is noted as being section 0, and has a unique ch value, but it is completely out of the expected values. see this image of all evidence of the mapping: 
- RHS terminal says all are section 0 (what we are aiming to do)
- LHS terminal shows that all have been started ok 
- top windows show that the mapping is correct, confirmed by the RHS
- LHS of the screen we see very weird scope values, not matching what we expect
[[fig-260429-ubuntu-emulation-mappin-weird values.png]]
![[fig-260429-ubuntu-emulation-mappin-weird values.png]]

## Results 
the results were interesting, using the whole dataset:
![[fig-260429-ubuntu-emulation-mappin-results-full.png]]
This gives us a clear section that suddenly becomes out of line, some key differences between these mappings: 
- these 5 data points were digitisers that were NOT originally on the `super-rt` list **most likely!**
- they occur after a gap in the data, caused by a digitiser not being on (in process of fixing this)

when we use data without the digitisers i added to super RT (double checked all values btw) we get a different result: 

[[fig-260429-ubuntu-emulation-mappin-results-cleaned.png]]
![[fig-260429-ubuntu-emulation-mappin-results-cleaned.png]]

[[fig-260429-ubuntu-emulation-mappin-results-total.png]]
![[fig-260429-ubuntu-emulation-mappin-results-total.png]]
