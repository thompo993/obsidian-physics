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
The Results for the 43mm Tiles are also excellent. However now there is a stud difference implementation being added (see [[#Stud Difference Procedure]]). 
[[fig-260512-43mm_tiles_both_stud.png]]
![[fig-260512-43mm_tiles_both_stud.png]]

For the stud differences: 
[[fig-260512-43mm_tiles_multi_channel_comparison.png]]
![[fig-260512-43mm_tiles_multi_channel_comparison.png]]
As can be seen by the above plot, the LHS vs RHS yielded no significant results, with the exception of tile ID:055, this can also be seen clearly inside of the 2D plots. 
![[fig-260512-43mm_id055_LHS_run1_BvsD.png| 300]] ![[fig-260512-43mm_id055_RHS_run1_BvsD.png | 300]] 
### 63mm tiles 
same as normal benchmark tile [[260313 - Setting Benchmark]] (ID:015). These tiles also performed exceptionally. 

![[fig-260512-63mm_tiles_both_stud.png]]


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

From this example here, we have a greater LHS stud output when it is on channel B (LHS on channel B run), and when we place the LHS stud on channel D, we get a greater light output on channel D, this means that the LHS stud has a greater light output than the RHS stud. 

**note: Often they will both be below the white line, this is either PMT or source alignment, this cannot be from the tile.**

if you want to do further analysis, I recommend a LHS vs RHS scatter plot, as above here is the code I used to split the data frame (using the same naming conventions as above)
```
def select_studs(df):

    """Select LHS and RHS studs based on file name and channel.

    - If 'LHS' in filename: Channel B = LHS stud, Channel D = RHS stud

    - If 'RHS' in filename: Channel D = LHS stud, Channel B = RHS stud

    - If Channel is 'Ch_B+D': add to total_studs

    Returns three dataframes: lhs, rhs, and total studs.

    """

    lhs_studs = []

    rhs_studs = []

    total_studs = []

    for idx, row in df.iterrows():

        if row["Channel"] == "Ch_B+D":

            total_studs.append(row)

        elif "LHS" in row["File"]:

            if row["Channel"] == "Ch_B":

                lhs_studs.append(row)

            elif row["Channel"] == "Ch_D":

                rhs_studs.append(row)

        elif "RHS" in row["File"]:

            if row["Channel"] == "Ch_D":

                lhs_studs.append(row)

            elif row["Channel"] == "Ch_B":

                rhs_studs.append(row)

    lhs_df = pd.DataFrame(lhs_studs)

    rhs_df = pd.DataFrame(rhs_studs)

    total_df = pd.DataFrame(total_studs)

    return lhs_df, rhs_df, total_df
```