---
tags:
  - document
  - super-musr
  - coding
  - "#scintillating-tiles"
created: 2026-07-18
---
# PMT-Swap-Corrected Stud Light-Output Asymmetry

## 1. Overview

This code analyses paired 2D light-output histograms from a scintillator tile
test rig, with the goal of measuring whether one end of a tile (the "LHS
stud" or "RHS stud") produces systematically more light than the other.

The complication is that this can't be measured directly from a single
histogram, because the two PMT channels used to read out the tile are not
identical, and any per-channel gain difference looks exactly like a stud
asymmetry if you don't correct for it. The code handles this by taking two
measurements per tile with the channel-to-stud mapping deliberately swapped,
then combining them in a way that cancels the channel-dependent part and
keeps the stud-dependent part.

The script does three things:

1. `plot_2d_files` — plots the raw 2D histogram for every individual `.2D` file.
2. `quantify_stud_asymmetry` — pairs up each tile's LHS-run and RHS-run files,
   computes the swap-corrected LHS/RHS response, builds a swap-corrected
   *averaged* 2D histogram, and quantifies its asymmetry two different ways.
3. Everything is written out as PNGs and a summary CSV for use in the report.

---

## 2. Experimental Setup and Data Convention

- Two fixed hardware readout channels, referred to as **Channel B** and
  **Channel D**, feed into an ADC and are histogrammed against each other.
- Each tile has two physical ends: an **LHS stud** and an **RHS stud**.
- Each tile is measured **twice**:
  - **LHS run**: Channel B looks at the LHS stud, Channel D looks at the RHS
    stud.
  - **RHS run**: the mapping is swapped — Channel B looks at the RHS stud,
    Channel D looks at the LHS stud.
- Filenames follow the convention:

  ```
  {tile_length}_{tileID}_{LHS|RHS}_{run}_BvsD.2D
  ```

  e.g. `105mm_id005_LHS_run001_BvsD.2D`

- Each `.2D` file is a 2D count histogram loaded with `np.loadtxt`, with
  **rows = Channel D bin (y-axis)** and **columns = Channel B bin (x-axis)**.
  This matches how `plot_2d_files` calls `imshow(data)` with
  `xlabel = b_hand`, `ylabel = d_hand`.

---

## 3. The Problem: Channel Mismatch Masquerades as Stud Asymmetry

Channels B and D use physically different PMTs, which will never have
perfectly matched gain, HV response, or quantum efficiency. If you only ever
looked at one run — say, the LHS run, where B=LHS and D=RHS — then any
observed difference between the B-axis centroid and the D-axis centroid is a
mix of two effects that are impossible to separate from that single
measurement:

- a genuine difference in light output between the LHS and RHS stud, and
- a systematic gain/offset difference between channel B and channel D.

The swap measurement exists specifically to break this degeneracy.

---

## 4. Theory: Why Swapping and Averaging Cancels the Channel Effect

Model each channel's measured response as an approximately linear function of
the true light output it's looking at:

```
measured_response = gain_channel * true_light_output + offset_channel
```

where `gain_B ≠ gain_D` and `offset_B ≠ offset_D` in general (this is the
channel mismatch we want to remove). Let `L` and `R` denote the true light
output of the LHS and RHS stud respectively for a given tile.

**LHS run** (B looks at LHS, D looks at RHS):

```
B_LHSrun = gain_B * L + offset_B
D_LHSrun = gain_D * R + offset_D
```

**RHS run** (B looks at RHS, D looks at LHS):

```
B_RHSrun = gain_B * R + offset_B
D_RHSrun = gain_D * L + offset_D
```

Now form the swap-corrected estimates the same way the code does
(`compute_weighted_means` + the averaging in `quantify_stud_asymmetry`):

```
LHS_estimate = mean(B_LHSrun, D_RHSrun)
             = mean(gain_B*L + offset_B,  gain_D*L + offset_D)
             = L * (gain_B + gain_D)/2 + (offset_B + offset_D)/2

RHS_estimate = mean(D_LHSrun, B_RHSrun)
             = mean(gain_D*R + offset_D,  gain_B*R + offset_B)
             = R * (gain_B + gain_D)/2 + (offset_B + offset_D)/2
```

Both `LHS_estimate` and `RHS_estimate` now carry the **same** combined gain,
`(gain_B + gain_D)/2`, and the **same** combined offset,
`(offset_B + offset_D)/2` — the channel-specific mismatch has cancelled out
of the *comparison* between them entirely. Any remaining difference between
`LHS_estimate` and `RHS_estimate` must come from a genuine difference between
`L` and `R`, not from the channels.

**Where this breaks down:** this cancellation is exact under the linear
response model above. If the true channel response is nonlinear (e.g.
saturation at high light levels, threshold effects near the pedestal), the
cancellation is only approximate. The code comments note this was checked
against synthetic data with a known stud asymmetry and a known 20% channel
gain mismatch, and the swap-corrected estimate recovered the true stud values
to within noise — but a genuinely nonlinear channel response is a residual
systematic this method does not fully remove.

---

## 5. Extending the Correction to the Full 2D Distribution

Section 4 corrects the *centroid* (mean) of each channel's response. The same
idea is applied to the full 2D histogram, not just its mean, to build one
combined "averaged" plot per tile:

- In the **LHS run** file: columns = LHS stud, rows = RHS stud — already the
  orientation we want.
- In the **RHS run** file: columns = RHS stud, rows = LHS stud — the
  **transpose** of the orientation we want.

So the swap-corrected combination is:

```
averaged_2D = ( normalise(data_LHSrun) + normalise(data_RHSrun.T) ) / 2
```

with columns = LHS stud, rows = RHS stud throughout.

### Why normalise first

The two runs are not guaranteed to contain the same total number of events —
run time, trigger rate, and live time can all differ between them. If you
average the raw counts, whichever run happened to collect more statistics
dominates the combined histogram, which quietly reintroduces a run-dependent
bias into a quantity that's supposed to represent two equally-weighted
measurements.

`normalise_histogram(data)` divides a histogram by its own total count,
turning it into a probability distribution (bins sum to 1) before the
average is taken. This guarantees each run contributes exactly 50% of the
combined distribution's weight, regardless of its raw statistics.

**Worked check:** two histograms with genuinely different shapes (centroids
at bin 20 and bin 30) were combined with a 100× difference in total counts
between them. Averaging the raw counts pulled the combined centroid to 29.9
— almost entirely determined by whichever run had more statistics.
Normalizing first before averaging gave a combined centroid of exactly 25.0,
the true midpoint between the two runs, independent of their relative
statistics.

---

## 6. Quantifying Asymmetry on the Averaged Plot

Two complementary metrics are computed from `averaged_2D`.

### 6.1 Centroid-based asymmetry (`asymmetry_centroid`)

```
asymmetry_centroid = (LHS_estimate - RHS_estimate) / (LHS_estimate + RHS_estimate)
```

Bounded in `[-1, 1]`. Positive → LHS stud brighter on average; negative →
RHS stud brighter. This is a **first-moment** measure: it only compares the
two marginal means, so it's cheap to compute but can miss asymmetries in the
*shape* of the distribution (skew, correlated outliers, bimodality) that
don't move the mean very much.

### 6.2 Diagonal-split asymmetry (`asymmetry_diagonal`)

This uses the full shape of `averaged_2D`, not just its marginal means. Every
bin `(i, j)` in the averaged histogram (row `i` = RHS bin, column `j` = LHS
bin) sits either:

- below the perfect-correlation line `y = x`, if `j > i` (LHS read brighter
  for that event),
- above the line, if `i > j` (RHS read brighter for that event), or
- exactly on the line, if `i == j`.

Summing counts on each side gives:

```
asymmetry_diagonal = (N_LHS_brighter - N_RHS_brighter) / (N_LHS_brighter + N_RHS_brighter)
```

also bounded in `[-1, 1]`, with the same sign convention as
`asymmetry_centroid`. Counts sitting exactly on the diagonal
(`counts_on_diagonal`) are tracked separately and excluded from the ratio.

**Why have both:** the diagonal-split metric is sensitive to asymmetries the
centroid metric can miss, because it doesn't collapse the distribution down
to a single number before comparing sides — it looks at where every count
actually sits relative to `y = x`. In practice, if `asymmetry_centroid` and
`asymmetry_diagonal` disagree noticeably for a given tile, that's a sign the
underlying imbalance isn't a simple shift but something shape-related (skew,
a subpopulation of events on one side, etc.), and it's worth looking at the
difference-map plot for that tile directly.

### 6.3 Visualising *where* the asymmetry sits

`plot_asymmetry_difference_map` plots `averaged_2D - averaged_2D.T` with a
diverging colormap. This difference is exactly zero everywhere the
distribution is mirror-symmetric about `y = x`; it's positive where an LHS
brighter bin outweighs its RHS-brighter mirror bin, negative for the
reverse. This turns the single `asymmetry_diagonal` number into a spatial
map, showing whether the imbalance is spread uniformly across the light
range or concentrated at particular light levels.

---

## 7. Error and Uncertainty Considerations

**Statistical uncertainty.** Treating each event as either "LHS brighter" or
"RHS brighter" (ignoring on-diagonal ties) is a binomial process with
`N = N_LHS_brighter + N_RHS_brighter` trials and `p = N_LHS_brighter / N`.
Since `asymmetry_diagonal = 2p - 1`, standard binomial error propagation
gives an approximate statistical uncertainty of

```
sigma(asymmetry_diagonal) ≈ 2 * sqrt( p * (1-p) / N )
                           = 2 * sqrt(N_LHS_brighter * N_RHS_brighter) / N^{3/2}
```

This is **not currently computed by the code** — it's included here so it can
be added to the summary CSV if the report needs quoted uncertainties on the
asymmetry values, rather than bare point estimates. It also gives a quick
sanity check: if `N` is small for a given tile/run, don't over-interpret a
non-zero `asymmetry_diagonal` as a real effect without checking this against
its statistical spread.

**Systematic effects not corrected for:**

- *Nonlinear channel response.* The swap-and-average cancellation in
  Section 4 is exact for a linear response and only approximate otherwise
  (Section 4, "where this breaks down").
- *Drift between the two runs.* The method assumes the channel gain/offset
  are the same during the LHS run and the RHS run. If the PMT gain drifted
  (temperature, HV instability) between the two runs, the cancellation is
  incomplete — the two runs need to have been taken close enough together
  in time/conditions for this assumption to hold.
- *Binning/quantisation.* Both asymmetry metrics work on binned histograms;
  the diagonal-split test in particular can be sensitive to how counts land
  exactly on `i == j` for coarse binning, which is why those on-diagonal
  counts are reported and excluded rather than arbitrarily assigned to one
  side.
- *Shape mismatch between runs before normalisation.* Normalising each run
  to a probability distribution (Section 5) removes any *bias from unequal
  statistics*, but doesn't correct for a run whose 2D shape is itself
  distorted (e.g. a partially blocked scan, different beam/source position).
  That would need to be caught by inspecting the individual per-run plots
  from `plot_2d_files`.

---

## 8. Code Reference

| Function | Purpose |
|---|---|
| `plot_2d_files(folder_path, save_path)` | Plots every raw `.2D` file individually as a heatmap with a `y=x` reference line. |
| `compute_weighted_means(data)` | Counts-weighted centroid of a 2D histogram along each axis. |
| `compute_diagonal_asymmetry(data)` | Splits a 2D histogram's counts across the `y=x` line and returns the diagonal-split asymmetry metric. |
| `normalise_histogram(data)` | Normalises a 2D histogram to sum to 1 (probability distribution). |
| `parse_2d_filename(file_name)` | Parses `{tile_length}_{tileID}_{side}_{run}_BvsD.2D` into its components. |
| `plot_averaged_heatmap(...)` | Plots the swap-corrected, normalised, averaged 2D distribution for one tile/run pair. |
| `plot_asymmetry_difference_map(...)` | Plots `averaged - averaged.T`, showing where any asymmetry is concentrated. |
| `quantify_stud_asymmetry(folder_path, save_path)` | Top-level driver: pairs up LHS/RHS files per tile/run, applies the swap correction, and writes all outputs. |

### Outputs

| File | Contents |
|---|---|
| `{file_stem}.png` | Raw per-file heatmap (one per `.2D` file), from `plot_2d_files`. |
| `stud_asymmetry_summary.csv` | One row per `(tile_length, tile_id, run)`: `LHS_stud_response`, `RHS_stud_response`, `asymmetry_centroid`, `pmt_gain_effect_estimate`, `total_counts_LHSrun`, `total_counts_RHSrun`, `counts_lhs_brighter`, `counts_rhs_brighter`, `counts_on_diagonal`, `asymmetry_diagonal`. |
| `{tile_length}_{tile_id}_{run}_averaged.png` | Swap-corrected, normalised averaged 2D heatmap. |
| `{tile_length}_{tile_id}_{run}_asymmetry_map.png` | `averaged - averaged.T` difference map. |

---

## 9. Usage

```python
folder_path = r"path\to\raw\2D\files"
save_path   = r"path\to\output\folder"

plot_2d_files(folder_path, save_path)
quantify_stud_asymmetry(folder_path, save_path)
```

Requirements:

- `.2D` files must follow the `{tile_length}_{tileID}_{LHS|RHS}_{run}_BvsD.2D`
  naming convention exactly, or `quantify_stud_asymmetry` will skip them.
- Every tile/run needs **both** an LHS and an RHS file present to be
  included in the averaged/asymmetry outputs — an unpaired file is skipped
  with a printed warning.
- Both files in a pair are expected to have matching (square) histogram
  shapes; a shape mismatch is skipped with a warning rather than raising an
  error.
