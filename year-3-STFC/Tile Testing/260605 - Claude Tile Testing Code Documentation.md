---
tags:
  - note
  - "#scintillating-tiles"
  - super-musr
created: 2026-05-05
---
# Tags: 
[[scintillating tiles]]
[[super-musr]]

**Note:** This is written by Claude. I have read the documentation to check it is correct, but AI can make mistakes. 

# PHS Analysis Code — Full Documentation

**File:** `phs_analysis.py`  
**Purpose:** Automated analysis of Pulse Height Spectra (PHS) from scintillating tile detector measurements. Loads raw data files, smooths and finds peaks, fits them with polynomial and Gaussian models, integrates the spectrum over a user-defined voltage window, and saves summary results to CSV.

---

## Table of Contents

1. [Background and Context](#1-background-and-context)
2. [Dependencies](#2-dependencies)
3. [File Format Expectations](#3-file-format-expectations)
4. [How to Run the Code](#4-how-to-run-the-code)
5. [Configuration Parameters](#5-configuration-parameters)
6. [Function Reference](#6-function-reference)
   - [polynomial_2nd_order](#polynomial_2nd_order)
   - [gaussian](#gaussian)
   - [calculate_peak_statistics](#calculate_peak_statistics)
   - [extract_id_from_filename](#extract_id_from_filename)
   - [read_set_file](#read_set_file)
   - [format_integration_info](#format_integration_info)
   - [parse_runtime_to_seconds](#parse_runtime_to_seconds)
   - [integrate_counts](#integrate_counts)
   - [integrate_counts_error](#integrate_counts_error)
   - [extract_channel_names](#extract_channel_names)
   - [load_phs_file](#load_phs_file)
   - [save_plot_data_to_csv](#save_plot_data_to_csv)
   - [analyze_all_peaks](#analyze_all_peaks)
   - [create_phs_overlay](#create_phs_overlay)
   - [find_phs_files](#find_phs_files)
   - [process_phs_folder](#process_phs_folder)
7. [Output Files](#7-output-files)
8. [The Maths Explained](#8-the-maths-explained)
9. [End-to-End Data Flow](#9-end-to-end-data-flow)
10. [Common Issues and How to Fix Them](#10-common-issues-and-how-to-fix-them)

---

## 1. Background and Context

A **Pulse Height Spectrum (PHS)** records how many detector pulses (counts) were measured at each output voltage level from a photomultiplier tube (PMT). When a scintillating tile is struck by a neutron or gamma ray, it emits light, the PMT converts this to an electrical pulse, and the pulse height (voltage) is recorded. By histogramming thousands of these pulses you get a spectrum with characteristic peaks corresponding to different particle energies or interaction types.

This code automates three things that a physicist would otherwise do by hand for every data file:

1. **Find the peaks** in the spectrum.
2. **Fit each peak** with both a second-order polynomial (parabola) and a Gaussian to extract the peak voltage position and its uncertainty.
3. **Integrate** the spectrum over a user-specified voltage window (e.g. the neutron capture peak region) to get a total count rate.

It is designed to process a whole folder of files in one call, handling both single-channel and multi-channel data, and optionally normalising everything by measurement runtime so that runs of different lengths can be compared fairly.

---

## 2. Dependencies

All standard scientific Python libraries. Install with pip if not already present:

```
pip install numpy matplotlib scipy pandas
```

| Library | What it is used for |
|---|---|
| `numpy` | Array maths throughout |
| `matplotlib` | Generating all plots |
| `scipy.signal.find_peaks` | Automated peak detection on the smoothed spectrum |
| `scipy.signal.savgol_filter` | Savitzky-Golay smoothing of raw spectra before peak detection |
| `scipy.optimize.curve_fit` | Least-squares fitting of Gaussian and polynomial models to peak regions |
| `pandas` | Reading tab-separated data files, building the output summary table |
| `pathlib.Path` | Clean cross-platform file path handling |
| `glob`, `os` | Finding files in folders, creating directories |
| `re` | Regular expressions used in runtime string parsing |
| `datetime` | Timestamps on output filenames |

---

## 3. File Format Expectations

### Data files (`.txt` or `.dat`)

Tab-separated. The first row is a header.

**Single-channel mode** (`multi_channel=False`):

- Columns used depend on the `tile_30mm` flag:
  - `tile_30mm=True` → column 0 = Voltage, column 1 = Counts (first two columns)
  - `tile_30mm=False` → column 4 = Voltage, column 5 = Counts (fifth and sixth columns)

**Multi-channel mode** (`multi_channel=True`):

The header row is expected to follow this pattern, with channel pairs separated by tabs **this is the exact format by .dat files from the picoscope**:

```
Volts:Ch_A    Counts:Ch_A    Volts:Ch_C    Counts:Ch_C    Volts:Ch_A+C    Counts:Ch_A+C
```

Each channel appears as a `Volts:ChannelName` / `Counts:ChannelName` pair. The code discovers channels automatically by reading column headers that start with `Counts:`.

### Settings files (`.set`)

Each data file should have a companion settings file with the **same name but `.set` extension** in the same folder. The code reads key=value lines from it. Relevant keys:

| Key | What it stores |
|---|---|
| `RunTime` | How long the measurement ran (used for normalisation) |
| `StartDateTime` | When the measurement started (metadata only, written to CSV) |
| `IntegrationTime` | Hardware integration time setting |
| `IsIntegrationEnabled` | `True` or `False` |
| `ChanFullScaleRange[1]` | Full-scale voltage range for channel 1 (used to calculate mV per division) |
| `ChanFullScaleRange[3]` | Full-scale voltage range for channel 3 |
| `TriggerLevel[1]` | Trigger threshold for channel 1 (metadata, written to plot) |
| `TriggerLevel[3]` | Trigger threshold for channel 3 |

If no `.set` file is found, the code prints a warning and continues with `None` values for all metadata fields.

### Filename convention

Files are expected to contain the tile ID embedded in the filename, formatted as `id<number>_`. For example:

```
tile_id042_run1.txt  →  ID extracted: "042"
measurement_id7_250601.dat  →  ID extracted: "7"
```

This ID is used to group files in the overlay plot and appears in the summary CSV.

---

## 4. How to Run the Code

Edit the two path variables at the bottom of the file and run it directly:

```python
if __name__ == "__main__":
    folder_path = r"path\to\your\data\folder"
    custom_save_path = r"path\to\where\you\want\results"

process_phs_folder(
    folder_path,
    save_results=True,
    save_plots=True,
    save_csv=True,
    custom_save_path=custom_save_path,
    normalise=True,
    phs_overlay=True,
    multi_channel=True,
    tile_30mm=False,
    integration_lower=0.02,
    integration_upper=0.20
)
```

Run from the command line:

```bash
python phs_analysis.py
```

Or import and call `process_phs_folder()` directly from another script or Jupyter notebook.

---

## 5. Configuration Parameters

These are the parameters passed to `process_phs_folder()`. They control every major behaviour of the analysis.

| Parameter | Type | Default | What it does |
|---|---|---|---|
| `folder_path` | `str` | — | **Required.** Path to the folder containing your `.txt`/`.dat` data files. |
| `save_results` | `bool` | `True` | Save the peak summary table as a CSV file. |
| `save_plots` | `bool` | `False` | Save each individual spectrum plot as a PNG. |
| `save_csv` | `bool` | `False` | Save the raw + smoothed data for each spectrum as a per-file CSV. |
| `custom_save_path` | `str` | `None` | Where to save outputs. Defaults to `folder_path` if not set. |
| `normalise` | `bool` | `True` | Divide counts by runtime (in seconds) to get counts/second. Requires a valid `.set` file with `RunTime`. |
| `phs_overlay` | `bool` | `False` | Generate a single overlay plot of all spectra on one set of axes. |
| `multi_channel` | `bool` | `False` | Read all channels from the file header instead of a fixed column pair. |
| `tile_30mm` | `bool` | `True` | If `False`, reads voltage/counts from columns 4 and 5 instead of 0 and 1. Relevant for the 210 mm tile rig data format. |
| `integration_lower` | `float` | `None` | Lower voltage bound for spectrum integration. No integration if both bounds are `None`. |
| `integration_upper` | `float` | `None` | Upper voltage bound for spectrum integration. |

---

## 6. Function Reference

The functions are listed in the order they appear in the file, grouped by their role.

---

### `polynomial_2nd_order`

```python
def polynomial_2nd_order(x, a, b, c)
```

**What it does:** Evaluates a second-order polynomial (parabola) at position `x`.

**Formula:** `y = a·x² + b·x + c`

**Used by:** `analyze_all_peaks` — passed to `scipy.optimize.curve_fit` as the model function when fitting around each detected peak.

**Why a parabola?** Near a smooth peak, any well-behaved function looks approximately parabolic. The fitted parabola's vertex gives a sub-bin-resolution estimate of the peak position. A negative `a` gives a downward-opening parabola (a peak); a positive `a` gives a minimum (a trough). The code checks the sign of `a` when interpreting results.

---

### `gaussian`

```python
def gaussian(x, amplitude, mean, sigma)
```

**What it does:** Evaluates a Gaussian (normal distribution) curve at position `x`.

**Formula:** `y = amplitude · exp(-(x - mean)² / (2·sigma²))`

**Parameters:**
- `amplitude` — the peak height
- `mean` — the centre position (the quantity we want)
- `sigma` — the standard deviation (controls width)

**Used by:** `calculate_peak_statistics` — passed to `curve_fit` for the Gaussian fit of each peak region.

---

### `calculate_peak_statistics`

```python
def calculate_peak_statistics(x_data, y_data) → (mean, mean_error)
```

**What it does:** Fits a Gaussian to a region of the spectrum and extracts the peak centre and its uncertainty.

**Inputs:**
- `x_data` — voltage values in the fitting window
- `y_data` — count values in the fitting window (already normalised if normalisation is on)

**Returns:** `(mean, mean_error)` where `mean` is the fitted peak voltage and `mean_error` is the one-sigma uncertainty extracted from the diagonal of the covariance matrix returned by `curve_fit`. Returns `(None, None)` if the fit fails or there are fewer than 3 valid points.

**Initial guesses for the fit:**
- Amplitude: maximum y value in the window
- Mean: x position of that maximum
- Sigma: one quarter of the window width (a rough but usually workable starting point)

**Why use a Gaussian for PHS peaks?** Detector peaks in pulse height spectra are broadened by Poisson statistics in the photoelectron production process and electronic noise — both of which produce approximately Gaussian distributions. The Gaussian mean is therefore a physically motivated estimator of the true peak position.

---

### `extract_id_from_filename`

```python
def extract_id_from_filename(filename) → str or None
```

**What it does:** Pulls the tile ID out of a filename by looking for the substring `id` and returning everything between it and the next underscore (or the file extension if there is no underscore).

**Examples:**
```
tile_id042_run1.txt  →  "042"
scan_id7.dat         →  "7"
data_file.txt        →  None  (no 'id' found)
```

**Case-insensitive:** Works on `ID`, `Id`, and `id`.

---

### `read_set_file`

```python
def read_set_file(data_file_path) → (runtime, start_datetime, integration_time,
                                     is_integration_enabled, division, trig_1, trig_3)
```

**What it does:** Looks for a `.set` file with the same base name as the data file, in the same directory. Reads it line by line looking for `Key=Value` pairs and extracts the fields listed in the table in section 3.

**Division logic:** If both `ChanFullScaleRange[1]` and `ChanFullScaleRange[3]` are present, channel 1's value is used. If only one is present, that one is used with a warning. If neither is found, `division` defaults to `1.0`. The displayed "mV per division" on plots is `division × 100`.

**Returns:** A 7-tuple. Any field not found in the `.set` file is returned as `None`.

---

### `format_integration_info`

```python
def format_integration_info(integration_time, is_integration_enabled) → str
```

**What it does:** Produces a human-readable string describing the hardware integration setting, suitable for printing on a plot. For example: `"Integration: 2.50e-07s"` or `"Integration: OFF"`.

**Used by:** `analyze_all_peaks` to build the info text box on each plot.

---

### `parse_runtime_to_seconds`

```python
def parse_runtime_to_seconds(runtime_str) → float or None
```

**What it does:** Converts a runtime string from the `.set` file into a plain number of seconds.

**Handles two formats:**
- `HH:MM:SS` (e.g. `"0:05:30"` → `330.0` seconds)
- `MM:SS` (e.g. `"5:30"` → `330.0` seconds)
- A plain number (e.g. `"330"` → `330.0`)

**Fallback:** If normal parsing fails, uses a regular expression to extract the first numeric value from the string. Returns `None` if no number can be found at all, which causes normalisation to be skipped gracefully.

---

### `integrate_counts`

```python
def integrate_counts(x, y, lower_bound=None, upper_bound=None) → float or None
```

**What it does:** Numerically integrates the spectrum `y` over the voltage range `[lower_bound, upper_bound]` using the trapezoidal rule (`numpy.trapz`).

**Inputs:**
- `x` — voltage array
- `y` — counts (or counts/second if already normalised)
- `lower_bound`, `upper_bound` — voltage limits; default to the full range of `x` if `None`

**Returns:** The area under the curve (units: counts·V or counts/second·V). Returns `None` if no data points fall within the bounds.

**Physical meaning:** This integral is proportional to the total number of detected events within the voltage window, giving a single number that can be compared across different tiles or measurement conditions.

---

### `integrate_counts_error`

```python
def integrate_counts_error(x, y_raw, lower_bound=None, upper_bound=None,
                           runtime_seconds=None) → float or None
```

**What it does:** Computes the one-sigma statistical uncertainty on the trapezoidal integral, propagating Poisson counting statistics through the numerical integration.

**The maths (see also section 8):**

For a Poisson process, the variance on a raw count bin `nᵢ` is equal to `nᵢ` itself. The trapezoidal rule weights each bin `i` by a factor `wᵢ` (half the sum of the widths of its left and right neighbouring trapezoid intervals). By standard error propagation:

```
σ(I) = sqrt( Σᵢ wᵢ² · Var(yᵢ) )
```

If the spectrum is normalised by runtime `T`, each bin `yᵢ = nᵢ / T` and `Var(yᵢ) = nᵢ / T²`.

**Key design decision:** This function always takes `y_raw` (the raw, un-normalised counts) to ensure the Poisson assumption is applied to integer counts, then scales the resulting error by `1/T` to match the units of a normalised integral. Applying Poisson statistics to already-normalised (non-integer) values would give incorrect results.

**Returns:** One-sigma error on the integral, in the same units as `integrate_counts`. Returns `None` if there are fewer than 2 data points in the window.

---

### `extract_channel_names`

```python
def extract_channel_names(header_line) → list[str]
```

**What it does:** Parses the first line of a multi-channel data file and returns a list of channel names in the order they appear.

**Expects** a tab-separated header where channel names appear in columns labelled `Counts:ChannelName`. For example:

```
Volts:Ch_A    Counts:Ch_A    Volts:Ch_C    Counts:Ch_C
```

Returns: `['Ch_A', 'Ch_C']`

---

### `load_phs_file`

```python
def load_phs_file(file_path, multi_channel=False, tile_30mm=True)
```

**What it does:** Reads a `.txt` or `.dat` data file and returns the voltage and counts arrays.

**Single-channel mode** returns `(x, y)` — two 1D numpy arrays.

**Multi-channel mode** returns `(channels_data, channel_names)` where:
- `channels_data` is a dictionary keyed by channel name, each containing `{'x': array, 'y': array}`
- `channel_names` is a list of channel names in order

**Column selection (`tile_30mm`):**
- `True` → columns 0 (voltage) and 1 (counts) — used for 30 mm tile data
- `False` → columns 4 (voltage) and 5 (counts) — used for 210 mm tile rig data where additional pre-analysis columns are present

**Robustness:** NaN values are stripped from the output. If the file has fewer columns than expected (wrong `tile_30mm` setting), a clear warning is printed and `None` is returned so the caller can skip the file.

---

### `save_plot_data_to_csv`

```python
def save_plot_data_to_csv(x, y, y_smooth, peaks, save_path, file_name,
                          channel_name=None, normalise=True, runtime_seconds=None)
```

**What it does:** Saves the per-file spectrum data used for plotting to a CSV file. This is useful for re-plotting in other tools (e.g. Origin, Excel) without re-running the analysis.

**Output columns:**

| Column | Contents |
|---|---|
| `Voltage` | The voltage axis values |
| `Counts_Raw` | The raw (or normalised) count values, before smoothing |
| `Counts_Smoothed` | The Savitzky-Golay smoothed values used for peak detection |
| `Is_Peak` | `1` if this bin was identified as a peak, `0` otherwise |

**Filename format:** `<original_name>_<YYMMDD_HHMMSS>_<normalised/raw>[_<channel>]_plot_data.csv`

---

### `analyze_all_peaks`

```python
def analyze_all_peaks(x, y, window=10, poly=3, prominence=0.05, ...) 
    → (all_peak_info, integrated_counts, normalised_used, integrated_counts_error)
```

**What it does:** This is the core analysis function. Given a single spectrum (one `x`, `y` pair), it:

1. **Normalises** the counts by runtime if `normalise=True` and runtime is available.
2. **Smooths** the spectrum using a Savitzky-Golay filter.
3. **Detects all peaks** on the smoothed spectrum using `scipy.signal.find_peaks`.
4. **Fits each peak** with both a second-order polynomial and a Gaussian in a local window around the peak.
5. **Calculates the peak position and error** from both fits.
6. **Integrates** the spectrum over the specified voltage bounds.
7. **Produces and optionally saves a plot** showing the raw spectrum, smoothed spectrum, polynomial fits, and marked peak positions.
8. **Returns** a list of dictionaries (one per peak) plus the integration results.

**Smoothing parameters:**
- `window=10` — the number of data points in the Savitzky-Golay filter window. Larger values → smoother but may wash out narrow peaks.
- `poly=3` — the polynomial order used inside the filter. 3 is standard.

**Peak detection thresholds:**
- `prominence=0.05` — a peak must be at least 5% of the spectrum maximum in height. Increase this (e.g. to `0.1`) to suppress noise peaks; decrease it to catch smaller features.
- `distance=len(y)//20` — peaks must be at least 5% of the spectrum width apart. Prevents one broad feature being counted as multiple peaks.

**Fitting window:** ±10% of the full voltage range around the detected peak position. So for a spectrum spanning 0 to 1 V, the fit window is ±0.1 V around each peak.

**Low-voltage exclusion:** Peaks at voltages ≤ 0.005 V are recorded in the output but skipped for fitting. This avoids fitting the large noise/pedestal feature that often sits near zero volts.

**Per-peak output dictionary keys:**

| Key | Meaning |
|---|---|
| `peak_number` | Index starting at 1 |
| `peak_x_poly` | Peak voltage from polynomial vertex formula `-b/(2a)` |
| `peak_x_poly_err` | One-sigma error on the polynomial peak position |
| `peak_x_data_mean` | Peak voltage from Gaussian fit mean |
| `peak_x_data_mean_err` | One-sigma error on the Gaussian mean |
| `peak_y` | Peak height (counts or counts/s) |
| `polynomial_a/b/c` | Fitted polynomial coefficients |
| `polynomial_a/b/c_err` | One-sigma errors on each coefficient |
| `num_data_points` | Number of data points in the fit window |

**Returns:** `(all_peak_info, integrated_counts, normalised_used, integrated_counts_error)`
- `all_peak_info` — list of dicts as above
- `integrated_counts` — float (or `None` if integration bounds not set)
- `normalised_used` — bool, `True` if runtime normalisation was applied
- `integrated_counts_error` — one-sigma error on `integrated_counts` (or `None`)

---

### `create_phs_overlay`

```python
def create_phs_overlay(spectra_data, save_path=None, normalise=True)
```

**What it does:** Produces a single figure with all spectra plotted together, one line per spectrum. Useful for a quick visual comparison of many tiles or runs.

**Colour assignment:** Each unique tile ID (extracted from the filename) is given a consistent colour using matplotlib's `tab20` colourmap (up to 20 IDs) or `hsv` (for more than 20). This means the same tile will always appear in the same colour even if spectra are processed in different orders.

**`spectra_data`** is a list of dictionaries, each built up during `process_phs_folder`:

```python
{
    'x': array,           # voltage axis
    'y': array,           # counts (already normalised if normalise=True)
    'filename': str,      # for the legend
    'runtime': str,       # for reference
    'channel': str,       # optional, for multi-channel
    'id': str             # tile ID for colour grouping
}
```

---

### `find_phs_files`

```python
def find_phs_files(folder_path) → list[str]
```

**What it does:** Searches `folder_path` for files with extensions `.txt`, `.csv`, `.dat`, or `.data` and returns a sorted list of their full paths. This is a simple glob scan — it does not recurse into subdirectories.

---

### `process_phs_folder`

```python
def process_phs_folder(folder_path, save_results=True, save_plots=False, save_csv=False,
                       custom_save_path=None, normalise=True, phs_overlay=False,
                       multi_channel=False, tile_30mm=True,
                       integration_lower=None, integration_upper=None)
```

**What it does:** The top-level entry point. Orchestrates the complete analysis of an entire folder of data files.

**Step-by-step flow:**

1. Call `find_phs_files` to get the list of data files.
2. For each file:
   a. Call `load_phs_file` to read the data.
   b. Call `read_set_file` to read the companion metadata.
   c. Optionally add the spectrum to `spectra_data` for the overlay plot.
   d. Call `analyze_all_peaks` to detect and fit peaks, integrate, and plot.
   e. Append one row per detected peak to the `results` list.
3. Optionally call `create_phs_overlay` with all collected spectra.
4. Save the `results` list as a summary CSV.
5. Print the summary table to the console.

**Multi-channel vs single-channel:** When `multi_channel=True`, steps 2a and 2d are repeated for each channel found in the file header, and a `Channel` column is added to the results.

**Output CSV filename format:**

```
PHS_All_Peaks_Summary_<YYMMDD_HHMMSS>_<normalised/raw>[_multichannel].csv
```

---

## 7. Output Files

Running `process_phs_folder` can produce up to four types of output file, depending on your settings:

### Summary CSV (`save_results=True`)

One row per detected peak across all files. Columns:

| Column | Description |
|---|---|
| `ID` | Tile ID parsed from filename |
| `File` | Source filename |
| `Channel` | Channel name (multi-channel mode only) |
| `Peak_Number` | Peak index (1 = first peak found from left) |
| `Peak_X_Gaussian` | Peak voltage from Gaussian fit (V) |
| `Peak_X_Gaussian_Err` | One-sigma error on Gaussian peak position (V) |
| `Peak_X_Poly` | Peak voltage from polynomial vertex (V) |
| `Peak_X_Poly_Err` | One-sigma error on polynomial peak position (V) |
| `Peak_Y` | Peak height in counts or counts/s |
| `Num_Data_Points` | Points used in the fit window |
| `Polynomial_a/b/c` | Fitted parabola coefficients |
| `Polynomial_a/b/c_err` | One-sigma errors on coefficients |
| `Runtime` | Runtime string from `.set` file |
| `StartDateTime` | Measurement start time from `.set` file |
| `IntegrationTime` | Hardware integration time from `.set` file |
| `IsIntegrationEnabled` | Boolean from `.set` file |
| `normalised` | `True` if runtime normalisation was applied |
| `Integrated_Counts` | Area under the spectrum in `[integration_lower, integration_upper]` |
| `Integrated_Counts_Error` | One-sigma Poisson error on the integrated counts |
| `Integration_Lower` | Lower voltage bound used for integration |
| `Integration_Upper` | Upper voltage bound used for integration |

### Per-file spectrum plots (`save_plots=True`)

One PNG per file (per channel in multi-channel mode). Shows:
- Raw or normalised spectrum (light blue/grey)
- Savitzky-Golay smoothed spectrum (blue)
- Polynomial fits around each peak (green dashed)
- Polynomial peak position markers (red circle)
- Gaussian peak position markers (purple square)
- Info box with metadata, integration results, and peak count

Filename format: `<original_name>_<YYMMDD_HHMMSS>_<normalised/raw>[_<channel>]_all_peaks_plot.png`

### Per-file data CSVs (`save_csv=True`)

One CSV per file containing the voltage axis, raw counts, smoothed counts, and a binary peak marker. Useful for re-plotting outside Python.

### Overlay plot (`phs_overlay=True`)

One PNG containing all spectra overlaid. Saved as `PHS_Spectra_Overlay_<timestamp>_<normalised/raw>.png`.

---

## 8. The Maths Explained

### Savitzky-Golay Smoothing

Raw PHS data is noisy. Before searching for peaks the spectrum is smoothed using a **Savitzky-Golay filter** (`scipy.signal.savgol_filter`). This fits a polynomial of order `poly` to a sliding window of `window` adjacent points and replaces the central point with the polynomial value. Unlike a simple moving average, it preserves peak heights and widths much better because it fits a local curve rather than averaging linearly.

### Peak Detection

`scipy.signal.find_peaks` scans the smoothed spectrum for local maxima subject to two constraints:

- **Height threshold:** `height = max(y_smooth) × prominence`. Any peak below 5% (default) of the global maximum is ignored — this removes small noise bumps.
- **Minimum separation:** `distance = len(y) // 20`. Peaks must be at least 5% of the total number of bins apart — this prevents one broad physical peak being split into multiple detections.

### Polynomial Peak Position

A parabola `y = ax² + bx + c` is fitted to the ±10% voltage window around each detected peak. The vertex of a parabola is at:

```
x_peak = -b / (2a)
```

The error is propagated from the fit covariance matrix using partial derivatives:

```
∂x_peak/∂a = b / (2a²)
∂x_peak/∂b = -1 / (2a)

σ²(x_peak) = (b/(2a²))² · σ²(a) + (1/(2a))² · σ²(b)
```

### Gaussian Peak Position

A Gaussian `y = A · exp(-(x-μ)² / (2σ²))` is fitted to the same window. The mean `μ` is the peak position. Its uncertainty `σ(μ)` comes directly from the square root of the (1,1) element of the covariance matrix returned by `curve_fit` — this is the standard least-squares error on the mean parameter.

### Trapezoidal Integration

The integral of the spectrum over `[lower_bound, upper_bound]` is computed using the **trapezoidal rule**:

```
I = Σₖ  0.5 · (yₖ + yₖ₊₁) · (xₖ₊₁ - xₖ)
```

This is equivalent to approximating the area under the curve as a series of trapezoids between adjacent data points. It is numerically exact for linear data and a good approximation for smoothly varying spectra with fine binning.

### Integration Error (Poisson Propagation)

Each raw count bin `nᵢ` is assumed to follow Poisson statistics, so `Var(nᵢ) = nᵢ`.

In the trapezoidal rule, each bin `i` contributes to the integral via a weight `wᵢ`:
- Edge bins: `wᵢ = 0.5 · dx` (one neighbour)
- Interior bins: `wᵢ = 0.5 · (dx_left + dx_right)` (two neighbours)

By the standard error propagation formula (assuming independent bins):

```
σ²(I) = Σᵢ wᵢ² · Var(yᵢ)
```

If normalised by runtime `T`, `yᵢ = nᵢ/T` and `Var(yᵢ) = nᵢ/T²`, giving:

```
σ(I_normalised) = sqrt( Σᵢ wᵢ² · nᵢ / T² )
```

This is always computed from the **raw counts** `nᵢ` before normalisation to ensure integer Poisson statistics are used.

---

## 9. End-to-End Data Flow

The diagram below traces what happens from calling `process_phs_folder` to the final CSV.

```
process_phs_folder()
│
├── find_phs_files()         ← glob scan of folder for .txt/.dat/.csv/.data
│
└── for each file:
    │
    ├── load_phs_file()      ← reads tab-separated data into numpy arrays
    │     │
    │     └── extract_channel_names()   ← (multi-channel mode only)
    │
    ├── read_set_file()      ← reads companion .set metadata file
    │
    ├── [build spectra_data for overlay]
    │
    └── analyze_all_peaks()  ← main analysis per spectrum
          │
          ├── parse_runtime_to_seconds()     ← convert runtime string to float
          ├── y = y / runtime_seconds        ← normalisation (if enabled)
          ├── savgol_filter()                ← smooth the spectrum
          ├── find_peaks()                   ← locate peak indices
          │
          ├── [for each peak]:
          │     ├── calculate_peak_statistics()  ← Gaussian fit → mean ± error
          │     └── curve_fit(polynomial_2nd_order)  ← parabola fit → vertex ± error
          │
          ├── integrate_counts()             ← trapezoidal area in voltage window
          ├── integrate_counts_error()       ← Poisson error on that area
          │
          ├── [build matplotlib figure]
          ├── save_plot_data_to_csv()        ← (if save_csv=True)
          └── plt.savefig()                  ← (if save_plots=True)
    
    └── results.append(one row per peak)

├── create_phs_overlay()     ← (if phs_overlay=True)
└── pd.DataFrame(results).to_csv()   ← summary CSV
```

---

## 10. Common Issues and How to Fix Them

### "No .set file found for ..."

The code looks for a file with the same name as the data file but with a `.set` extension, in the same directory. Check the directory and filename match exactly. If `.set` files don't exist, normalisation will be skipped and metadata columns in the CSV will be empty — the analysis still runs.

### "No peaks detected in data"

Usually one of three causes:
- The spectrum is flat or very noisy. Try reducing `prominence` (e.g. `prominence=0.02`).
- The `tile_30mm` flag is wrong, causing the wrong columns to be read (the voltage column gets read as counts and vice versa). Check your data file structure.
- The smoothing window `window` is too large, washing out genuine peaks. Try `window=5`.

### "Warning: Polynomial fit failed for peak N"

`curve_fit` failed to converge for that peak. This can happen if the peak region is very narrow (few data points) or asymmetric. The peak is still recorded in the output using the smoothed-data peak position, but `Peak_X_Poly` and `Peak_X_Poly_Err` will reflect the raw smoothed peak rather than a fit. The Gaussian result may still be valid.

### Peak position seems wrong or polynomial fit gives a vertex outside the window

This typically means the peak region is not well described by a parabola — for example, if the fit window catches a shoulder from an adjacent peak. Try reducing the fit window (currently hardcoded at ±10% of the voltage range in `analyze_all_peaks`). For future work, this could be made a configurable parameter.

### Wrong number of channels found

Check that the header line of your data file exactly follows the `Volts:ChName  Counts:ChName` pattern with tab separators. Leading/trailing spaces are stripped, but other formats will not be detected.

### `Integrated_Counts_Error` is `None` in the CSV

This happens when: (a) `integration_lower` and `integration_upper` are both `None` (integration not requested), or (b) normalisation was not applied (the code only integrates the normalised spectrum). Set both bounds and ensure a valid `.set` file with `RunTime` is present.

### Output files end up in the wrong folder

If `custom_save_path` is not set, outputs go to `folder_path`. Set `custom_save_path` explicitly to separate processed results from raw data.
