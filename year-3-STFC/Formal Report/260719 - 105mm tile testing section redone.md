---
tags:
  - note
  - super-musr
  - scintillating-tiles
created: 2026-07-19
---
# Links: 
[[260603 - Plan]]
# Notes:
## Current 105mm tile testing section 
```

\subsection{105mm Tiles}
For the 105mm tiles, 41 tiles that were tested. The tiles on average performed 2.9\% better than the mean benchmark repeat runs. This is to be expected as unlike the 30mm tiles, these tiles simply need to be comparable to the benchmark tile. As a result the acceptance threshold was set 2 standard deviations below the benchmark repeats. Of the 41 Tiles three fall the acceptance threshold and hence were flagged for further investigation. For tiles longer than 63mm, the epoxy was substituted for a new rein, RT152, which is a low viscosity optically clear resin \cite{ResintechLtdPotting}. This resin was selected for the 105mm and 210mm tiles as RAL71 resin has to be cured at higher temperatures. This was causing the longer tiles to undergo warping and bending inside the curing and gluing jig. As a result a RT152 was selected as it has a curing time of 48hrs at 23$^\circ C$, meaning the oven now simply maintains a controlled low humidity environment for curing.
\begin{table}[h!]
\centering
\begin{tabular}{c cl}
    \toprule
    \textbf{ID} & \textbf{Resin} & \textbf{Production Notes} \\
    \midrule
    18 & RT152 & Chilled Resin, Minor scuffs on tile, Small oxidation above 1 stud\\
    \addlinespace
    28 & RT152 & Chilled Resin, Tiny hair in 1 stud, 1 Loose Stud\\
    \addlinespace
    76 & RT152 & Semi Chilled Resin, Bubble in 1 stud\\    
    \addlinespace
    34* & RT152 & Chilled Resin, Minor scratches on tile, Tiny hair and specs in 1 stud\\
    \bottomrule
\end{tabular}
\caption{Production teams notes notes for poor performing 105mm tiles. (*) Note tile 34 has been included to show how notes compare to a tile which meets the required light output.\label{tab:105mm_tile_technician_notes}} 
\label{tab:30mm-poor-performance}
\end{table}
\begin{figure}[htbp]
    \centering
    \includegraphics[width=1\linewidth]{105mm_tiles_scatter.png}
    \caption{Scatter diagram of Pulse Height Spectra Peak Values vs Tile ID for the 105mm Tiles.}
    \label{fig:105mm_scatter}
\end{figure}
Each of these tiles was noted to have an issue in a single stud, however so did the comparison tile that has performed above the acceptance threshold. A Tile Stud analysis was conducted in order to determine if there was any clear difference betweens stud performance. Figure \ref{fig:2x2_105mm_stud_analysis} outlines all four of the tiles in Table \ref{tab:105mm_tile_technician_notes}. All three of the poorly performing tile, and tile ID 34 exhibit asymmetry across one of the studs. It cannot be justified that one stud is at fault for any of the tiles. Another feature of note is "Chilled Resin" this refers to refrigerating the resin before using it, a process technicas integrated into the production process to improve the ease of handling of RT152 resin. Overall the tiles were noted to have 7.3\% failure rate, with no clear indication of the cause of the drop-in performance. It must also be noted that accounting for error these tiles may still have an acceptable degree of light output. Therefore future work will involve re-testing these tiles in order to determine if they are suitable for the final detector.

```

