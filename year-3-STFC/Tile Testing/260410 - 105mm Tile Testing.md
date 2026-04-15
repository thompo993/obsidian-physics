### Tags:
[[Super MuSR]]
[[scintillating tiles]]

### Notes:
Issues were present for the peak finding software for this length of tile, without really good alignment the tiles seemed to not be able to find a central peak, but instead found two peaks
This could be due to: 
- Tile has different light output 
- tile not placed symmetrically 
- source not aligned properly 
- **need to do a test of stud location on the PMT photocathode.**

#### Double peak Issue
- The issue is that there are two peaks and the peak finder struggles
- Lowered voltage of the RHS PMT by 5 V from 760 --> 750
- This fixed the issue, I am unsure why, was the voltage too high, after pulsing or analogue saturation? 
- issue returned on a separate run, the root cause must be the **Optical coupling**. 
- **fix:** Do more optical grease and a greater focus on optical coupling

#### Spiky peak issue
- Sometimes the D+B channel PHS seems really "spiky" essentially looks low stats when it isnt. 
- This may be some sort of binning issue 
- **fix:** restarting Pico scope seems to fix this issue and returns normal looking PHS, 
### Procedure 
- Select tile 
- Mark tile at midpoint 
- place onto CENTRE of the photocathode 
- align the source properly onto the midpoint 
- measure the tile. 