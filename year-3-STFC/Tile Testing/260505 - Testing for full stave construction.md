## Tags:
[[Super MuSR]]
[[scintillating tiles]]
[[260512 - Small Batch Tile Testing and Stave Assembly]]

- [ ] Check all tiles with neil database ⏫ 📅 2026-05-13
# Key Findings:
The 30mm, 43mm and 63mm tiles are all performing well, there is reason for concern regarding the 105mm tiles and the  210mm tiles. 

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
[[fig-260512-63mm_tiles_both_stud.png]]
![[fig-260512-63mm_tiles_both_stud.png]]

[[fig-260512-63mm_tiles_multi_channel_comparison.png]]
![[fig-260512-63mm_tiles_multi_channel_comparison.png]]
For the multi channel analysis, we have one that stands out, tile ID 041, which was also picked up on by the 2D plotting: 
![[fig-260512-63mm_id041_LHS_run1_BvsD.png | 300]] ![[fig-260512-63mm_id040_RHS_run1_BvsD.png| 300]] 


### 105mm tiles
ID 005 was used as a benchmark tile, so we must consider that this tile was simply excellent, however our tiles do appear to be worse than the benchmark. 
[[fig-260512-105mm_tiles_both_studs.png]]
![[fig-260512-105mm_tiles_both_studs.png]]
As we can see above the benchmark tiles are performing better than the batch of tiles selected for the made up stave, they are **13.2%** worse on average. however, this could be attributed to the benchmark being extremely good. 

For the stud symmetry checks: 
![[fig-260512-105mm_tiles_multi_ch.png]]
From this we see that tile ID 041 stands out, checking this with the 2d plots shows
![[fig-260512-63mm_id041_LHS_run1_BvsD.png | 300]] ![[fig-260512-63mm_id040_RHS_run1_BvsD.png| 300]]
This is not as clear as the others, but we see, a slight bias towards the RHS stud when it is placed on the Ch_D, and it is very dominant when placed on Ch_B, so we know the RHS stud has a greater output. 


### 210mm tiles 
Tile ID 008 used as benchmark, and we obtain the following results from the PHS Peaks. 

[[fig-260512-210mm_tiles_both_studs.png]]
![[fig-260512-210mm_tiles_both_studs.png]]
Once again we have worse performance for the 210mm tiles compared to the benchmark, this time, we have a percentage difference of **22.26** this is crazy. I need to look into this more. (I look into it in this file [[260513 - 210mm Tile Poor Performance Investigation]])


[[fig-260512-210mm_tiles_multi_ch.png]]
![[fig-260512-210mm_tiles_multi_ch.png]]
rom this we see that tile ID 041 and 56 stand out:
#### id 041
![[fig-260512-210mm_id041_LHS_run1_BvsD.png | 300]] ![[fig-260512-210mm_id041_RHS_run1_BvsD.png| 300]]

#### 056
![[fig-260512-210mm_id041_LHS_run1_BvsD.png | 300]] ![[fig-260512-210mm_id056_RHS_run1_BvsD.png]]





## Benchmark vs Tile Length




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