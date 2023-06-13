# Consulting Practicum

This is a 2-month sponsored capstone project. Our project aims to find the optimal combine settings to farmers that result in the maximum yield while trying to minimize MOG(Materials Other Than Grains) and Broken Grains based on real-time weather conditions during harvesting. The process of the demo can be divided into five parts (data exploration will not be demonstrated).

- The constraints to focus on: Crop Type, Combine Model Number(size), Location Information (country, city, etc.), Current Temperature, Current Precipitation, Current Wind Speed, Current Pressure
- The combine settings to focus on:  Concave Position, Chaffer Position, Sieve Position, Rotor Speed, Fan Speed
- The particular losses to focus on: Material Other Than Grain, Broken Grain

### Part 1 - get inputs from google sheets:

The function is applied to read data from google sheets. 

To make the process more convinence for users(farmers), a worksheet is created for them to enter the following basic informations: crop type they attempt to  harvest, combine model number(size) they are using, country, and city (if inside USA, zipcode will be taken into consideration). This is the interface to enter information on users end and auto-populate recommended combine settings from model. 

### Part 2 - get current weather conditions from API:

By using Geopy package in Python, location information from input sheets will be converted into longtitude and latitude. Given longtitude and latitude, we'll fetch current weather data and store the values from API. 

### Part 3 - data manipulation:

The function in this part has two tasks in order to better prepare for modeling:
1. Data cleaning is performed in this part such as: checking NAs, removing unnecessary columns, creating additional target DELTA variable (MOG + Broken Grain - Yield), etc.
2. To only keep the relevant data, filtering the dataset based on model number and crop type inputs from the interface is neccessary. 

### Part 4 - fit in Guassian Process Regressor model:

Our concept in the part is to 1) interpolate unknow DELTA value, 2) apply the constrainsts of current weather data to shrink the searching area, 3) locate the Min DELTA within the surface, and 4) its corresponding settings.
![image](https://github.com/ruijing-xiong/consulting_practicum/assets/129993213/13cd2771-425d-419f-a1fe-f4c604aeb4c5)

To formulate the optimization problem, Guassian Process Regressor is selected for optimization becasue of two reasons:
a) it is a powerful tool to interpolte data in nonparametric dataset.
b) it works well on small datasets. 

  #### Here is a demostration of 2 parameters:
1. We start with a 2D scatter plot with a colorbar by DELTA value.  
![image](https://github.com/ruijing-xiong/consulting_practicum/assets/129993213/629f56d0-48ba-4f3b-b32e-abf4b9d91048)

2. After interpolating data by using Guassian Process Regressor model, unknown DELTA based on the given settings can be estimated. The blank space from the previous 2D plot are filled with values and colored by DELTA values. 
![image](https://github.com/ruijing-xiong/consulting_practicum/assets/129993213/fb7ce5d5-1e83-4992-af6b-61f60e5da5e5)

3. Min DELTA of the area is found and marked as a white 'X' on the plot.
4. Recommended settings with the min DELTA are printed at the bottom of the plot. 

### Part 5 - populate optimal settings:

The google sheet will be updated with the optimal outputs from Gussian Process Regressor.  






