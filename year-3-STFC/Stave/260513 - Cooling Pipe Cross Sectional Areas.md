# Tags 
[[Stave]]
[[Super MuSR]]

# Summary Table: 
| Tube Name     | External diameter | Thickness | Best Estimate (mm$^2$) |
| ------------- | ----------------- | --------- | ---------------------- |
| 1 - Uncreased | 6mm               | 0.6mm     | **13.92**              |
| 1 - Creased   | 6mm               | 0.6mm     | **9.48**               |
| 2             | 6mm               | 0.8mm     | **11.16**              |
| 3             | 6mm               | 0.8mm     | **11.15**              |
| Baseline      | 5mm               | 0.5mm     | **12.56**              |

# Introduction 
Three deliberately deformed into a shape resembling a rectangle with two semi circles attached either side. These pipes have been investigated for their internal cross sectional area and compared to a baseline spherical pipe. The three samples can be found below: 
![[fig-260513-stave-tubing-types.jpg | 300]]

---
## Baseline Tube

| Material         | Copper |
| ---------------- | ------ |
| Supplier         | Amazon |
| Outside Diameter | 5mm    |
| Thickness        | 0.5mm  |


## Tube 1 - Short Tube
### Specifications:

| Material         | Copper |
| ---------------- | ------ |
| Supplier         | BES    |
| Outside Diameter | 6mm    |
| Thickness        | 0.6mm  |

### Notes:
#### Uncreased Side
This side of the tube looked very promising, and had clean cut all the way around this lead to the following upper bound, lower bound, and "best estimate" guesses: 
#### Creased Side
This side of the tube had noticeable deformities, a result from bending the pipe after cooling using freeze spray. The concern is that this crease will reduce cross sectional area and hence flow  rate. From the table above t appears that the crease has no effect on the surface area. **Disclaimer:** These are estimates, to discern the difference between these two with a higher degree of certainty, a  surface area, or position scan should be made with the microscope. 
## Tube 2 - Long Tube
### specifications: 
| Material         | Copper |
| ---------------- | ------ |
| Supplier         | RS     |
| Outside Diameter | 6mm    |
| Thickness        | 0.8mm  |

---
### Notes:

Very Cleanly cut and uniform thickness around the edge  
## Tube 3 - Long Tube
### specifications: 
| Material         | Copper |
| ---------------- | ------ |
| Supplier         | RS     |
| Outside Diameter | 6mm    |
| Thickness        | 0.8mm  |
### Notes:

Very Cleanly cut and uniform thickness around the edge  
#  Estimation
### Best Estimate: 
This was estimated using repeated averages of the internal height and width of the tubes, for all regular shaped tubes. Then the assumption was made that the shape of the pipe was equivalent to a rectangle with a semi-circle attached either side, as per the figure below:
![[fig-260513-stave-tubing-geom.jpg |]]
Distance y was determined using the average height across the straight part of the tubing, and x was determined using the assumption that y is equal to the diameter of the circle on each end, therefore: 
$$
x = L - y
$$
where L is the full internal length of the  pipe. From here estimates were done using 
$$
(L-\frac{y}{2})\times y + \pi(\frac{y}{2})^2
$$
### Upper and Lower bounds:
Upper bound was done using: 
$$
L \times y
$$
Lower bound was done using: 
$$
x \times y
$$