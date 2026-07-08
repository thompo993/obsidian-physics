---
tags:
  - note
created: 2026-05-01
---
# SuperMUSR GUI
# Tags
[[Super MuSR]]
[[GUI]]
## Parameters

### Digitizer Section

#### Basic Timing Parameters

- **send_delay** [dgtz.send_delay]:
    Delay in microseconds to wait before sending a packet after the end of acquisition.
    Should be used because the status packet has a delay with respect to the trigger,
    and we can't send before receiving the status packet completely.

- **pre** [dgtz.pre]:
    Pre-trigger acquisition width in microseconds (should be less than 4us).
    Defines how much data is captured before the trigger signal.

- **post** [dgtz.post]:
    Post-trigger acquisition width in microseconds (should be less than 120us).
    Defines how much data is captured after the trigger signal.

- **trg_delay** [dgtz.trg_delay]:
    Delay in microseconds applied to the trigger signal.

- **wavemode** [dgtz.wavemode]:
    Waveform output mode. Options: "analog", "processed", "filtered".
    Selects which type of waveform data to output from the digitizer.

#### LEMO Connectors (DAQ)

- **lemo_mode** [dgtz.lemo.mode]:
    Mode for both LEMO connectors on the DAQ. Options:
    - "in_h": input with high impedance
    - "in_50": input with 50 ohm termination enabled
    - "out": output mode

- **lemo_source** [dgtz.lemo.source]:
    Signal function for the two LEMO connectors on the DAQ. Options:
    - "gnd": ground (logic 0)
    - "high": logic 1
    - "t0_out": T0 signal (replica of the T0)
    - "trigger_out": trigger from the self_le OR
    - "tot_chX": Time-Over-Threshold of the 8 channels
    - "run": logic 1 when digitizer is taking data
    - "busy": logic 1 when digitizer is busy transferring to client
    - "acquisition": logic 1 when acquisition is in progress (pre to post)
    - "mp_gate": logic 1 when multiphoton gate is open and multiphoton spectrum is accumulating

#### Sync Output Lines

- **sync_outmode** [dgtz.sync.outmode]:
    The DAQ has 8 output lines that can be routed to the BASE.
    This parameter selects which signal is routed to sync line X from DAQ to BASE. Options:
    - "gnd": ground (logic 0)
    - "high": logic 1
    - "t0_out": T0 signal (replica of the T0)
    - "trigger_out": trigger from the self_le OR
    - "tot_chX": Time-Over-Threshold of the 8 channels
    - "run": logic 1 when digitizer is taking data
    - "busy": logic 1 when digitizer is busy transferring to client
    - "acquisition": logic 1 when acquisition is in progress (pre to post)
    - "mp_gate": logic 1 when multiphoton gate is open and multiphoton spectrum is accumulating

#### Digitizer Section --> EMU (Emulator)

The emulator generates synthetic signals for testing purposes.

- **amp** [dgtz.emu.amp]:
    Amplitude of the emulated signal for each channel (8 channels). Unit: ADC counts.

- **delay** [dgtz.emu.delay]:
    Delay of the emulated signal for each channel (8 channels). Unit: nanoseconds.

- **noiseamp** [dgtz.emu.noiseamp]:
    Amplitude of noise added to the emulated signal. Unit: ADC counts.

- **offset** [dgtz.emu.offset]:
    Offset (baseline) of the emulated signal. Unit: ADC counts.

- **n** [dgtz.emu.n]:
    Number of peaks to generate in the spectrum.

- **dn** [dgtz.emu.dn]:
    Distance between peaks.

- **enable_pulse** [dgtz.emu.enable_pulse]:
    Enable pulse generation in emulator mode. Options: "true", "false".

- **enable** [dgtz.emu.enable]:
    Enable the emulator. Options: "true", "false".

- **ch_map_mode** [dgtz.emu.ch_map_mode]:
    Channel mapping mode for emulator. Options: "true", "false".
    With this option the emulator will put the peak delay and amplitude position in function of the channel number in the full detector system.

### Input Section

- **polarity** [in.polarity]:
    Input signal polarity. Options:
    - "pos": positive polarity
    - "neg": negative polarity (invert signal)

- **offset** [in.offset]:
    Input offset (baseline correction) for each of the 8 channels. Unit: ADC counts.
    this is a pure digital offset applied to the ADC samples. Does not affect the hardware offset of the stave. YOU MUST TRIM THE STAVE OFFSET TO AVOID ADC SATURATION.

### Trigger Section

- **mode** [trg.mode]:
    Trigger mode selection. Options:
    - "ext_trigger": external trigger input (trigger of the DAQ is from the base)
    - "self_le": self trigger on leading edge
    - "self_de": self trigger on dual edge
    - "periodic": periodic internal trigger
    - "manual": manual trigger (from software command)
    - "lemo_0": trigger from DAQ LEMO connector 0 

- **self_coinc** [trg.self_coinc]:
    Self-trigger coincidence mode. Options:
    - "or": trigger when any channel crosses threshold
    - "and2": trigger when at least 2 channels (adjacent 0-1 2-3) cross threshold
    - "and": trigger when all channels cross threshold

- **self_rate** [trg.self_rate]:
    Self-trigger rate in Hz (for periodic mode).

- **trigger_inib** [trg.trigger_inib]:
    Trigger inhibit time in ns. Prevents re-triggering within this time window.

- **threshold** [trg.threshold]:
    Trigger threshold for each of the 8 channels. Unit: ADC counts.

- **mask** [trg.mask]:
    Trigger mask for each of the 8 channels. 0 = channel enabled, 1 = channel disabled for trigger.

### Stave Section

The stave section controls the SiPM detector modules, including bias voltage, temperature control, offset compensation, and pulser settings.

#### BIAS Subsection

Controls the bias voltage for the SiPM detectors (64 channels total, organized in 2 groups).

- **enable** [stave.BIAS.enable]:
    Enable bias voltage supply for each of the 2 bias groups controlled by the stave. Options: "true", "false".
    This is the Digitizer main HV power supply, not the stave fine regulators (LDO).
    This is common to entire

- **V** [stave.BIAS.V]:
    Target bias voltage for each of the 2 bias groups. Unit: Volts.

- **max_v** [stave.BIAS.max_v]:
    Maximum allowed bias voltage for each of the 2 bias groups. Safety limit. Unit: Volts.

- **max_i** [stave.BIAS.max_i]:
    Maximum allowed bias current for each of the 2 bias groups. Safety limit. Unit: mA.

- **ldo_enable** [stave.BIAS.ldo_enable]:
    Enable LDO (Low Dropout regulator) for each of the 64 SiPM channels. Options: "true", "false".

- **correction_mode** [stave.BIAS.correction_mode]:
    Temperature correction mode for each of the 64 channels. Options:
    - "off": no temperature correction
    - "auto": automatic temperature correction (using temperature sensors and coefficient)
    - "manual": manual correction value

- **correction_manual** [stave.BIAS.correction_manual]:
    Manual temperature correction value for each of the 64 channels when correction_mode is "manual". Unit: Volts.

- **correction_coeff** [stave.BIAS.correction_coeff]:
    Temperature correction coefficient for each of the 64 channels. Unit: V/°C.

- **T0** [stave.T0]:
    Reference temperature for bias voltage correction. Unit: °C.

#### Temperature Control Subsection

- **target** [stave.T.target]:
    Target temperature for each of the 8 thermal zones. Unit: °C.

- **enable** [stave.T.enable]:
    Enable temperature control for each of the 8 thermal zones. Options: "true", "false".

- **PID.P** [stave.PID.P]:
    Proportional gain for PID temperature controller.

- **PID.I** [stave.PID.I]:
    Integral gain for PID temperature controller.

- **PID.D** [stave.PID.D]:
    Derivative gain for PID temperature controller.

- **SerpentineLSB** [stave.serpentine.lsb]:
    Serpentine heater LSB value for each of the 8 thermal zones. Controls heater power.

### Offset and Preamplifier

#### Channel mapping 
| DIGITIZER | LEFT | RIGHT | HV LEFT | HV RIGHT |
| --------- | ---- | ----- | ------- | -------- |
| 0         | 0    | 4     | 0*      | 4        |
| 1         | 1    | 5     | 1       | 5        |
| 2         | 2    | 6     | 2       | 6        |
| 3         | 3    | 7     | 3       | 7        |
| 4         | 8    | 12    | 8       | 12       |
| 5         | 9    | 13    | 9       | 13       |
| 6         | 10   | 14    | 10      | 14       |
| 7         | 11   | 15    | 11      | 15       |

 this number is plus 16 for section 1, plus 32 for section 2 and plus 48 for section 3

 #### Preamp mapping
| SECTION | LEFT | RIGHT |
| ------- | ---- | ----- |
| 0       | 1    | 0     |
| 1       | 3    | 2     |
| 2       | 5    | 4     |
| 3       | 7    | 6     |


- **offset** [stave.offset]:
    Baseline offset correction for each of the 32 channels. Unit: Volts.
    This is the hardware offset of the stave, and affects the real input signal level.

- **pre_enable** [stave.pre_enable]:
    Enable preamplifier for each of the 8 groups. Options: "true", "false".
    

#### Pulser Subsections (Pulser_0, Pulser_1, Pulser_2, Pulser_3)

Each stave has 4 independent pulsers for calibration. All pulsers share the same parameters:

- **enable_local_clock** [stave.pulserX.enable_local_clock]:
    Use local clock for pulser timing. Options: "true", "false".

- **enable_periodic** [stave.pulserX.enable_periodic]:
    Enable periodic pulser mode. Options: "true", "false".

- **enable_offspill** [stave.pulserX.enable_offspill]:
    Enable pulser during beam off-spill periods. Options: "true", "false".

- **enable_autocycle** [stave.pulserX.enable_autocycle]:
    Enable automatic cycling in offspill changing autamatically the pulser is on every off-spill gate signal toggle. Options: "true", "false".

- **enable_global_trigger** [stave.pulserX.enable_global_trigger]:
    Enable global trigger synchronization for pulser. This disable the periodic and, when enabled, drive directly the pulser led with the LEMO 0 of the stave. Options: "true", "false".

- **selected_pulser** [stave.pulserX.selected_pulser]:
    Select which pulser LSB configuration to use (0-15).

- **frequency** [stave.pulserX.frequency]:
    Pulser frequency. Unit: Hz.

- **width** [stave.pulserX.width]:
    Pulser pulse width. Unit: clock cycles (5ns per unit -> 0 means min width <1ns).

- **ch_enable** [stave.pulserX.ch_enable]:
    Enable pulser for each of the 16 channels in this pulser group. Options: "true", "false".

- **ch_ladder** [stave.pulserX.ch_ladder]:
    Ladder resistor selection 0, pulser not enabled, 7 maximum current.

- **ch_lsb** [stave.pulserX.ch_lsb]:
    Pulser amplitude in mV for each of the 16 channels. Controls pulse amplitude.

### Multiphoton (MP) Section

The multiphoton section handles the detection and measurement of multiphoton events.

- **spectrum_readout_mode** [mp.spectrum_readout_mode]:
    Spectrum readout mode. Options:
    - "manual": manual spectrum readout
    - "auto": automatic spectrum readout at regular intervals

- **spectrum_mode** [mp.spectrum_mode]:
    Spectrum acquisition mode. Options:
    - "charge_integrator": integrates charge over the gate window
    - "peak_holder": holds the peak value during the gate window
      *** UNUSED *** FIRMWARE AFTER 2025.x.x.x has only peak holder mode
    

- **auto_spectrum_time** [mp.auto_spectrum_time]:
    Time interval for automatic spectrum readout (when readout_mode is "auto"). Unit: milliseconds.

- **gate_len** [mp.gate_len]:
    Length of the integration gate for multiphoton detection. Unit: microseconds.
    *** UNUSED *** FIRMWARE AFTER 2025.x.x.x has only peak holder mode

- **delay_from_trigger** [mp.delay_from_trigger]:
    Delay from trigger to start of multiphoton gate. Unit: microseconds.
    *** UNUSED *** FIRMWARE AFTER 2025.x.x.x has only peak holder mode

- **trigger_inib** [mp.trigger_inib]:
    Trigger inhibit time for multiphoton events. Unit: microseconds.
    

- **trigger_delta** [mp.trigger_delta]:
    Trigger delta time parameter. Unit: microseconds.

- **bl_len** [mp.bl_len]:
    Baseline measurement length. Unit: microseconds.
    *** UNUSED *** FIRMWARE AFTER 2025.x.x.x has only peak holder mode

- **bl_hold** [mp.bl_hold]:
    Baseline hold time. Unit: microseconds.
    *** UNUSED *** FIRMWARE AFTER 2025.x.x.x has only peak holder mode

- **int_pre** [mp.int_pre]:
    Integration pre-gate time. Unit: microseconds.
    *** UNUSED *** FIRMWARE AFTER 2025.x.x.x has only peak holder mode

- **int_post** [mp.int_post]:
    Integration post-gate time. Unit: microseconds.
    *** UNUSED *** FIRMWARE AFTER 2025.x.x.x has only peak holder mode

- **peak_detector_window** [mp.peak_detector_window]:
    Peak detector window length. Unit is samples (1 sample = 4ns).

- **enable** [mp.enable]:
    Enable multiphoton detection for each of the 8 channels. Options: "true", "false".

- **gain** [mp.gain]:
    Gain setting for each of the 8 multiphoton channels.
    *** UNUSED *** FIRMWARE AFTER 2025.x.x.x has only peak holder mode

- **threshold** [mp.threshold]:
    Multiphoton detection threshold for each of the 8 channels. Unit: ADC counts.

- **offset** [mp.offset]:
    Multiphoton baseline offset for each of the 8 channels. Unit: ADC counts.

### Base Section

The base section controls the base board signals, clock distribution, T0 generation, and power.

#### LEMO Connectors (BASE)

- **lemo_mode** [base.lemo.mode]:
    Mode for each of the 16 LEMO connectors on the BASE. Options:
    - "in": input mode
    - "in_h": input with high impedance
    - "out": output mode

- **lemo_source** [base.lemo.source]:
    Signal source for each of the 16 LEMO connectors. Options include:
    - "gnd", "high": logic levels
    - "t0_out": T0 signal output
    - "common_trigger": common trigger signal (typically is the T0)
    - "clk_in": clock input
    - "busy": busy signal
    - "status_packet_int": status packet interrupt
    - "veto_int": veto interrupt
    - "frame_sp_rx": frame from status packet receiver (copy of the status packet)
    - "veto_usart_rx": veto from USART receiver (copy of the veto packet)
    - "pulse_gen": pulse generator output
    - "frame_usart_tx": frame USART transmit (emulator of status packet)
    - "veto_usart_tx": veto USART transmit (emulator of veto packet)
    - "lvds_x", "lvds_x+16": LVDS signals
    - "rj45_lvds_0/1/2/3": RJ45 LVDS signals
    - "adc_sync": ADC sync signal
    - "reg_x": register signals
    - "veto_x": veto signals
    - "sync_a/b/c/d_0-7": sync signals from groups A/B/C/D (from DAQ 8 sync lines)

- **sync_outmode** [dgtz.sync.outmode]:
    Routing configuration for the 8 sync output lines to the DAQ sync input. Options include:
    - "gnd", "high": logic levels
    - "common_trigger": common trigger signal
    - "t0_out": T0 signal output
    - "clk_in": clock input
    - "busy": busy signal
    - "status_packet_int": status packet interrupt
    - "veto_int", "veto_in", "veto_in+8": veto signals
    - "lemo_0" through "lemo_15": LEMO connector signals

#### Pulse Generator

- **pulsegen_freq** [base.pulsegen.freq]:
    Pulse generator frequency. Unit: Hz.

#### Status Packet Receiver (sp_rx)

- **frame_source** [base.sp_rx.frame_source]:
    Frame signal source for status packet receiver. Options: "lemo_0", "lemo_4", "lemo_8", "lemo_12", "rj45_lvds_0", "lvds_0", "lvds_8", "lvds_16", "lvds_24", "frame_usart_tx".

- **veto_source** [base.sp_rx.veto_source]:
    Veto signal source for status packet receiver. Options: "lemo_1", "lemo_5", "lemo_9", "lemo_13", "rj45_lvds_1", "lvds_1", "lvds_9", "lvds_17", "lvds_25", "veto_usart_tx".

#### T0 Generator

- **source** [base.t0.source]:
    T0 signal source. Options include:
    - "gnd", "high": logic levels
    - "t0_self": internal T0 generator
    - "rj45_lvds_0/1/2/3": RJ45 LVDS inputs
    - "lvds_0/1/2/3", "lvds_28/29/30/31": LVDS signals
    - "lemo_0" through "lemo_15": LEMO connector inputs

- **freq** [base.t0.freq]:
    T0 frequency when using internal T0 generator. Unit: Hz.

#### Clock and Synchronization

- **common_clock_source** [base.common_clock.source]:
    Common clock source. CLOCK MUST BE 25 MHZ!! Options:
    - "clk_int": internal clock
    - "clk_ext": external clock
    - "rj45_lvds_0/1/2/3": RJ45 LVDS inputs
    - "lvds_16", "lvds_26": LVDS signals

- **adc_sync_source** [base.adc_sync.source]:
    ADC synchronization source. Options:
    - "internal": internal sync
    - "gnd", "high": logic levels
    - "rj45_lvds_0/1/2/3": RJ45 LVDS inputs
    - "lvds_6", "lvds_14", "lvds_23", "lvds_27": LVDS signals
    - "lemo_0" through "lemo_15": LEMO connector inputs

#### Power Control

- **stave_power** [base.stave.power]:
    Enable power to the stave modules. Options: "true", "false".

### Software Processing Section

Controls the real-time signal processing performed by the software on acquired waveforms.

- **enable** [sw_process.enable]:
    Enable software processing. Options: "true", "false".

- **hist** [sw_process.hist]:
    Histogram binning parameter.

#### Histogram Processing

- **trigger_mode** [sw_process.histogram.trigger_mode]:
    Trigger mode for histogram processing. 
    1 = leading edge
    0 = derivative

- **histeresys** [sw_process.histogram.histeresys]:
    Hysteresis value for LE trigger detection. Unit: ADC counts.

- **treshold** [sw_process.histogram.treshold]:
    Time-of-flight threshold for each of the 8 channels. Unit: ADC counts.
    This is the threshold used for measuring the time of arrival of the signal.

- **treshold_ampl** [sw_process.histogram.treshold_ampl]:
    Amplitude spectrum threshold for each of the 8 channels. Unit: ADC counts.

- **timelife_ampl_thrs** [sw_process.histogram.timelife_ampl_thrs]:
    Time-lifetime amplitude threshold for each of the 8 channels. Unit: ADC counts.
    This is a validation threshold for the amplitude spectrum used in time-lifetime analysis. Only events with amplitude above this threshold are considered for time-lifetime calculations. 
    An event must pass both the time-of-flight threshold and the amplitude threshold to be included in the time-lifetime histogram.

```
                                    ---------
    timelife_ampl_thrs          -->/         |
                                  /          |                   -------------
    threshold                 -->/           |               -> /            |
                      ........../            |................./             |
                                 ^  Validated                    Discarded
                                    |  for time-lifetime

```

- **deconv_m** [sw_process.histogram.deconv_m]:
    Deconvolution parameter m for each of the 8 channels. Used for signal restoration.

- **deconv_enable** [sw_process.histogram.deconv_enable]:
    Enable deconvolution processing. Options: 1 (enabled), 0 (disabled).

- **superresolution_mode** [sw_process.histogram.superresolution_mode]:
    Enable super-resolution timing mode. Options: 1 (enabled), 0 (disabled).
    *** REMOVED FROM FIRMWARE AFTER 2025.x.x.x ***

- **delta** [sw_process.histogram.delta]:
    Delta parameter for timing analysis. Unit: nanoseconds.
    *** REMOVED FROM FIRMWARE AFTER 2025.x.x.x ***

- **inib** [sw_process.histogram.inib]:
    Inhibit time for histogram processing. Unit: nanoseconds.

- **rebin** [sw_process.histogram.rebin]:
    Rebinning factor for histogram data. Higher values reduce time resolution but improve statistics.

- **nf** [sw_process.histogram.nf]:
    Noise filter parameter.
    *** REMOVED FROM FIRMWARE AFTER 2025.x.x.x ***

#### Amplitude Histogram Subsection

- **blsub** [sw_process.histogram.A.blsub]:
    Baseline subtraction mode for amplitude histogram. 0 = no subtraction.

- **prebl** [sw_process.histogram.A.prebl]:
    Pre-baseline measurement length. Unit: samples.

- **maxw** [sw_process.histogram.A.maxw]:
    Maximum window for amplitude measurement. Unit: samples.

#### Low-Pass Filters
*** REMOVED FROM FIRMWARE AFTER 2025.x.x.x ***
- **enable** [sw_process.lp_filters.enable]:
    Enable low-pass filtering. Options: 1 (enabled), 0 (disabled).

- **a** [sw_process.lp_filters.a]:
    Filter coefficient 'a' (2 values for IIR filter).

- **b** [sw_process.lp_filters.b]:
    Filter coefficient 'b' (3 values for IIR filter).
