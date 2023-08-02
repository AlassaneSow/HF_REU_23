# Auto Chamber Flux Pipeline
THis is code used to get raw data from soil collars in two Harvard Forest hemlock stands. All you need are the .DAT files and measurements for collar heights in cm. 
You will need to measure collar heights every year and change that in the heights data frame (df).
.DAT files have an issue in R where column names become parts of the df so please make sure to remove those and rename accordingly. 

Each collar takes a measurement for 5 minutes in 30 minute cycles. This pipeline takes advantage of that and groups the time of each measurement, taken every second, into half hour groups. 
Then the first and last minutes of measurements are removed from the df. If these are not removed you will get near 0 fluxes because of measurements taken when the chamber is opening/closing. 
A linear model of CO2ppm and time is made. Lastly the slope is used to calculate CO2 flux and the flux is converted to umol of CO2/m^2s^-1. Fluxes with high p-values or extremely high/low values are filtered.

#Trouble Shooting

-You may get a warning that says something like ,'model is essentially a perfect fit', which is no cause for concern this happens everytime. 

-If you are finding that you have near 0 fluxes for every measurement I would check for leaks, broken seals etc or filter more minutes out in line 58.

-I've found that the V1,V2.... column names change between df so I would check that each column is being renamed the correct thing. If not CO2ppm could label an empty column and you will get no linear models.

-If some dates fail to get parsed it is likely because the columns were not removed, but if not you can use the parse_ymd_hms function and find specific rows. 
