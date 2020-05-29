# ML_Predicting-Vehicle-Fuel-Efficiency
Fitting a model that predicts a vehicle’s fuel efficiency in miles per gallon (MPG) - of vehicles, using the known values of other characteristics of the vehicle.

This is an example of using R to tackle a prediction problem using machine learning.

This particular example was most-immediately inspired by a case study in the excellent free online course Supervised Machine Learning Case Studies in R developed by Julia Silge. That case study involves 2018 model year vehicles, while this one uses 2020 model year vehicles. There are also multiple other similar data sets that are commonly used for case studies in regression.


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


## Latex Packages Missing

Installing and maintaining TinyTeX is easy for R users, since the R package tinytex has provided wrapper functions (N.B. the lowercase and bold tinytex means the R package, and the camel-case TinyTeX means the LaTeX distribution). You can use tinytex to install TinyTeX:
install.packages('tinytex')
tinytex::install_tinytex()

to uninstall TinyTeX, run tinytex::uninstall_tinytex() 

To compile a LaTeX document to PDF, call one of these functions (depending on the LaTeX engine you want to use) in tinytex: pdflatex(), xelatex(), and lualatex(). When these functions detect LaTeX packages required but not installed in TinyTeX, they will automatically install the missing packages by default.


## Exploratory Data Analysis
dlookr can help to understand the distribution of data by calculating descriptive statistics of numerical data. In addition, correlation between variables is identified and normality test is performed. It also identifies the relationship between target variables and independent variables.:

The following is a list of the EDA functions included in the dlookr package.:

- describe() provides descriptive statistics for numerical data.
- normality() and plot_normality() perform normalization and visualization of numerical data.
- correlate() and plot_correlate() calculate the correlation coefficient between two numerical data and provide visualization.
- target_by() defines the target variable and relate() describes the relationship with the variables of interest corresponding to the target variable.
- plot.relate() visualizes the relationship to the variable of interest corresponding to the destination variable.
- eda_report() performs an exploratory data analysis and reports the results.

## Model Types

Three types of models are calibrated in this example:

- linear regression using Ordinary Least Squares (OLS)

- decision trees

- random forests