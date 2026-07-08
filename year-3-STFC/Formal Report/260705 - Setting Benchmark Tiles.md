---
tags:
  - note
created: 2026-07-05
---
# Tags 
[[Super MuSR]]
[[year in industry report]]

# current block 
```

\subsection{Determining Acceptance Threshold}
In order to determine an acceptance threshold for scintillating tiles, a series of Benchmark Tiles have to be determined to set as a criterion, as testing all tiles on the beamline is not possible. The Pulse Height Spectrum (PHS) peak-to-valley ratio (P/V) ratio was utilised as the Figure-of-Merit (FoM) for the tile light output and an indicator of a suitable signal to noise ratio required for Super MuSR. For a tile be considered as acceptable light output for Super MuSR, the P/V must exceed that of the current MuSR instrument (Dan Pooley Personal Communication, 2025). PHS from the old MuSR instrument were taken using an 8-bit oscilloscope, providing a P/V of 1.664.

\begin{figure}[htbp]
    \centering
    \begin{subfigure}[t]{0.48\textwidth}
        \centering
        \includegraphics[width=\linewidth]{phs_analysis_og_musr_ptv.png}
        \caption{PHS taken of the MuSR instrument for a ZF with a silver sample. MuSR uses PMTs and so the Pulse Height is a function of Voltage.}
        \label{fig:phs_old_musr}
    \end{subfigure}\hfill
    \begin{subfigure}[t]{0.48\textwidth}
        \centering
        \includegraphics[width=\linewidth]{43mm_3PtV_beamline_260618_id018.png}
        \caption{PHS of a 43mm tile taken parasitically on a Super MuSR detector Stave. Super MuSR uses SiPMs which have been converted from voltage to Least Significant Bit (LSB) which represents the smallest $\Delta V$ of the SiPM.}
        \label{fig:phs_super_musr}
    \end{subfigure}
    \caption{Comparison of PHS between MuSR and Super MuSR instruments.}
    \label{fig:phs_compare}
\end{figure}

Taking the average PHS P/V ratio for all 32 tiles on the stave involved taking multiple runs with each detector module lined up with the sample as there is no geometry where all four modules can be in the line of sight of the target, the overall P/V ratio across the whole stave is 2.73 so the whole stave and ensemble of tiles have an improved performance of 64.0\%. Therefore these tiles are suitable for the final detector. All tile lengths exceeding the current MuSR instrument except for the 210mm tiles, which with a P/V ratio of 1.44 performed 13.5\% worse than the required threshold. This is in part attributed to the greater range of path lengths possible in a longer tile, this broadens and flattens the peak, reducing the P/V ratio. Due to ease of construction and design changes, the 210mm tiles were longer than originally intended which leads to the individual tile criteria not being met. 






The determination of the 30mm benchmark has been established in earlier work, the tile was noted as the minimum acceptable light output and henceforth used as a benchmark tile (Dan Pooley Personal Communication, 2025).

```

# Updated block plan 

### Determining acceptance threshold 
- speak about why we need them 
	- cant test all tiles on beamline 
	- need to determine acceptable light output to test tiles as they come off production 
	- peak to valley is the chosen metric, as it gives an idea of both tiles brightness and signal to noise 
		- super MuSR needs to set  noise threshold, basically need to cut the noise and true positron data, so a deep or wide peak to valley ratio must be chosen. 

#### ensemble detector 
- whole detector over the musr PtV on average 
- therefore all the tiles can be used as benchmark, this is the key result 
- **write this but confirm with dan**

#### individual tiles
all rings passed except 210mm
why 