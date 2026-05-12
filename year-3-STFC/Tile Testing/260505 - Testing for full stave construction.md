## Tags:
[[Super MuSR]]
[[scintillating tiles]]
[[Small Batch Tile Testing and Stave Assembly]]

## 30mm Tiles
Here we used the original "tile21og", which is a historic benchmark tile that has always been used for 30mm tiles. The results for this tile are great: 
[[fig-260512-30mm_tiles_comparison.png]]
![[fig-260512-30mm_tiles_comparison.png]]
As can be seen above, all tiles are significantly above benchmark, which is what we expect. There are insufficient tile numbers to plot a histogram with this tile batch. 

## 43mm Tiles 
The Results for the 43mm Tiles are also excellent. However now there is a stud difference implementation being added.



### 63mm tiles 
same as normal benchmark tile [[260313 - Setting Benchmark]]

### 105mm tiles
005 used as a benchmark tile,
we do not know if 105 is an exemplary tile 
need to do full testing on these tiles ASAP. 

### 210mm tiles 
008 used as benchmark





## Stud Difference Procedure
### Defining LHS and RHS
To determine if the studs are different, we first label the left and right hand sides of the tiles. To save on time I did not label these with the label maker, and instead used the following convention: 

| **LHS** | ![[fig-260512-tile-ordering-convention.jpg \| 400]] | **RHS** |
| ------- | --------------------------------------------------- | ------- |
Such that if the text is the correct way up, as we read from left to right, we define the left hand side as the LHS of the tile. 

### Measuring the Tiles 
We then conduct two runs, so each stud gets a turn on each PMT, this way PMT differences will create the illusion of one stud being worse, when it may be that the PMT is lower gain (shouldn't be but better safe than sorry).

For the code I wrote to work, we need a couple of things setup in the file names so the code parses it correctly, naming convention for stud check runs are as follows: 
```
43mm_benchmark_LHS_run1
```
The only thing that actually matters is the LHS, and this signifies which side of the tile is on **Ch_B** in the Pico scope, assuming you have not changed the wiring since I left in 2026 (please check) this should be the leftmost PMT as you look at the box.

This code should help you tell if the tile has non symmetrical light output. 

![[fig-260512-43mm_id055_LHS_run1_BvsD.png| 300]] ![[fig-260512-43mm_id055_RHS_run1_BvsD.png | 300]] 

From this example here, we have a greater LHS stud output

