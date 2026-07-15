---
tags:
  - note
created: 2026-05-01
---
# Tags
[[super-musr]]
[[GUI]]

# Documentation for calibraiton of SiPM boards for scintillator tiles in SuperMuSR

## Calibration settings

`calibration_settings.py` contains all settings for calibration to be run.
### GLOBAL PARAMETERS
* `IP`: needs to be set so that the pc can connect to the board
Channel that needs to be calibrated
* `SECTION`: With full stave it's important to set the section that is to be calibrated (put the module in the correct position). The options are 0 (shortest, module A), 1 (module A), 2 (module B), 3 (longest, module C). With protostave it doesn't matter and the section should be set to 0
* `MAP`: **must** not be changed as it references how the SiPMs are mathched to the channels. For an explanation reference the table in the comments of the settings file. HV  follows the same mapping. Left is odd numbers, right is even numbers. For sections 1, 2, 3 the mapping numbers can be calculated by adding respectively 16, 32 and 48 to the section 1 indices
* `VTARGET`: target voltage used in `PULSER_SCAN.py`. This voltage is already compensated for module type and this is the real approximate voltage that will be applied to the SiPM during the scan
* `MODULE`: ***DANGER, if not set correctly it can destroy the module A***. The old module A cannot be operated at more than 59.5 V, while module B and C use a newer version that needs higer voltages to function
* `SINGLE_PHOTON_PEAK`: single photon PE estimation at `V_TARGET`, will be refined in `PULSER_SCAN.py`

### Offser parameters
* `TARGET_OFFSET`: offset that will be applied to digitizer waveform baseline. In GUI there is a parameter that is `in.offset` (fine tuning), different than `stave.offset` (true offset). `stave.offset` is important to recover bit usage, while `in.offset` is for alignement purposes and doesn't matter for data quality. With `calibration-offset.py` we calculate `stave.offset`
* `OFFSET_CAL_START_OFFSET, OFFSET_CAL_END_OFFSET`: minimum and maximum voltage applied to the offset
* `OFFSET_CAL_RAW_STEP, OFFSET_CAL_FINE_STEP`: voltage step size in volts

### Pulser calibration parameters
* `PULSER_MIN_VOLTAGE, PULSER_MAX_VOLTAGE`: maximum range for the pulser scan in mV, for safety the maximum should not exceed 2400
* `PULSER_STEP_RAW`: step in mV between coarse measurements
* `PULSER_MIN_PEAKS_COUNT, PULSER_MAX_PEAKS_COUNT`: requierements to have a PHS that is considered valid. If `PULSER_MAX_PEAKS_COUNT` is exceeded, the scan for that LED stops and an optimal value for $V_{LED}$
* `PULSER_FINE_SCAN`: number of scans for the fine tuning, arranged symmetrically around the optimal value chosen in the coarse scan
* `PULSER_FINE_STEP`: step in mV between two fine scans
* `PULSER_PPFACTOR_WEIGHT, PULSER_PEAK_WEIGHT`: the optimal voltage is chosen via two parameters: peak-to-valley factor and number of peaks in the spectrum that are above 1000 counts. The sum of the two weight should always be 1
* `PULSER_NOISE_FLOOR_INTEGRATION_TIME`: in seconds

![Typical output for coarse pulser scan](PULSER_CALIB_SCAN_0.png)
![Good spectrum example](PULSER_CALIB_0.png)


### HV scan parameters
* `HV_SCAN_TIME_STEP`: acquisition time
* `V1`: list of all voltages that will be analysed
* `PE_TARGET`: ADC LSB channels per photoelectron (target of the fit)
![Typical output of HV calibration](Combined_RIGHT_LEFT_P0_PE_vs_V_with_fit.png)

### Breakdown Voltage fit parameters
* `VBR_HV_MIN, VBR_HV_MAX`: minimum and maximum voltage to be scanned in volts, don't exceed 58 V
* `VBR_HV_STEP`: step in volts
* `VBR_N_WAVEFORMS`: how many waveforms to take for each HV point, to be averaged
* `VBR_PULSER_VOLTAGE`: pulser voltage for the scan in mV. **DANGER: cannot exceed 1500 as it is continuous light emission and the SiPM could be damaged**
![TO BE UPDATED: typical output for VBR calibration](VBR_variance_vs_HV_LEFT.png)

### **SETTINGS THAT MUST NOT BE EDITED**
```
if MODULE == 'A':
    V_cQ = 0
    MAIN_HV=59.9
else:
    V_cQ = 2.5
    MAIN_HV=62.0

V_cM = 1
```

## Run calibration
To run everything from powershell:
1.   Be sure that you're in the same folder as the GUI
2.   Activate virtual environment via `.\[name of environment]\Scripts\Activate.ps1`
3.  Run the powershell file `.\calibration-run-all.ps1`
4.  RUn the unified calibration file: `python create-unified-calibration.py --PID [serial number] --type [A,B,C]`

This will produce in folder `run` will create a unified calibration file called `unified_calibration_[type]_[PID].json`

### UPLOAD TO SERVER
This file should then be uploaded on the server, so that it can be accessed through  the GUI.
1. Rename the file as just `[PID].json`
2. Via File Explorer, the file needs to be uploade to the folder `\\daqserver.isis.cclrc.ac.uk\daqserver$\[experiment name]\calibration` This folder can be accessed via any RAL computer. 
![Calibration folder](doc1.png)


The calibrations can be accessed via the GUI, under `Calibration`
![GUI calibration tab](doc2.png)



# Explaination of the calibration step algorithms

## OFFSET CALIBRATION

### OVERVIEW:
---------
This script performs a two-stage offset calibration for SiPM (Silicon 
Photomultiplier) detector channels connected to a digitizer. The goal is to 
determine the optimal DC offset voltage (stave.offset parameter) that produces 
a target baseline level (TARGET_OFFSET, default 120 ADC counts) on the digitized 
waveforms.

The calibration is essential because each SiPM channel has slight variations in 
its baseline response, and proper baseline setting is critical for accurate signal 
processing and pulse detection.

### CALIBRATION METHODOLOGY:
------------------------
The script uses a two-stage approach:

1. COARSE SCAN:
   - Sweeps through offset voltages from OFFSET_CAL_START_OFFSET (0.6V) to 
     OFFSET_CAL_END_OFFSET (0.9V) in steps of OFFSET_CAL_RAW_STEP (0.025V)
   - For each offset value, captures waveforms and calculates the average baseline 
     for each channel
   - Performs linear regression (ADC counts vs offset voltage) for each channel
   - Calculates the offset voltage that would produce TARGET_OFFSET ADC counts
   - Generates "offset_scan_initial_fit.png" showing the linear fits

2. FINE SCAN:
   - Uses the coarse scan results as starting points
   - Scans ±N_FINE_STEP/2 points around each channel's estimated offset value
   - Uses smaller steps (OFFSET_CAL_FINE_STEP = 0.004V) for higher precision
   - Performs another linear regression on the refined data
   - Calculates the final offset voltage for each channel
   - Generates "offset_scan_fine_fit.png" showing the refined fits
   - Saves final offset values to "run/last_offset_calibration.csv"

### HARDWARE CONFIGURATION ASSUMPTIONS:
-----------------------------------
- The digitizer is configured with:
  * Bias voltage enabled at MAIN_HV (62.0V for modules B/C, 59.9V for module A)
  * Both LEFT and RIGHT SiPMs for each channel are DISABLED (ldo_enable = false)
    This ensures no photon signals interfere with baseline measurement
  * Periodic trigger mode at 40 kHz (trg.self_rate = 40)
  * Positive polarity input (in.polarity = "pos")
  * Pre-trigger: 1 μs, Post-trigger: 50 μs
  * Analog waveform mode (no digital processing)
  * Preamplifiers enabled for the selected SECTION
  * Emulator disabled (dgtz.emu.enable = false)
  * Pulser disabled to avoid injected signals

- Channel mapping follows the standard SuperMUSR layout:
  * Each digitizer channel (0-7) connects to LEFT and RIGHT SiPMs
  * LEFT SiPMs map to indices: [0,1,2,3,8,9,10,11]
  * RIGHT SiPMs map to indices: [4,5,6,7,12,13,14,15]
  * For multi-section staves, add 16*SECTION to the SiPM indices

### PARAMETERS (from calibration_settings.py):
------------------------------------------
- TARGET_OFFSET: Target baseline level in ADC counts (default: 120)
  This value is chosen to leave headroom below baseline for negative fluctuations
  while maximizing the dynamic range for positive signals (max ADC = 4095)

- OFFSET_CAL_START_OFFSET: Starting DC offset voltage (default: 0.6V)
  Lower bound of the coarse scan range

- OFFSET_CAL_END_OFFSET: Ending DC offset voltage (default: 0.9V)
  Upper bound of the coarse scan range - these bounds are empirically determined
  to cover the typical range needed for most SiPM modules

- OFFSET_CAL_RAW_STEP: Coarse scan step size (default: 0.025V)
  Larger steps for quick initial characterization (~12 points in coarse scan)

- OFFSET_CAL_FINE_STEP: Fine scan step size (default: 0.004V)
  Smaller steps for precision measurement (~6x finer than coarse scan)

- OFFSET_CAL_N_FINE_STEP: Number of fine scan points (default: 5)
  Creates a scan of ±2 points around the coarse result (5 total points)

- ENABLED_CH: List of channels to calibrate (default: [0,1,2,3,4,5,6,7])
  Must calibrate all 8 channels together to maintain thermal stability

- SECTION: Which stave section is being calibrated (0, 1, 2, or 3)
  Protostave uses SECTION=0; full staves use corresponding section number

- MAIN_HV: Main bias voltage (62.0V for B/C modules, 59.9V for A modules)
  Although SiPMs are disabled during offset calibration, the bias supply must
  be at its nominal voltage to properly initialize the system

### KEY ASSUMPTIONS:
----------------
1. The relationship between offset voltage and ADC baseline is LINEAR
   This is validated by the linear regression with typically R² > 0.99

2. Waveforms with avg=0 indicate SATURATION at the lower rail and are excluded
   This prevents fitting errors from clipped data points

3. Each channel's offset response is INDEPENDENT
   Channels are calibrated separately to account for individual variations

4. The system is THERMALLY STABLE during calibration
   Temperature changes can affect baselines; calibration assumes constant temperature

5. NO LIGHT is present during calibration
   SiPMs are disabled and external light sources should be off or blocked

6. Two waveform reads are performed per measurement (line 88-89)
   The first read is discarded to ensure the system has settled after configuration
   changes (addresses potential timing issues with parameter updates)

### OUTPUT FILES:
-------------
- run/offset_scan_initial_fit.png: Coarse scan linear fits for all channels
- run/offset_scan_fine_fit.png: Fine scan linear fits for all channels
- run/last_offset_calibration.csv: Final offset voltages (one row, 8 values)

The CSV file contains one offset voltage per channel (in Volts) that should be
applied via the stave.offset parameter to achieve TARGET_OFFSET ADC baseline.

### TYPICAL CALIBRATION VALUES:
----------------------------
- Expected offset voltages: 0.6 - 0.9V (varies by channel and module)
- Expected baseline spread: ±50 ADC counts before calibration
- Expected baseline accuracy after calibration: ±2 ADC counts
- Calibration time: ~5-10 minutes depending on step sizes

### USAGE:
------
1. Ensure calibration_settings.py is configured correctly (IP, SECTION, MODULE)
2. Verify no light sources are active
3. Run: python calibration-offset.py
4. Check generated plots for linearity (R² should be > 0.99)
5. Use values from last_offset_calibration.csv in subsequent calibrations
   or load them into the GUI's calibration system

### ERROR HANDLING:
---------------
- Saturation detection: Points with avg=0 are automatically excluded
- Insufficient data: If < 2 valid points exist for a channel, fitting is skipped
- Configuration errors: execute_cmd errors are caught and logged
- Timeout protection: SDK timeout set to 10 seconds for communication

---

## DETAILED ALGORITHM EXPLANATION

### Mathematical Foundation

The offset calibration exploits the **linear relationship** between the applied DC offset voltage and the resulting ADC baseline:

```
ADC_baseline = m × V_offset + q
```

Where:
- `ADC_baseline`: Mean value of the digitized waveform (in ADC counts, 0-4095)
- `V_offset`: Applied DC offset voltage (in Volts)
- `m`: Slope (ADC counts per Volt) - represents the gain of the offset circuit
- `q`: Intercept (ADC counts) - represents the baseline when V_offset = 0

**Goal**: Find the `V_offset` value that produces `ADC_baseline = TARGET_OFFSET` (default 120 ADC counts)

Solving for V_offset:
```
V_offset = (TARGET_OFFSET - q) / m
```

### Algorithm Flow Diagram

```
START
  │
  ├─► Initialize Hardware
  │    - Disable SiPMs (no light)
  │    - Set bias voltage to MAIN_HV
  │    - Configure trigger (40 kHz periodic)
  │    - Enable preamplifiers
  │
  ├─► COARSE SCAN (Stage 1)
  │    │
  │    ├─► FOR V_offset = 0.6V to 0.9V (step 0.025V)
  │    │    │
  │    │    ├─► Set stave.offset = V_offset for all channels
  │    │    ├─► Configure digitizer
  │    │    ├─► Read waveforms (discard first read)
  │    │    ├─► Read waveforms (actual measurement)
  │    │    │
  │    │    └─► FOR each channel (0-7)
  │    │         ├─► Calculate mean(waveform) → ADC_baseline
  │    │         ├─► IF ADC_baseline > 0 (not saturated)
  │    │         │    └─► Store (V_offset, ADC_baseline) pair
  │    │         └─► ELSE skip (saturation)
  │    │
  │    ├─► FOR each channel
  │    │    ├─► Perform linear regression on collected (V_offset, ADC_baseline) pairs
  │    │    │    → Get slope (m) and intercept (q)
  │    │    ├─► Calculate: V_target = (TARGET_OFFSET - q) / m
  │    │    └─► Store V_target for this channel
  │    │
  │    └─► Generate plot: offset_scan_initial_fit.png
  │         (shows linear fits for all channels)
  │
  ├─► FINE SCAN (Stage 2)
  │    │
  │    ├─► FOR step = -2 to +2 (±N_FINE_STEP/2)
  │    │    │
  │    │    ├─► FOR each channel
  │    │    │    └─► V_offset = V_target[channel] + step × 0.004V
  │    │    │
  │    │    ├─► Set all channel offsets
  │    │    ├─► Configure digitizer
  │    │    ├─► Read waveforms (discard first read)
  │    │    ├─► Read waveforms (actual measurement)
  │    │    │
  │    │    └─► FOR each channel
  │    │         ├─► Calculate mean(waveform) → ADC_baseline
  │    │         ├─► IF ADC_baseline > 0
  │    │         │    └─► Store (V_offset, ADC_baseline) pair
  │    │         └─► ELSE skip
  │    │
  │    ├─► FOR each channel
  │    │    ├─► Perform linear regression on fine scan data
  │    │    │    → Get refined slope (m') and intercept (q')
  │    │    ├─► Calculate: V_final = (TARGET_OFFSET - q') / m'
  │    │    └─► Store V_final for this channel
  │    │
  │    ├─► Generate plot: offset_scan_fine_fit.png
  │    │    (shows refined linear fits)
  │    │
  │    └─► Save final values to CSV:
  │         run/last_offset_calibration.csv
  │         Format: [V_ch0, V_ch1, V_ch2, ..., V_ch7]
  │
  └─► END
```

### Step-by-Step Breakdown

#### 1. Hardware Initialization (Lines 56-76)

```python
# Disable SiPMs to eliminate photon signals
sdk.set_parameter("stave.BIAS.ldo_enable", "false", LEFT_SIPM)
sdk.set_parameter("stave.BIAS.ldo_enable", "false", RIGHT_SIPM)

# Set bias voltage (important for circuit stability)
sdk.set_parameter("stave.BIAS.V", MAIN_HV)
sdk.set_parameter("stave.BIAS.enable", "true")

# Configure trigger for continuous waveform acquisition
sdk.set_parameter("trg.mode", "periodic")
sdk.set_parameter("trg.self_rate", 40)  # 40 kHz

# Enable preamplifiers
sdk.set_parameter("stave.pre_enable", "true", LEFT_PREAMP)
sdk.set_parameter("stave.pre_enable", "true", RIGHT_PREAMP)
```

**Why these settings?**
- **SiPMs disabled**: No light signals → measure pure baseline (electronic noise only)
- **Periodic trigger**: Ensures continuous data flow for averaging
- **MAIN_HV enabled**: Maintains circuit stability even though SiPMs are off
- **Preamplifiers enabled**: Emulates real operating conditions

#### 2. Coarse Scan (Lines 81-151)

**Purpose**: Quickly map the V_offset ↔ ADC_baseline relationship

```python
# Scan range: 0.6V to 0.9V in 0.025V steps (~12 points)
for r_o in np.arange(START_OFFSET, END_OFFSET, RAW_STEP):
    # Set offset for all channels
    for ch in ENABLED_CH:
        sdk.set_parameter("stave.offset", r_o, ch + SECTION*8)

    # Apply configuration
    configureAll(sdk)

    # Double-read pattern (first read discarded)
    wave = sdk.read_data("get_waveforms")  # Discard (settling time)
    wave = sdk.read_data("get_waveforms")  # Actual measurement

    # Calculate baseline for each channel
    for ch, waveform in enumerate(wave["wave"]):
        avg = np.mean(waveform)

        if avg > 0:  # Not saturated
            channel_data[ch]['r_o'].append(r_o)
            channel_data[ch]['avg'].append(avg)
```

**Why double read?**
The first waveform may contain transient effects from the configuration change. The second read ensures the system has fully settled.

**Linear Regression:**
```python
# Fit line: ADC = m × V_offset + q
slope, intercept, r_value, p_value, std_err = stats.linregress(r_o_data, avg_data)

# Solve for target offset voltage
r_o_target = (TARGET_OFFSET - intercept) / slope
```

**Example:**
```
Measured points:
V_offset (V) | ADC_baseline
  0.60       |    50
  0.65       |   110
  0.70       |   170
  0.75       |   230
  ...

Linear fit: ADC = 1200 × V + (-670)

Target: ADC = 120
V_target = (120 - (-670)) / 1200 = 0.658V
```

#### 3. Fine Scan (Lines 154-229)

**Purpose**: Refine the offset voltage estimate with higher precision

```python
# Scan ±2 steps around coarse result with 0.004V resolution
for ss in range(-N_FINE_STEP//2, N_FINE_STEP//2 + 1):
    for ch in ENABLED_CH:
        # Fine-tune around coarse result
        r_o = target_r_o_values[ch] + ss * FINE_STEP
        sdk.set_parameter("stave.offset", r_o, ch + SECTION*8)

    # Same measurement procedure as coarse scan
    configureAll(sdk)
    wave = sdk.read_data("get_waveforms")
    wave = sdk.read_data("get_waveforms")

    # Collect fine-resolution data
    for ch, waveform in enumerate(wave["wave"]):
        avg = np.mean(waveform)
        if avg > 0:
            channel_data[ch]['r_o'].append(...)
            channel_data[ch]['avg'].append(avg)
```

**Why fine scan?**
- Coarse scan: Fast but resolution limited to ±0.0125V (half step size)
- Fine scan: Slower but resolution improved to ±0.002V
- Overall calibration accuracy: ~2 ADC counts (vs. 120 target)

**Final Linear Regression:**
```python
# Re-fit with fine data
slope, intercept, _, _, _ = stats.linregress(r_o_data, avg_data)
r_o_target = (TARGET_OFFSET - intercept) / slope
final_r_o_values.append(r_o_target)
```

#### 4. Output Generation (Lines 231-238)

```python
# Save to CSV: one row, 8 columns (one per channel)
with open("run/last_offset_calibration.csv", 'w') as file:
    writer = csv.writer(file)
    writer.writerow(final_r_o_values)
    # Example: [0.658, 0.662, 0.655, 0.670, 0.648, 0.661, 0.652, 0.665]
```

### Numerical Example - Complete Walkthrough

**Channel 0 Calibration:**

**Coarse Scan Data:**
```
V_offset (V) | ADC_baseline | Data Point #
   0.600     |     48       |      1
   0.625     |    105       |      2
   0.650     |    162       |      3
   0.675     |    219       |      4
   0.700     |    276       |      5
   0.725     |    333       |      6
   0.750     |    390       |      7
   0.775     |    447       |      8
   0.800     |    504       |      9
```

**Coarse Linear Fit:**
```
Regression: ADC = 1824.0 × V - 1047.2
R² = 0.9998  (excellent linearity!)

Target calculation:
V_coarse = (120 - (-1047.2)) / 1824.0 = 0.6399V
```

**Fine Scan Data (around 0.6399V):**
```
V_offset (V) | ADC_baseline
   0.6319    |    105
   0.6359    |    112
   0.6399    |    119
   0.6439    |    126
   0.6479    |    133
```

**Fine Linear Fit:**
```
Regression: ADC = 1820.5 × V - 1045.8
R² = 0.9999

Final calculation:
V_final = (120 - (-1045.8)) / 1820.5 = 0.6404V
```

**Result:**
- Coarse estimate: 0.6399V
- Fine estimate: 0.6404V
- Refinement: 0.5 mV improvement
- Expected ADC at V_final: 120.0 ± 1.5 counts

### Why This Algorithm Works

1. **Linearity**: The offset circuit has excellent linear response (R² > 0.99)
2. **Two-stage approach**: Balances speed (coarse) vs. precision (fine)
3. **Saturation handling**: Automatically excludes invalid data points
4. **Channel independence**: Each channel calibrated separately (accounts for component variations)
5. **Settling time**: Double-read pattern ensures stable measurements

### Common Issues and Solutions

| Issue | Symptom | Solution |
|-------|---------|----------|
| **Non-linear response** | R² < 0.95 | Check for SiPM light leakage, verify SiPMs are disabled |
| **Saturation (ADC=0)** | Many points excluded | Increase START_OFFSET (try 0.65V instead of 0.6V) |
| **Saturation (ADC=4095)** | Points at upper end | Decrease END_OFFSET (try 0.85V instead of 0.9V) |
| **Channel variation** | Offsets differ by >0.1V | Normal (component tolerances), use per-channel values |
| **Temperature drift** | Results change over time | Ensure thermal stability, wait 30 min after power-on |

### Integration with Other Calibrations

The offset calibration is **step 1** in the calibration chain:

```
1. Offset Calibration → baseline at 120 ADC counts
   ↓
2. Pulser Calibration → find optimal LED voltage for single-PE resolution
   ↓
3. HV Calibration → find V_bias for target PE/ADC gain (20 ADC/PE)
   ↓
4. Unified Calibration → combine all parameters into single JSON file
```

All subsequent calibrations assume the baseline is correctly set to TARGET_OFFSET.

---

## PULSER CALIBRATION (calibration-pulser-ai.py)

### OVERVIEW

The pulser calibration script determines the optimal LED voltage for each SiPM channel that produces a clear photoelectron (PE) spectrum with well-resolved peaks. This calibration is essential for:
- Verifying SiPM functionality
- Measuring single photoelectron resolution
- Providing a reference for HV calibration

The script operates in **two phases**:
1. **Noise Floor Calibration**: Find optimal trigger threshold by identifying the noise valley
2. **Pulser Calibration**: Find optimal LED voltage using a two-stage scan (coarse + fine)

### CALIBRATION METHODOLOGY

#### PHASE 1: NOISE FLOOR CALIBRATION

**Goal**: Determine the optimal trigger threshold that separates electronic noise from single-PE signals

**Process**:
1. Set initial trigger at `TARGET_OFFSET - IN_OFFSET + 0.2 × SINGLE_PHOTON_PEAK`
2. Enable SiPMs with bias voltage but **disable all pulsers** (no light)
3. Acquire dark count spectrum for 10 seconds
4. Find the first valley from the left (minimum between noise pedestal and single-PE peak)
5. Set trigger at the valley position

**Parallel Acquisition Strategy**:
- LEFT side: All 8 channels measured simultaneously (one 10s acquisition)
- RIGHT side: All 8 channels measured simultaneously (one 10s acquisition)
- Total noise calibration time: ~20 seconds (instead of 160s if done sequentially)

**Output**:
- Optimal trigger value for each of the 16 SiPM channels (8 LEFT + 8 RIGHT)
- Noise floor plots showing trigger positions
- Summary CSV with trigger values

#### PHASE 2: PULSER CALIBRATION

**Goal**: Find the optimal LED voltage that produces the best photoelectron spectrum

**Two-Stage Approach**:

**1. COARSE SCAN:**
- Sweep LED voltage from `PULSER_MIN_VOLTAGE` (1750 mV) to `PULSER_MAX_VOLTAGE` (2060 mV)
- Step size: `PULSER_STEP_RAW` (20 mV) → ~15 voltage points
- Integration time: 3 seconds per voltage
- For each voltage:
  - Count number of peaks in spectrum
  - Calculate peak-to-valley factor (ppfactor)
  - Store both metrics

**Peak Detection Algorithm**:
```python
# Find valley near main peak (within ±SINGLE_PHOTON_PEAK window)
valley_value = find_valley_near_peak(spectrum, max_position, SINGLE_PHOTON_PEAK)

# Dynamic prominence threshold: 5% above valley
prominence = 0.05 × (spectrum - valley_value)

# Find peaks with:
# - Height > 2% of max peak
# - Prominence > 5% above valley
# - Distance > SINGLE_PHOTON_PEAK / 2.5
peaks = find_peaks(spectrum, height=0.02*max_value,
                   prominence=prominence,
                   distance=SINGLE_PHOTON_PEAK/2.5)
```

**Peak-to-Valley Factor (ppfactor)**:
```
For each pair of adjacent peaks:
    valley = minimum value between peaks
    max_peak = max(left_peak, right_peak)
    ppfactor = |max_peak - valley| / max_peak

Best ppfactor = maximum across all peak pairs
```

Higher ppfactor (closer to 1.0) = better peak separation = clearer spectrum

**Selection Criteria**:
Only voltages with `PULSER_MIN_PEAKS_COUNT ≤ num_peaks ≤ PULSER_MAX_PEAKS_COUNT` are considered valid.

**Weighted Score**:
```
score = (PULSER_PEAK_WEIGHT × normalized_peaks) +
        (PULSER_PPFACTOR_WEIGHT × normalized_ppfactor)

Default weights: 20% peaks, 80% ppfactor
```

The voltage with the highest score is selected for fine scanning.

**Early Exit Criterion**:
If counts at position `STOP_LIMITER` exceed 10% of peak value, the scan stops (spectrum too bright/saturated).

**2. FINE SCAN:**
- Scan around the optimal voltage from coarse scan
- Range: ±`PULSER_FINE_SCAN` (10 mV) in steps of `PULSER_FINE_STEP` (5 mV)
- Example: If coarse found 1820 mV → scan [1810, 1815, 1820, 1825, 1830] mV
- Integration time: 3 seconds per voltage
- Metric: Only ppfactor (number of peaks already optimized)

**Final Selection**:
The voltage with the maximum ppfactor in the fine scan is the final optimal voltage.

### ALGORITHM FLOW DIAGRAM

```
START
  │
  ├─► Load offset calibration results
  │    (from run/last_offset_calibration.csv)
  │
  ├─► Hardware Setup
  │    - Set bias voltage to VTARGET
  │    - Apply offset calibration values
  │    - Configure multiphoton mode (peak_holder)
  │    - Enable periodic trigger (40 kHz)
  │    - Disable all SiPMs and pulsers initially
  │
  ├─► PHASE 1: NOISE FLOOR CALIBRATION
  │    │
  │    ├─► LEFT SIDE (all 8 channels in parallel)
  │    │    │
  │    │    ├─► Set initial trigger = TARGET_OFFSET - IN_OFFSET + 0.2×SINGLE_PHOTON_PEAK
  │    │    ├─► Enable all LEFT SiPMs (pulsers OFF)
  │    │    ├─► Enable LEFT preamplifier
  │    │    ├─► Reset spectra
  │    │    ├─► Integrate for 10 seconds
  │    │    ├─► Read all 8 dark count spectra
  │    │    │
  │    │    └─► FOR each of 8 channels
  │    │         ├─► Find first valley from left (min_depth=20)
  │    │         ├─► Set optimal_trigger[ch, LEFT] = valley_position
  │    │         ├─► Save noise spectrum plot
  │    │         └─► Save summary file
  │    │
  │    ├─► RIGHT SIDE (all 8 channels in parallel)
  │    │    │
  │    │    ├─► Disable LEFT SiPMs
  │    │    ├─► Enable all RIGHT SiPMs (pulsers OFF)
  │    │    ├─► Enable RIGHT preamplifier
  │    │    ├─► Reset spectra
  │    │    ├─► Integrate for 10 seconds
  │    │    ├─► Read all 8 dark count spectra
  │    │    │
  │    │    └─► FOR each of 8 channels
  │    │         ├─► Find first valley from left
  │    │         ├─► Set optimal_trigger[ch, RIGHT] = valley_position
  │    │         ├─► Save noise spectrum plot
  │    │         └─► Save summary file
  │    │
  │    └─► Save NOISE_FLOOR_CALIBRATION.csv
  │         (16 rows: trigger values for all SiPMs)
  │
  ├─► PHASE 2: PULSER CALIBRATION
  │    │
  │    └─► FOR each of 16 SiPMs (8 LEFT + 8 RIGHT)
  │         │
  │         ├─► Use optimal trigger from Phase 1
  │         ├─► Enable only this SiPM + pulser
  │         │
  │         ├─► COARSE SCAN
  │         │    │
  │         │    ├─► FOR voltage = 1750 to 2060 mV (step 20 mV)
  │         │    │    │
  │         │    │    ├─► Set LED voltage
  │         │    │    ├─► Wait 3s stabilization
  │         │    │    ├─► Reset spectra
  │         │    │    ├─► Integrate 3 seconds
  │         │    │    ├─► Read spectrum
  │         │    │    │
  │         │    │    ├─► Find peaks (dynamic prominence)
  │         │    │    ├─► Count number of peaks
  │         │    │    ├─► Calculate ppfactor
  │         │    │    │
  │         │    │    ├─► IF counts_at_limit > 10% of peak
  │         │    │    │    └─► BREAK (saturation)
  │         │    │    │
  │         │    │    └─► Save (voltage, num_peaks, ppfactor)
  │         │    │
  │         │    ├─► Filter voltages with valid peak count
  │         │    │    (PULSER_MIN_PEAKS_COUNT ≤ peaks ≤ PULSER_MAX_PEAKS_COUNT)
  │         │    │
  │         │    ├─► Calculate weighted scores
  │         │    │    score = 0.2×(peaks/max_peaks) + 0.8×(ppfactor/max_ppfactor)
  │         │    │
  │         │    ├─► optimal_voltage_coarse = argmax(score)
  │         │    │
  │         │    └─► Save coarse scan plot
  │         │
  │         ├─► FINE SCAN
  │         │    │
  │         │    ├─► FOR voltage in [optimal_coarse - 10 : optimal_coarse + 10] (step 5 mV)
  │         │    │    │
  │         │    │    ├─► Set LED voltage
  │         │    │    ├─► Wait 3s stabilization
  │         │    │    ├─► Reset spectra
  │         │    │    ├─► Integrate 3 seconds
  │         │    │    ├─► Read spectrum
  │         │    │    │
  │         │    │    ├─► Calculate ppfactor
  │         │    │    │
  │         │    │    └─► Save (voltage, ppfactor)
  │         │    │
  │         │    ├─► optimal_voltage_fine = argmax(ppfactor)
  │         │    │
  │         │    └─► Save fine scan plot
  │         │
  │         ├─► Final acquisition at optimal_voltage_fine
  │         │    └─► Save final spectrum plot
  │         │
  │         └─► Return optimal_voltage_fine
  │
  ├─► Save PULSER_CALIBRATION.csv
  │    Format: [Digitizer_CH, Pulser_LEFT, Voltage_LEFT, Pulser_RIGHT, Voltage_RIGHT]
  │
  └─► END
```

### MATHEMATICAL DETAILS

#### Peak Detection Parameters

**Dynamic Prominence Calculation**:
```
valley = find_valley_near_peak(spectrum, peak_position, window=SINGLE_PHOTON_PEAK)
prominence(x) = 0.05 × (spectrum[x] - valley)
```

This adaptive approach ensures:
- Peak detection adjusts to baseline level
- Works across different light intensities
- Reduces false positives from noise

**Distance Constraint**:
```
minimum_distance = SINGLE_PHOTON_PEAK / 2.5
```
If `SINGLE_PHOTON_PEAK = 30`, then peaks must be at least 12 ADC counts apart.

#### Weighted Scoring Function

```
Normalized values:
  norm_peaks = num_peaks / max(valid_num_peaks)
  norm_ppfactor = ppfactor / max(valid_ppfactors)

Weighted score:
  S = w_peaks × norm_peaks + w_ppfactor × norm_ppfactor

Default weights:
  w_peaks = 0.20  (PULSER_PEAK_WEIGHT)
  w_ppfactor = 0.80  (PULSER_PPFACTOR_WEIGHT)
```

**Rationale**: ppfactor is more important than peak count because:
- Good peak separation → accurate PE counting
- Too many peaks → light too bright, peaks merge
- Too few peaks → insufficient dynamic range
- ppfactor directly measures spectrum quality

### NUMERICAL EXAMPLE

**Channel 0 LEFT - Complete Walkthrough**

**Noise Floor Calibration**:
```
Initial trigger: 120 - 0 + 0.2×30 = 126 ADC

Noise spectrum (pulsers OFF, 10s integration):
  Position | Counts
    100    | 1200   ← Noise pedestal starts
    115    |  950
    125    |  420   ← Valley (minimum)
    135    |  650   ← Single-PE events start rising
    150    | 1100

Optimal trigger = 125 ADC (at valley)
```

**Coarse Scan Results**:
```
Voltage (mV) | Num Peaks | ppfactor | Score
   1750      |     2     |  0.45    | 0.24
   1770      |     3     |  0.52    | 0.30
   1790      |     4     |  0.61    | 0.38
   1810      |     6     |  0.71    | 0.51  ← Good candidate
   1830      |     7     |  0.68    | 0.54  ← Best score!
   1850      |     9     |  0.62    | 0.51
   1870      |    11     |  0.55    | 0.44
   1890      |    12     |  0.48    | 0.38
   1910      |    14     |  0.41    | 0.28  (exceeds MAX_PEAKS)

Valid range: 5 ≤ peaks ≤ 15
Optimal coarse voltage: 1830 mV (score = 0.54)
```

**Fine Scan Results** (around 1830 mV):
```
Voltage (mV) | ppfactor
   1820      |  0.665
   1825      |  0.681
   1830      |  0.687  ← Maximum!
   1835      |  0.679
   1840      |  0.671

Optimal fine voltage: 1830 mV (ppfactor = 0.687)
```

**Final Result**: LED voltage = 1830 mV for Channel 0 LEFT

### PARAMETERS (from calibration_settings.py)

- **PULSER_MIN_VOLTAGE**: 1750 mV
  Starting voltage for coarse scan

- **PULSER_MAX_VOLTAGE**: 2060 mV
  **DANGER**: Do not exceed 2400 mV (LED damage risk)

- **PULSER_STEP_RAW**: 20 mV
  Coarse scan step size

- **PULSER_MIN_PEAKS_COUNT**: 5
  Minimum acceptable number of peaks

- **PULSER_MAX_PEAKS_COUNT**: 15
  Maximum acceptable number of peaks (prevents saturation)

- **PULSER_FINE_SCAN**: 10 mV
  Fine scan range (±10 mV around coarse optimum)

- **PULSER_FINE_STEP**: 5 mV
  Fine scan step size (5 points total)

- **PULSER_PPFACTOR_WEIGHT**: 0.80
  Weight for peak-to-valley factor in scoring

- **PULSER_PEAK_WEIGHT**: 0.20
  Weight for number of peaks in scoring

- **PULSER_NOISE_FLOOR_INTEGRATION_TIME**: 10 seconds
  Integration time for noise floor measurement

- **SINGLE_PHOTON_PEAK**: 30 ADC counts (estimated)
  Expected single-PE peak width (refined during calibration)

### KEY ASSUMPTIONS

1. **Linear LED Response**: LED output is approximately proportional to voltage in the scan range
2. **Stable Temperature**: SiPM gain doesn't change during calibration
3. **Offset Calibration Done**: Baseline is correctly set at TARGET_OFFSET
4. **No External Light**: Room is dark or SiPMs are light-sealed
5. **Bias Voltage Stable**: VTARGET voltage is applied and stable

### OUTPUT FILES

**Per SiPM Channel** (16 total):
- `NOISE_FLOOR_CH{ch}_STAVE{stave}_LEFT/RIGHT.png`: Noise floor plot with trigger markers
- `NOISE_FLOOR_SUMMARY_CH{ch}_STAVE{stave}_LEFT/RIGHT.txt`: Noise statistics
- `_noisefloor_ch{ch}_stave{stave}_LEFT/RIGHT_initial.csv`: Raw noise spectrum data
- `PULSER_CALIB_SCAN_{stave}.png`: Coarse scan overlay plot
- `PULSER_CALIB_SCAN_FINE_{stave}.png`: Fine scan overlay plot
- `PULSER_CALIB_{stave}.png`: Final optimal spectrum
- `PULSER_CALIB_LOG_{stave}.txt`: Detailed log with tables
- `_fastscan_{stave}_{voltage}.csv`: Raw coarse scan spectra
- `_fine_{stave}_{voltage}.csv`: Raw fine scan spectra
- `_final_{stave}_{voltage}.csv`: Final optimal spectrum data

**Summary Files**:
- `NOISE_FLOOR_CALIBRATION.csv`: Trigger values for all 16 SiPMs
- `PULSER_CALIBRATION.csv`: Optimal LED voltages for all 16 SiPMs
- `run/PULSER_CALIBRATION.csv`: Copy for downstream processing

### TYPICAL CALIBRATION VALUES

- **Noise trigger**: 100-150 ADC counts (depends on offset setting)
- **Optimal LED voltage**: 1750-2000 mV (varies by SiPM and bias voltage)
- **Number of peaks**: 5-15 (optimal range)
- **ppfactor**: 0.5-0.8 (higher is better)
- **Total calibration time**: ~60 minutes for 16 SiPMs


## HV CALIBRATION (calibration-scan-hv.py)

### OVERVIEW

The HV (High Voltage) calibration script determines the optimal bias voltage for each SiPM that produces the target photoelectron (PE) gain. This is the **most critical calibration** because:
- Sets the operating point for all SiPM measurements
- Directly determines signal-to-noise ratio
- Ensures uniform gain across all 16 SiPMs
- Affects energy resolution and timing performance

**Goal**: Find the bias voltage where the single photoelectron peak appears at `PE_TARGET` ADC counts (default: 20 ADC/PE).

### CALIBRATION METHODOLOGY

The HV calibration measures the photoelectron spectrum at multiple bias voltages and fits the relationship between voltage and PE gain.

**Key Equation**:
```
PE_position = m × V_bias + q

Where:
- PE_position: Single-PE peak position in ADC counts
- V_bias: Applied bias voltage (Volts)
- m: Slope (ADC counts per Volt) - SiPM gain coefficient
- q: Intercept (ADC counts) - baseline offset

To find optimal voltage for target gain:
V_optimal = (PE_TARGET - q) / m
```

**Process Overview**:
1. Load offset and pulser calibration results
2. For each voltage in `V1` list (e.g., 56.0V to 61.0V):
   - Dynamically adjust trigger threshold (FindMPETrigger)
   - Enable one SiPM at a time with its optimal LED voltage
   - Acquire photoelectron spectrum (HV_SCAN_TIME_STEP seconds)
   - Save spectrum for offline analysis
3. Offline analysis (typically calibration-hv-fit-calibfile.py):
   - Find single-PE peak position in each spectrum
   - Plot PE peak position vs bias voltage
   - Perform linear fit to get m and q
   - Calculate optimal voltage for PE_TARGET

### ALGORITHM FLOW DIAGRAM

```
START
  │
  ├─► Load Previous Calibration Results
  │    - run/last_offset_calibration.csv → STAVE_OFFSET_SETTINGS[8]
  │    - run/PULSER_CALIBRATION.csv → PULSER_LEFT[8], PULSER_RIGHT[8]
  │
  ├─► Hardware Setup
  │    - Apply offset calibration values (stave.offset)
  │    - Apply pulser LED voltages (stave.pulserX.ch_lsb)
  │    - Configure multiphoton mode (peak_holder, gate_len=20us)
  │    - Set bias voltage limits (max_v = MAIN_HV + 2V)
  │    - Enable periodic trigger (40 kHz)
  │    - Disable all SiPMs and pulsers initially
  │
  ├─► FOR each voltage V in V1 list
  │    │    Example: V1 = [56.0, 56.5, 57.0, ..., 60.5, 61.0]
  │    │
  │    ├─► FindMPETrigger() - Adaptive trigger calibration
  │    │    │
  │    │    ├─► Enable ALL SiPMs (LEFT + RIGHT)
  │    │    ├─► Set initial trigger = TARGET_OFFSET - IN_OFFSET + 20
  │    │    ├─► Configure and wait 3s
  │    │    ├─► Integrate for 3 seconds
  │    │    ├─► Read spectra for all 8 channels
  │    │    │
  │    │    └─► FOR each channel
  │    │         ├─► Find spectrum peak: max_position = argmax(spectrum)
  │    │         ├─► Set trigger above peak: new_trigger = max_position + 15
  │    │         └─► Update mp.threshold parameter
  │    │
  │    ├─► FOR each LEFT SiPM (8 SiPMs)
  │    │    │
  │    │    ├─► Set bias voltage for ALL SiPMs
  │    │    │    V_applied = V × V_cM - V_cQ
  │    │    │    (Module A: V_cM=1, V_cQ=0; Module B/C: V_cM=1, V_cQ=2.5)
  │    │    │
  │    │    ├─► Disable all SiPMs and pulsers (switch_off_everything)
  │    │    ├─► Enable ONLY this LEFT SiPM + pulser (switch_on_channel)
  │    │    │    - stave.BIAS.ldo_enable = true
  │    │    │    - stave.pulserX.ch_enable = true
  │    │    │    - stave.pre_enable LEFT = true, RIGHT = false
  │    │    │
  │    │    ├─► Set LED voltage = PULSER_LEFT[ch]
  │    │    ├─► Configure hardware
  │    │    │
  │    │    ├─► RunAcquisitionEnergySpectrum()
  │    │    │    ├─► Wait 3s stabilization
  │    │    │    ├─► Reset spectra (double reset)
  │    │    │    ├─► Integrate for HV_SCAN_TIME_STEP seconds
  │    │    │    ├─► Read dark count spectrum
  │    │    │    │
  │    │    │    └─► Save: run_hv/_LEFT_PULSER_{ch}_{V}_spectra_A.csv
  │    │    │
  │    │    └─► Progress bar update
  │    │
  │    └─► FOR each RIGHT SiPM (8 SiPMs)
  │         │
  │         ├─► Same process as LEFT
  │         │    - Enable RIGHT preamplifier instead
  │         │    - Use PULSER_RIGHT[ch] voltage
  │         │
  │         └─► Save: run_hv/_RIGHT_PULSER_{ch}_{V}_spectra_A.csv
  │
  ├─► OFFLINE ANALYSIS (separate script: calibration-hv-fit-calibfile.py)
  │    │
  │    ├─► FOR each SiPM (16 total: 8 LEFT + 8 RIGHT)
  │    │    │
  │    │    ├─► Load all spectra across voltages
  │    │    │    Example for LEFT 0:
  │    │    │    - _LEFT_PULSER_0_56.0_spectra_A.csv
  │    │    │    - _LEFT_PULSER_0_56.5_spectra_A.csv
  │    │    │    - ... (all voltages)
  │    │    │
  │    │    ├─► FOR each voltage
  │    │    │    ├─► Find single-PE peak position
  │    │    │    │    (histogram maximum, typically 15-30 ADC)
  │    │    │    └─► Store (V_bias, PE_position) pair
  │    │    │
  │    │    ├─► Linear regression: PE_position = m × V_bias + q
  │    │    │    scipy.stats.linregress(V_values, PE_values)
  │    │    │
  │    │    ├─► Calculate optimal voltage
  │    │    │    V_optimal = (PE_TARGET - q) / m
  │    │    │
  │    │    ├─► Generate plot
  │    │    │    - Scatter: measured (V, PE) points
  │    │    │    - Line: fitted line PE = m×V + q
  │    │    │    - Marker: optimal point (V_optimal, PE_TARGET)
  │    │    │
  │    │    └─► Save calibration parameters
  │    │         {
  │    │           "SiPM_X": {
  │    │             "m": slope,
  │    │             "q": intercept,
  │    │             "channel": ch,
  │    │             "position": "LEFT/RIGHT",
  │    │             "P": PE_TARGET
  │    │           }
  │    │         }
  │    │
  │    └─► Export unified_calibration JSON
  │
  └─► END
```

### DETAILED STEP BREAKDOWN

#### 1. Loading Previous Calibrations (Lines 36-62)

```python
# Load offset calibration (8 values, one per digitizer channel)
with open("run/last_offset_calibration.csv", 'r') as f:
    content = f.read().strip()
    STAVE_OFFSET_SETTINGS = [float(x) for x in content.split(',')]
# Example: [0.658, 0.662, 0.655, 0.670, 0.648, 0.661, 0.652, 0.665]

# Load pulser calibration (8 LEFT + 8 RIGHT LED voltages)
with open("run/PULSER_CALIBRATION.csv", 'r') as f:
    reader = csv.reader(f)
    for row in reader:
        if row[0] != "Digitizer CH":
            ch = int(row[0])
            PULSER_LEFT[ch] = float(row[2])   # Column 2: LEFT voltage (mV)
            PULSER_RIGHT[ch] = float(row[4])  # Column 4: RIGHT voltage (mV)
# Example: PULSER_LEFT = [1830, 1825, 1840, ...]
#          PULSER_RIGHT = [1815, 1820, 1835, ...]
```

**Why load previous calibrations?**
- **Offset**: Ensures baseline is at TARGET_OFFSET (120 ADC)
- **Pulser**: Uses optimal LED voltage to generate clean PE spectra with good peak separation

#### 2. FindMPETrigger() Function (Lines 136-181)

**Purpose**: Dynamically adjust trigger threshold to adapt to gain changes at each bias voltage.

**Problem**: At different bias voltages, the SiPM gain changes significantly:
```
V_bias = 56.0V → Single-PE at ~100 ADC → Need trigger at ~115 ADC
V_bias = 60.0V → Single-PE at ~200 ADC → Need trigger at ~215 ADC
```
A fixed trigger won't work across the full voltage range.

**Solution**: Before each voltage measurement, acquire a quick spectrum and find where the signal actually is.

```python
def FindMPETrigger(sdk):
    # Enable ALL SiPMs (both LEFT and RIGHT) to get representative spectra
    for ch in ENABLED_CH:
        sdk.set_parameter("stave.BIAS.ldo_enable", "true", MAP_LEFT[ch] + SECTION*16)
        sdk.set_parameter("stave.BIAS.ldo_enable", "true", MAP_RIGHT[ch] + SECTION*16)

        # Set conservative initial trigger (low, to catch everything)
        initial_trigger = TARGET_OFFSET - IN_OFFSET + 20  # ~140 ADC typical
        sdk.set_parameter("mp.threshold", initial_trigger, ch)

    configureAll(sdk)
    time.sleep(3)  # Wait for system to stabilize

    # Quick 3-second integration to see where signal is
    sdk.execute_cmd("reset_darkcount_spectra")
    time.sleep(3)
    spectra_a = sdk.read_data("get_darkcount_spectra")

    # Adjust trigger for each channel based on actual signal position
    for ch in ENABLED_CH:
        spectrum = np.array(spectra_a[ch][0:trigger+1000])
        max_position = np.argmax(spectrum[1:]) + 10  # Find peak (+10 offset)

        # Set trigger 15 ADC above peak (captures signal, rejects noise)
        new_trigger = max_position + 15
        sdk.set_parameter("mp.threshold", new_trigger, ch)

        print(f"Channel {ch} -> Old trigger: {initial_trigger}, New trigger: {new_trigger}")
```

**Trigger Strategy**:
```
Noise floor: ~100-120 ADC
Single-PE peak: ~150-250 ADC (depends on V_bias)
Trigger setting: peak + 15 ADC

Result: Captures all PE events, rejects baseline noise
```

#### 3. Bias Voltage Application with Module Correction (Lines 247-249, 264-265)

**Hardware Detail**: The bias voltage system has a built-in offset (V_cQ) that varies by module type.

**Module A (Old SiPMs)**:
```
V_cQ = 0.0V
V_cM = 1.0

To apply 58.0V:
correction_manual = 58.0 × 1.0 - 0.0 = 58.0
Actual voltage = correction_manual + 0.0 = 58.0V ✓
```

**Modules B/C (New SiPMs)**:
```
V_cQ = 2.5V
V_cM = 1.0

To apply 58.0V:
correction_manual = 58.0 × 1.0 - 2.5 = 55.5
Actual voltage = correction_manual + 2.5 = 58.0V ✓
```

**Code Implementation**:
```python
for ch in range(8):
    # Apply to both LEFT and RIGHT SiPMs of this digitizer channel
    sdk.set_parameter("stave.BIAS.correction_manual",
                      V1[iv1] × V_cM - V_cQ,
                      MAP_LEFT[ch] + SECTION*16)
    sdk.set_parameter("stave.BIAS.correction_manual",
                      V1[iv1] × V_cM - V_cQ,
                      MAP_RIGHT[ch] + SECTION*16)
```


#### 4. Sequential SiPM Measurement (Lines 244-272)

**Measurement Sequence for ONE Voltage Point**:
```
1. FindMPETrigger() → Set optimal trigger for this voltage

2. LEFT side (8 measurements):
   ├─► SiPM 0: Enable, measure, save, disable
   ├─► SiPM 1: Enable, measure, save, disable
   ├─► SiPM 2: Enable, measure, save, disable
   ├─► SiPM 3: Enable, measure, save, disable
   ├─► SiPM 4: Enable, measure, save, disable
   ├─► SiPM 5: Enable, measure, save, disable
   ├─► SiPM 6: Enable, measure, save, disable
   └─► SiPM 7: Enable, measure, save, disable

3. RIGHT side (8 measurements):
   ├─► SiPM 0: Enable, measure, save, disable
   ├─► SiPM 1: Enable, measure, save, disable
   └─► ... (same as LEFT)

Total: 16 measurements per voltage point
```

**Per-SiPM Measurement Code**:
```python
# For LEFT SiPM
scan_common_name = f"run_hv/_LEFT_PULSER_{pulser}_{V1[iv1]}"

# Select this pulser LED
sdk.set_parameter(f"stave.pulser{SECTION}.selected_pulser", MAP_LEFT[pulser])

# Disable all, then enable only this one
switch_off_everything(sdk)
switch_on_channel(sdk, MAP_LEFT[pulser], pre_ch=0)  # 0 = LEFT preamplifier

configureAll(sdk)

# Acquire spectrum
RunAcquisitionEnergySpectrum(sdk, HV_SCAN_TIME_STEP, scan_common_name, pbar2)
```

**Why one at a time?**
1. **Thermal management**: Prevents overheating (16 LEDs pulsing at 1 MHz = significant power)
2. **Crosstalk prevention**: No photons leak from one SiPM to neighbors
3. **Clean spectra**: Each measurement is independent, no interference
4. **Diagnostic capability**: Can identify bad SiPMs individually

#### 5. Data Acquisition Function (Lines 110-134)

```python
def RunAcquisitionEnergySpectrum(sdk, integrate_time, scan_common_name, pbar2):
    # Critical: Wait for voltage and LED to stabilize
    time.sleep(3)

    # Double reset to ensure clean accumulator state
    sdk.execute_cmd("reset_darkcount_spectra")
    sdk.execute_cmd("reset_darkcount_spectra")

    # Integrate (accumulate PE events)
    pbar2.reset()
    for i in range(integrate_time):
        time.sleep(1)
        pbar2.update(1)

    # Read accumulated histogram
    spectra_a = sdk.read_data("get_darkcount_spectra")

    # Save to CSV: transpose so rows = ADC channel, cols = digitizer channel
    with open(scan_common_name + "_spectra_A.csv", 'w', newline='\\n') as f:
        csv_writer = csv.writer(f)
        for row in zip(*spectra_a):  # Transpose operation
            csv_writer.writerow(row)
```

**CSV File Structure**:
```
File: _LEFT_PULSER_0_58.0_spectra_A.csv

ADC_ch | Dgtz_0 | Dgtz_1 | Dgtz_2 | ... | Dgtz_7
-------|--------|--------|--------|-----|-------
   0   |   12   |    5   |    8   | ... |    3
   1   |   15   |    7   |   10   | ... |    6
  ...  |  ...   |  ...   |  ...   | ... |  ...
  120  |  450   |   30   |   25   | ... |   15   ← Noise pedestal
  ...  |  ...   |  ...   |  ...   | ... |  ...
  150  | 1200   |   20   |   18   | ... |   12   ← Single-PE peak (Dgtz_0 enabled)
  ...  |  ...   |  ...   |  ...   | ... |  ...
```



### MATHEMATICAL ANALYSIS (Offline Processing)

After data collection, a separate script (calibration-hv-fit-calibfile.py) analyzes the spectra.

#### Peak Finding Algorithm

For each SiPM at each voltage:

```python
# Load spectrum
spectrum = np.loadtxt(f"_LEFT_PULSER_0_{voltage}_spectra_A.csv", delimiter=',')
sipm_spectrum = spectrum[:, 0]  # Column 0 = digitizer channel 0

# Find single-PE peak (usually the first major peak above baseline)
# Method 1: Simple maximum in expected range
baseline_end = 130  # After noise pedestal
search_window = baseline_end + np.arange(0, 200)  # Look in next 200 ADC bins
peak_position = search_window[np.argmax(sipm_spectrum[search_window])]

# Method 2: Scipy peak finder (more robust)
from scipy.signal import find_peaks
peaks, properties = find_peaks(sipm_spectrum[baseline_end:],
                                height=100,      # Minimum peak height
                                distance=15,     # Peaks at least 15 ADC apart
                                prominence=50)   # Peak prominence above baseline

single_PE_peak = peaks[0] + baseline_end  # First peak = 1 PE
```

#### Linear Regression

Collect data points across all voltages:

```python
# Example data for SiPM LEFT 0
V_data = [56.0, 56.5, 57.0, 57.5, 58.0, 58.5, 59.0, 59.5, 60.0, 60.5, 61.0]
PE_data = [105,  115,  125,  135,  145,  155,  165,  175,  185,  195,  205]

# Linear fit: PE_position = m × V_bias + q
from scipy.stats import linregress
m, q, r_value, p_value, std_err = linregress(V_data, PE_data)

# Result example:
# m = 20.0 ADC/V  (slope)
# q = -1015 ADC   (intercept)
# R² = 0.9998     (fit quality)

# Interpretation:
# For every 1V increase in bias voltage, PE peak moves by 20 ADC counts
```

#### Calculate Optimal Voltage

```python
PE_TARGET = 20  # Want single-PE peak at 20 ADC

# Solve: 20 = m × V_optimal + q
V_optimal = (PE_TARGET - q) / m
V_optimal = (20 - (-1015)) / 20.0
V_optimal = 1035 / 20.0
V_optimal = 51.75V

# Sanity check:
# At 51.75V, PE position = 20.0 × 51.75 + (-1015) = 1035 - 1015 = 20 ADC ✓
```

**Physical Interpretation**:
- Slope (m = 20 ADC/V): This SiPM's gain increases by 20 ADC per additional volt
- Intercept (q = -1015): Extrapolated PE position at 0V (meaningless, far below breakdown)
- Optimal voltage (51.75V): At this voltage, gain is exactly PE_TARGET ADC/PE

### NUMERICAL EXAMPLE - Complete Walkthrough

**SiPM: LEFT 0 on Module B/C**

**Step 1: Data Collection**
```
Voltage | Acquisition | Peak Position (ADC) | Raw File
--------|-------------|---------------------|-----------------------------------
 56.0V  | 5 seconds   |       105           | _LEFT_PULSER_0_56.0_spectra_A.csv
 56.5V  | 5 seconds   |       115           | _LEFT_PULSER_0_56.5_spectra_A.csv
 57.0V  | 5 seconds   |       125           | _LEFT_PULSER_0_57.0_spectra_A.csv
 57.5V  | 5 seconds   |       135           | _LEFT_PULSER_0_57.5_spectra_A.csv
 58.0V  | 5 seconds   |       145           | _LEFT_PULSER_0_58.0_spectra_A.csv
 58.5V  | 5 seconds   |       155           | _LEFT_PULSER_0_58.5_spectra_A.csv
 59.0V  | 5 seconds   |       165           | _LEFT_PULSER_0_59.0_spectra_A.csv
 59.5V  | 5 seconds   |       175           | _LEFT_PULSER_0_59.5_spectra_A.csv
 60.0V  | 5 seconds   |       185           | _LEFT_PULSER_0_60.0_spectra_A.csv
 60.5V  | 5 seconds   |       195           | _LEFT_PULSER_0_60.5_spectra_A.csv
 61.0V  | 5 seconds   |       205           | _LEFT_PULSER_0_61.0_spectra_A.csv

Total collection time: 11 voltages × 5 sec = 55 seconds (for this one SiPM)
```

**Step 2: Linear Regression**
```python
from scipy.stats import linregress
import numpy as np

V = np.array([56.0, 56.5, 57.0, 57.5, 58.0, 58.5, 59.0, 59.5, 60.0, 60.5, 61.0])
PE = np.array([105, 115, 125, 135, 145, 155, 165, 175, 185, 195, 205])

m, q, r, p, stderr = linregress(V, PE)

print(f"Slope (m): {m:.2f} ADC/V")
print(f"Intercept (q): {q:.2f} ADC")
print(f"R²: {r**2:.6f}")

Output:
Slope (m): 20.00 ADC/V
Intercept (q): -1015.00 ADC
R²: 1.000000
```

**Step 3: Calculate Optimal Voltage**
```python
PE_TARGET = 20  # ADC counts per photoelectron

V_optimal = (PE_TARGET - q) / m
V_optimal = (20 - (-1015)) / 20.0
V_optimal = 1035 / 20.0
V_optimal = 51.75V

print(f"Optimal bias voltage: {V_optimal:.2f}V")

# Verify:
PE_at_optimal = m * V_optimal + q
print(f"PE position at {V_optimal:.2f}V: {PE_at_optimal:.1f} ADC")

Output:
Optimal bias voltage: 51.75V
PE position at 51.75V: 20.0 ADC ✓
```

**Step 4: Save Calibration Parameters**
```python
calibration_data = {
    "SiPM_LEFT_0": {
        "m": 20.0,
        "q": -1015.0,
        "channel": 0,
        "position": "LEFT",
        "P": 20,
        "V_optimal": 51.75
    }
}

# This goes into unified_calibration.json
```

### PARAMETERS (from calibration_settings.py)

- **V1**: List of bias voltages to scan
  ```python
  V1 = [56.0, 56.5, 57.0, 57.5, 58.0, 58.5, 59.0, 59.5, 60.0, 60.5, 61.0]
  ```
  Typical range: 5-6V span around expected operating point

- **HV_SCAN_TIME_STEP**: Integration time per voltage point
  - Default: 5 seconds
  - Trade-off: Longer = better statistics, but longer total time
  - Total time: len(V1) × 16 SiPMs × HV_SCAN_TIME_STEP
  - Example: 11 voltages × 16 SiPMs × 5s = 880 seconds (~15 minutes)

- **PE_TARGET**: Target single-PE gain
  - Default: 20 ADC/PE
  - Rationale: Good signal-to-noise while leaving dynamic range
  - Range: 15-30 ADC/PE typical

- **MAIN_HV**: Safety limit for maximum voltage
  - Module A: 59.9V (older SiPMs, MUST NOT EXCEED!)
  - Modules B/C: 62.0V (newer SiPMs, higher breakdown voltage)

- **V_cM, V_cQ**: Module correction factors
  - Module A: V_cM=1.0, V_cQ=0.0
  - Modules B/C: V_cM=1.0, V_cQ=2.5
  - **NEVER CHANGE THESE** (hardware-dependent)

### KEY ASSUMPTIONS

1. **Linear SiPM Response Above Breakdown**:
   - Below breakdown (V_br ~ 53V): Exponential, chaotic
   - Above breakdown: Linear gain vs voltage
   - Assumption valid for V > V_br + 2V

2. **Temperature Stability**:
   - SiPM gain: ~50 mV/°C temperature coefficient
   - Room must be temperature-controlled
   - Wait 30 min after power-on for thermal equilibrium

3. **Previous Calibrations Valid**:
   - Offset calibration → baseline at TARGET_OFFSET
   - Pulser calibration → LED voltages optimized
   - If these are wrong, HV calibration inherits errors

4. **LED Stability**:
   - LED output doesn't drift during 15-minute scan
   - Temperature-stable LED driver circuit

5. **No Saturation**:
   - Spectra don't saturate at max ADC (4095)
   - If PE_TARGET too high, may hit ADC limit

### OUTPUT FILES

**Data Files** (in `run_hv/` directory):

For each SiPM and voltage:
```
_LEFT_PULSER_0_56.0_spectra_A.csv
_LEFT_PULSER_0_56.5_spectra_A.csv
...
_RIGHT_PULSER_7_61.0_spectra_A.csv
```

Total files: 16 SiPMs × len(V1) voltages
- Example: 16 × 11 = 176 CSV files

**CSV Format** (each file):
```
8 columns (one per digitizer channel)
4096 rows (one per ADC bin)

Only the enabled SiPM's column has signal, others show dark counts
```

**Calibration Output** (from offline analysis):
```
unified_calibration.json (or similar):
{
  "SiPM_LEFT_0": {"m": 20.0, "q": -1015, "V_opt": 51.75, ...},
  "SiPM_LEFT_1": {"m": 19.5, "q": -1005, "V_opt": 52.05, ...},
  ...
  "SiPM_RIGHT_7": {"m": 20.8, "q": -1025, "V_opt": 51.50, ...}
}
```

### TYPICAL CALIBRATION VALUES

- **Voltage range scanned**: 56-61V (module B/C), 54-59V (module A)
- **Slope (m)**: 15-25 ADC/V (typical SiPM at room temperature)
- **Optimal voltage**: 57-59V (module B/C), 55-57V (module A)
- **R² of fit**: > 0.998 (should be highly linear)
- **SiPM-to-SiPM variation**: ±0.5V (component tolerances)
- **Total calibration time**: ~15-20 minutes for 11 voltages

### WHY THIS ALGORITHM WORKS

1. **Voltage Sweep**: Covers full operating range to ensure valid linear fit
2. **Dynamic Trigger Adjustment**: Adapts to gain changes at each voltage (critical!)
3. **Sequential Measurement**: Prevents thermal and optical crosstalk between SiPMs
4. **Linear Fitting**: SiPM gain is linear above breakdown voltage (physics-based)
5. **Module Correction**: Accounts for hardware V_cQ offset automatically
6. **Offline Analysis**: Allows manual QC and outlier rejection
7. **Long Integration**: 5s per point → good statistics, clear PE peaks

### COMMON ISSUES AND SOLUTIONS

| Issue | Symptom | Solution |
|-------|---------|----------|
| **No PE peaks** | Flat spectrum, no structure | Check: Pulser enabled? LED voltage correct? SiPM enabled? |
| **Peaks too low** | All PE peaks < 10 ADC | Voltage too low, increase V1 range or check V_cQ setting |
| **Peaks too high** | PE peaks > 100 ADC, saturation | Voltage too high, reduce V1 range or increase PE_TARGET |
| **Non-linear fit** | R² < 0.95 | Voltage range includes breakdown region, narrow to higher voltages only |
| **Shifted baseline** | Baseline not at 120 ADC | Re-run offset calibration |
| **Missing data files** | Some CSV files not created | Check disk space, file permissions, SDK connection |
| **Temperature drift** | PE positions shift during scan | Wait for thermal equilibrium, reduce room temp variations |
| **Outlier voltage** | One point far from linear fit | Verify that voltage was correctly applied, may exclude from fit |
| **Different V_opt per SiPM** | V_optimal varies by > 2V | Normal (component variation), use per-SiPM values |

### INTEGRATION WITH OTHER CALIBRATIONS

HV calibration is **step 3** in the calibration chain:

```
1. Offset Calibration → baseline at 120 ADC
   ↓ (provides STAVE_OFFSET_SETTINGS[8])

2. Pulser Calibration → optimal LED voltage
   ↓ (provides PULSER_LEFT[8], PULSER_RIGHT[8])

3. HV Calibration → optimal bias voltage for PE_TARGET gain
   ↓ (provides m, q, V_optimal for each SiPM)

4. Unified Calibration → combine all into single JSON file
   (create-unified-calibration.py)
```

**Final JSON Structure**:
```json
{
  "PID": "ABC123",
  "module_type": "B",
  "timestamp": "2025-01-10T15:30:00",
  "channels": {
    "0": {"offset": 0.658},
    "1": {"offset": 0.662},
    ...
  },
  "sipm": {
    "LEFT_0": {
      "pulser_voltage": 1830,
      "pulser_ladder": 7,
      "hv_m": 20.0,
      "hv_q": -1015.0,
      "hv_optimal": 51.75,
      "channel": 0,
      "position": "LEFT"
    },
    ...
  }
}
```

This unified calibration file can be loaded into the GUI to apply all calibrated parameters at once.

