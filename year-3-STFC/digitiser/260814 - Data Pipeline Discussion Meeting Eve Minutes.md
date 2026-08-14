---
tags:
  - note
  - meeting
  - super-musr
  - daq121
---

Ben Thompson
Dave Templeman
Daniel Pooley
Daniel Nixon
Eve Herschman

- Running with compression off
- One channel had fault, maybe a network part? Not receptive to SSP network
- 6.5 GB currently
- Suggested to turn compression on to run as a test
- Can test previous compression now
- 3 broker IPs ('leaders') put to the single broker
- If multiple brokers, higher capability - so try and get these working
- Writing as a list in JSON wouldn't work, but hopefully don't need to do that, can pass as a string
	- Ben can find the exact formatting for this
- Ben turns compression on, zstd
- Checking txrx (transmission rate) but doesn't seem to have updated
- SystemConfig may require a reboot, but would be impractical to do this for more than one
- Sending uncompressed data, says Dan Nixon
- Ben rebooting DAQ121 - 24 - 1 (97) to test compression settings
- Should add reboot all command
- May not recognise zstd
- Do have to compile library of compression settings before changing settings
- Dan previously tested this and Andrea compiled
- Ben tests 'gzip' on same DAQ121
- Multiple sysconfig files, so may be the problem with not updating compression
- This can be easily changed later, may be fine when DAQs are set up and is only a problem now as a result of cooling
- Rebooting testing DAQ
- zstd gave lowest compression 
- Compression now copies over as gzip in temp but gives very low TX
- Testing specific compression that Dan knows, lz4
- Dan did turn off the digitizer while onboard calibrating so may be off
- Dan shared screenshots from compression testing using htop (?)
- lz4 compression type is accepted
- Should be sending out traces
- Compression expected: at most half
- Taken the right sysconfig but doesn't appear to be running
- Seems to be correct on the GUI, so suggests this has worked
- Turning off system processes, and TX now works
- However, queue is full so not broadcasting, because of no reboot
- Internal address being used in apt-ubuntu, should address if specific packages there are needed
- btm in terminal shows CPU data
- DAQ121 97 looks compressed on Dan Nixon's end
- Dan Nixon asks for zstd as compression type, with software processes false
- All DAQs should be coming through, and were yesterday
- Ben confirms that compression works
- No defaulting back to uncompressed, just seems to not be doing anything
- zstd mainly causes queue full errors in Dan's experience
- Logs haven't been configured on digitisers yet
- last working zstd had sw processes on, with CPU ~50% (Dan)
- Checking MuSR DAQs, whose software and firmware versions match the Super MuSR ones
- Default config and sysconfig appear to be the same
- No difference in software, firmware, so no real reason for the file structure to be different, unless configs aren't copied over entirely
- New ones had config and syscongif in a file called temp, whereas that wasn't present in the MuSR ones
- If DAQ sever was off, DAQs wouldn't havegrabbed the firmware
- Switching from local to remote firmware
- Switching back to turning sw processes on with zstd (same as when it was working except that was for l4z), but this still appears not to be working
- Seems to be stalling the Kafka producer
- Emulation mode may have an effect? But unsure why
- Appears to have stopped working with reconfig here. Compression doesn't appear to have started
- Testing no compression after checking firmware settings, and turing sw processes on
- Copying MuSR firmware across to Super MuSR, but these were identical so had no effect
- Starting Kafka with no compression and starting a run: returns to normal operation
- No compressions aside from lz4 appeared to work (although this didn't work for very long) - unclear why this is the case (unless appropriate libraries were not available)
- ldd run on binary (on digitiser bridge in ni then software then bridge), resulting data sent to Dan Nixon:
```
root@nibuntu-arm:/ni/software/bridge# ldd bridge  
        linux-vdso.so.1 (0x0000ffffb7219000)  
        librdkafka.so.1 => /lib/aarch64-linux-gnu/librdkafka.so.1 (0x0000ffffb6ec0000)  
        libzmq.so.5 => /lib/aarch64-linux-gnu/libzmq.so.5 (0x0000ffffb6e10000)  
        libstdc++.so.6 => /lib/aarch64-linux-gnu/libstdc++.so.6 (0x0000ffffb6b80000)  
        libm.so.6 => /lib/aarch64-linux-gnu/libm.so.6 (0x0000ffffb6ad0000)  
        libgcc_s.so.1 => /lib/aarch64-linux-gnu/libgcc_s.so.1 (0x0000ffffb6a90000)  
        libc.so.6 => /lib/aarch64-linux-gnu/libc.so.6 (0x0000ffffb68d0000)  
        /lib/ld-linux-aarch64.so.1 (0x0000ffffb71dc000)  
        liblz4.so.1 => /lib/aarch64-linux-gnu/liblz4.so.1 (0x0000ffffb6890000)  
        libzstd.so.1 => /lib/aarch64-linux-gnu/libzstd.so.1 (0x0000ffffb67d0000)  
        libsasl2.so.2 => /lib/aarch64-linux-gnu/libsasl2.so.2 (0x0000ffffb6790000)  
        libssl.so.3 => /lib/aarch64-linux-gnu/libssl.so.3 (0x0000ffffb66c0000)  
        libcrypto.so.3 => /lib/aarch64-linux-gnu/libcrypto.so.3 (0x0000ffffb6240000)  
        libz.so.1 => /lib/aarch64-linux-gnu/libz.so.1 (0x0000ffffb6200000)  
        libbsd.so.0 => /lib/aarch64-linux-gnu/libbsd.so.0 (0x0000ffffb61c0000)  
        libsodium.so.23 => /lib/aarch64-linux-gnu/libsodium.so.23 (0x0000ffffb6160000)  
        libpgm-5.3.so.0 => /lib/aarch64-linux-gnu/libpgm-5.3.so.0 (0x0000ffffb60f0000)  
        libnorm.so.1 => /lib/aarch64-linux-gnu/libnorm.so.1 (0x0000ffffb5fc0000)  
        libgssapi_krb5.so.2 => /lib/aarch64-linux-gnu/libgssapi_krb5.so.2 (0x0000ffffb5f50000)  
        libmd.so.0 => /lib/aarch64-linux-gnu/libmd.so.0 (0x0000ffffb5f20000)  
        libkrb5.so.3 => /lib/aarch64-linux-gnu/libkrb5.so.3 (0x0000ffffb5e40000)  
        libk5crypto.so.3 => /lib/aarch64-linux-gnu/libk5crypto.so.3 (0x0000ffffb5df0000)  
        libcom_err.so.2 => /lib/aarch64-linux-gnu/libcom_err.so.2 (0x0000ffffb5dc0000)  
        libkrb5support.so.0 => /lib/aarch64-linux-gnu/libkrb5support.so.0 (0x0000ffffb5d90000)  
        libkeyutils.so.1 => /lib/aarch64-linux-gnu/libkeyutils.so.1 (0x0000ffffb5d60000)  
        libresolv.so.2 => /lib/aarch64-linux-gnu/libresolv.so.2 (0x0000ffffb5d30000)  
root@nibuntu-arm:/ni/software/bridge#
```
- This confirms that library is not missing
- Emulation mode turned off (just noise being transmitted) across all three configs, and sample run
- Compression reapplied (zstd), and DAQs restarted - TX back up, so tested on another
- CPU up, but TX only around 50 Mbps (may only be sending a subset of the data/may be compressing really well), but loads of Kafka errors produced
- 3 of the CPUs working hard, with one not pulling its weight
- <mark class="hltr-purple" style="--hltr-color: #D2B3FFA6;">Emulation mode is sabotaging data transmission</mark> - ask Andrea about this
- Next thing to do: test pipeline and try lz4 on all digitisers
<mark class="hltr-purple" style="--hltr-color: #D2B3FFA6;">- Could use ansable (?) on linux to manage all digitisers at once for things like updates, etc</mark>
- Plan
	- Back in emulation mode with limited length next week
	- Try with lz4 on all the DAQs for sampling noise
	- Dan Nixon could put through compression modes himself on his own payload

