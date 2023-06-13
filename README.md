# Consulting Practicum

This is a 2-month sponsored capstone project. Our project aims to, during harvesting, find the optimal combine settings to farmers that result in the maximum yield while trying to minimize MOG(Materials Other Than Grains) and Broken Grains based on real-time weather conditions. The process of the demo can be divided into five parts (data exploration will not be demostrated).

- The constraints to focus on: Crop Type, Combine Model Number, Location Information (country, city, etc.), Current Temperature, Current Precipitation, Current Wind Speed, Current Pressure
- The combine settings to focus on:  Concave Position, Chaffer Position, Sieve Position, Rotor Speed, Fan Speed
- The particular losses to focus on: Material Other Than Grain, Broken Grain

### Part 1 - get inputs from google sheets:

This is the sheet to enter information on users end(left-hand side of the sheet) and auto-populate recommended combine settings from model (right-hand side).

To make the process more convinence for users(farmers), a simply sheets was created for them to enter the following basic informations: crop type they attempt to  harvest, combine model number(size) they are using, country, and city (if inside USA, zipcode will be available). 

### Part 2 - get current weather conditions from API:

By using Geopy package in Python, location information from input sheets will be converted into longtitude and latitude. Given longtitude and latitude, we'll fetch current weather data and store the values from API. 

### Part 3 - data manipulation:

1. Data cleaning is performed in this part such as: checking NAs, removing unnecessary columns, creating additional target DELTA variable (MOG + Broken Grain - Yield), etc.
2. To better prepare and keep the data relevant, filter data based on model number and crop type inputs from sheets. 

### Part 4 - fit in Guassian Process Regressor model:

Our concept in the part is to 1) interpolate unknow DELTA value, 2) apply the constrainsts of current weather data to shrink the searching area, 3) locate the Min DELTA within the surface, and 4) its corresponding settings.
![image](https://github.com/ruijing-xiong/consulting_practicum/assets/129993213/13cd2771-425d-419f-a1fe-f4c604aeb4c5)

To formulate the optimization problem, Guassian Process Regressor is selected for optimization becasue of two reasons:
a) it is a powerful tool to interpolte data in nonparametric dataset.
b) it works well on small datasets. 

  #### Here is a demostration of 2 parameters:
1. We start with a 2D scatter plot with a colorbar by DELTA value.  
![image](https://github.com/ruijing-xiong/consulting_practicum/assets/129993213/629f56d0-48ba-4f3b-b32e-abf4b9d91048)

2. After interpolating data which means, in our case, to estimate unknown DELTA based on the given settings, the blank space will be filled with values (blanks turn into green areas). 
![image](https://github.com/ruijing-xiong/consulting_practicum/assets/129993213/fb7ce5d5-1e83-4992-af6b-61f60e5da5e5)

3. Min DELTA is marked as a white 'X' on the plot. 
4. Recommended settings are printed at the bottom of the plot. 

### Part 5 - populate optimal settings:

The sheet will be updated with optimal outputs from Gussian Process Regressor.  






