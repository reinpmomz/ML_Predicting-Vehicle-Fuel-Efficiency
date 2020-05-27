# ML_Predicting-Vehicle-Fuel-Efficiency
Fitting a model that predicts a vehicle’s fuel efficiency in miles per gallon (MPG) - of vehicles, using the known values of other characteristics of the vehicle.

This is an example of using R to tackle a prediction problem using machine learning.

This particular example was most-immediately inspired by a case study in the excellent free online course Supervised Machine Learning Case Studies in R developed by Julia Silge. That case study involves 2018 model year vehicles, while this one uses 2020 model year vehicles. There are also multiple other similar data sets that are commonly used for case studies in regression.

Three types of models are calibrated in this example:

- linear regression using Ordinary Least Squares (OLS)

- decision trees

- random forests


Attached alongside is a data file named: `cars2020.csv`.

The columns in the data contain the following variables respectively:

- make: brand of the vehicle

- model: name of a specific car product

- mpg, miles per gallon: a representative number for the number of miles the car can be driven while using 1 gallon of fuel

- transmission: Automatic, CVT, or Manual

- gears: number of gears

- drive, drivetrain: 4WD, AWD, FWD, PT 4WD, RWD

- disp: engine displacement (in liters)

- cyl: number of cylinders

- class, vehicle category: Compact, Large, Mid St Wagon, Midsize, Minicompact, Minivan, Sm Pickup Truck, Sm St Wagon, Sm SUV, SPV, Std Pickup Truck, Std SUV, Subcompact, or Two Seater

- sidi, Spark Ignited Direct Ignition: Y or N

- aspiration: Natural, Super, Super+Turbo, Turbo

- fuel, primary fuel type: Diesel, Mid, Premium, Regular

- atvType, alternative technology: Diesel, FFV, Hybrid, None, or Plug-in Hybrid

- startStop, start-stop technology: Y or N