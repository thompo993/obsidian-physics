The long stave was booted by navigating to the beamline gui files held within idaaas. then botted using the following command. 

```
(.venv) (base) [bt1163544@host-172-16-102-77 supermusr-gui-beamline-260224]$ python3 supermusrgui.py 130.246.54.172
[0.01930532 0.03861064 0.01930532] [ 1.         -1.50054289  0.57776417]
['130.246.55.79', '130.246.53.95', '130.246.55.111', '130.246.55.85']
Connecting to IP 130.246.54.172
sn: 0  section: 0
SW-VER: 9.5.9.1 (Feb 19 2026)  -- FPGA-VER: 605164034
QStandardPaths: XDG_RUNTIME_DIR not set, defaulting to '/tmp/runtime-bt1163544'
libGL error: glx: failed to create dri3 screen
libGL error: failed to load driver: virtio_gpu
(.venv) (base) [bt1163544@host-172-16-102-77 supermusr-gui-beamline-260224]$ python3 supermusrgui.py 130.246.54.172
[0.01930532 0.03861064 0.01930532] [ 1.         -1.50054289  0.57776417]
['130.246.55.79', '130.246.53.95', '130.246.55.111', '130.246.55.85']
Connecting to IP 130.246.54.172
sn: 0  section: 0
SW-VER: 9.5.9.1 (Feb 19 2026)  -- FPGA-VER: 605164034
QStandardPaths: XDG_RUNTIME_DIR not set, defaulting to '/tmp/runtime-bt1163544'
libGL error: glx: failed to create dri3 screen
libGL error: failed to load driver: virtio_gpu
```

Then the traces were confirmed to be running when the beamline was on.