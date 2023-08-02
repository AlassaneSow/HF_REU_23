#Code used to perform a kriging interpolation on respiration data from Hemlock stands in Harvard Forest. 

In order to perform a kriging interpolation you need two things:
-Coordinates where measurements were taken. 
-Data

The least amount of points you can use is 30 and if you are able to I'd recommend using more. Where you take the measurements from is not very important but try to evenly spread them across your plot.
For reference I put five soil collars in 4m2 sub-plots six times in a 16m diameter hemlock stand. 

Briefly this code works by plotting your data on a variogram and using a line of best fit to model a variable, in my case respiration, across points.

A variogram is a function that describes the relationship of distance and semivariance in a given data set. Accounting for space is what makes kriging interpolations a powerful tool. 
Fitting a line to the variogram is the most time consuming part of this process.

The fit.variogram function needs a few arguments 
-a variogram

- a model which you have to make yourself. In the model function there are a few more arguments.

    -psill =, psill, meaning partial sill, is the sill minus the nugget

    -model =, this is where you can choose which type of line to fit on your data i.e. "Log","Exp". You can call vgm() to list models you can use.

    - range =, range is where the variogram begins to level off, this marks where data is no longer correlated with distance.
 
    - nugget =, nugget is where the line intercepts the y axis
 
You CAN do all of this by hand if you'd like but the autofitVariogram function does this for you and makes a data frame with the best options. 

The final heat maps are only as accurate as your model so I would take as much time as you can perfecting the line.

#Troubleshooting

-Data MUST have no NA's if not the variogram function will not work so be sure to remove any NA's and NA's are in a format R can understand. 

-GPS is not very accurate at small scales so the maps you make may look weird. The only way to avoid this is to space measurements out enough for GPS to be reliable. 

-R will round coordinates, I recommend checking that this does not happen every time you upload a new .csv and change the digits to 10.

-"No convergence after 200 iterations: try different initial values?". This warning happens often and is mostly a result of a small data set. I recommend collecting more measurements but the model will still work if this is not possible. 

-If you make the model by hand you may get an error about a negative range. I'd simply use the autofitVariogram function. Sometimes R ignores your inputs and makes models that don't make sense. 

-When plotting the final heat map you may get an empty map. This is because the size of your tiles is too small. you can change the dimensions in the geom_tile argument. 


