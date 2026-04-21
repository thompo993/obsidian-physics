### Tags 
[[Super MuSR]]
[[PMT]]
[[scintillating tiles]]
### Twin Peak Issue 

The issue is that the peak values of each channel seem to be different, making a broad and wide peak. The potential causes have been noted as follows: 
- alignment of source 
- PMT photocathode uniformity 
- mismatched gain due to slow HV drift
- **QE of the PMT, if LHS has greater counts, then this will cause the two peaks effect, LHS PMT seems to have much greater counts after 10 minutes**

- [ ] R/L the tiles and do a clustering study 
	- [ ] label all tiles
	- [x] Position scans of 105mm tiles, do we recover the twin peak issue? ✅ 2026-04-21
	- [x] Flip source measurement after each tile, to get a LHS and RHS. ✅ 2026-04-21

### potential solution
```
for idx, peak_idx in enumerate(peaks):

        peak_x = x[peak_idx]

        peak_y = y_smooth[peak_idx]

        # Fit polynomial around this peak only if peak_x is greater than 0.005

        if peak_x > 0.005:

            fit_range = (x > peak_x - (x[-1] - x[0]) * 0.075) & (x < peak_x + (x[-1] - x[0]) * 0.075)

            x_fit = x[fit_range]

            y_fit = y[fit_range]
```
changed the fit range from 0.05 to 0.075. This made it fit a larger range of the peak, so it can handle the broader peaks better. 
Still not perfect, but much better



### Position scan 
#### procedure 
- **Convention:** "LHS", marked as in the run names, refers too which stud side aligns with the LHS PMT (left hand side as you are looking at it. )
- measure difference relative to the LHS wall and near side of the source boom arm
- label tiles on each side, convention is LR  corresponds to the tile id and length such that it corresponds too how you would read the tiles ** for example:

| 30MM | ID001 |
| ---- | ----- |
| LHS  | RHS   |

- increase this by 1cm for 10 measurements across the tile 
- measure distance from the tile (which is not moved at all in this process) to get relative tile position (no real physical meaning)
- qualitative more than quantitative need to extract the "shape" of the PHS
	- LHS w.r.t position
	- RHS w.r.t position 
	- total w.r.t position 

#### 185mm
![[fig-260416-185mm-pos-scan-105mmt-iles.png]]

#### 195mm 
![[master/00 - assets/attachments/fig-260416-195mm-pos-scan-105mmt-iles.png]]

#### 205mm
![[fig-260416-205mm-pos-scan-105mm-tiles 1.png]]

### 215
![[fig-260416-215mm-pos-scan-105mm-tiles 1.png]]
#### 225
![[fig-260416-225mm-pos-scan-105mmt-iles.jpg]]

#### 235  
![[fig-260416-235mm-pos-scan-105mmt-iles.png]]

- this pattern continues, and then drops off very quickly while CHD spikes. overall effect of L/R alignment is seem (at a preliminary level, zero analysis conducted. )