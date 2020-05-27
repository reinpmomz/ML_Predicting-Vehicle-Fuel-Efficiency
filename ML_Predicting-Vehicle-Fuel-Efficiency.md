---
title: "ML_Predicting-Vehicle-Fuel-Efficiency"
author: "Reinp"
date: "2020-05-28"
output:
  html_document: 
    keep_md: yes
  pdf_document: default
  word_document: default
---

# R Programming

## Set Chunk requirements


```r
knitr::opts_chunk$set(echo = TRUE, message = FALSE, warning = FALSE)
#echo=FALSE indicates that the code will not be shown in the final document 
#(though any results/output would still be displayed).
#include=FALSE to have the chunk evaluated, but neither the code nor its output displayed
# warning=FALSE and message=FALSE suppress any R warnings or messages from being included 
#in the final document
```


## Load Relevant Packages and Data Set


```r
library(tidyverse)
## tidyverse includes readr, ggplot2, dplyr, forcats, tibble, tidyr, purrr, stringr

## Reading our dataset
setwd('E:/Documents/Reinp/GitHub Respositories/ML_Predicting-Vehicle-Fuel-Efficiency')


cars2020 <- read.csv("cars2020.csv")
attach(cars2020)
View(cars2020)
```

## Structure of the Data


```r
#cars2020 #The input data consists of 1,164 observations of 14 variables

head(cars2020)
```

```
##   ï..make          model     mpg transmission gears drive displ cylinders
## 1  Toyota        Corolla 34.2793          CVT    10   FWD   2.0         4
## 2  Toyota Corolla Hybrid 52.0000          CVT     1   FWD   1.8         4
## 3  Toyota        Corolla 31.8162       Manual     6   FWD   2.0         4
## 4  Toyota    Corolla XSE 33.6766          CVT    10   FWD   2.0         4
## 5  Toyota        Corolla 33.0496          CVT     1   FWD   1.8         4
## 6  Toyota        Corolla 33.1228       Manual     6   FWD   1.8         4
##     class lv2 lv4 sidi aspiration        fuelType1 atvType startStop
## 1 Compact   0  13    Y    Natural Regular Gasoline    None         N
## 2 Compact   0  13    N    Natural Regular Gasoline  Hybrid         Y
## 3 Compact   0  13    Y    Natural Regular Gasoline    None         N
## 4 Compact   0  13    Y    Natural Regular Gasoline    None         N
## 5 Compact   0  13    N    Natural Regular Gasoline    None         N
## 6 Compact   0  13    N    Natural Regular Gasoline    None         N
```

```r
tail(cars2020)
```

```
##            ï..make       model     mpg transmission gears drive displ cylinders
## 1159 Mercedes-Benz       S560e 22.8263    Automatic     9   RWD     3         6
## 1160           BMW M2 CS Coupe 19.5165       Manual     6   RWD     3         6
## 1161           BMW M2 CS Coupe 18.7179       Manual     7   RWD     3         6
## 1162          Audi         SQ7 17.1357    Automatic     8   AWD     4         8
## 1163          Audi         SQ8 17.0474    Automatic     8   AWD     4         8
## 1164       Bentley    Bentayga 18.7319    Automatic     8   AWD     3         6
##           class lv2 lv4 sidi aspiration        fuelType1        atvType
## 1159      Large   0   9    Y      Turbo Premium Gasoline Plug-in Hybrid
## 1160 Subcompact  10   0    Y      Turbo Premium Gasoline           None
## 1161 Subcompact  10   0    Y      Turbo Premium Gasoline           None
## 1162    Std SUV   0   0    Y      Turbo Premium Gasoline           None
## 1163    Std SUV   0   0    Y      Turbo Premium Gasoline           None
## 1164    Std SUV   0   0    Y      Turbo Premium Gasoline Plug-in Hybrid
##      startStop
## 1159         Y
## 1160         Y
## 1161         Y
## 1162         Y
## 1163         Y
## 1164         Y
```

```r
# How many variables and observations are there?
ncol(cars2020)
```

```
## [1] 16
```

```r
nrow(cars2020)
```

```
## [1] 1164
```

```r
#learn more about the dataset
help(cars2020)
```

```
## No documentation for 'cars2020' in specified packages and libraries:
## you could try '??cars2020'
```

```r
??cars2020
str(cars2020)
```

```
## 'data.frame':	1164 obs. of  16 variables:
##  $ ï..make     : Factor w/ 43 levels "Acura","Alfa Romeo",..: 41 41 41 41 41 41 41 24 24 24 ...
##  $ model       : Factor w/ 834 levels "124 Spider","1500 2WD",..: 227 230 227 232 227 227 231 703 704 703 ...
##  $ mpg         : num  34.3 52 31.8 33.7 33 ...
##  $ transmission: Factor w/ 3 levels "Automatic","CVT",..: 2 2 3 2 2 3 2 2 2 3 ...
##  $ gears       : int  10 1 6 10 1 6 1 1 1 6 ...
##  $ drive       : Factor w/ 5 levels "4WD","AWD","FWD",..: 3 3 3 3 3 3 3 3 3 3 ...
##  $ displ       : num  2 1.8 2 2 1.8 1.8 1.8 2 2 2 ...
##  $ cylinders   : int  4 4 4 4 4 4 4 4 4 4 ...
##  $ class       : Factor w/ 15 levels "Compact","Large",..: 1 1 1 1 1 1 1 9 9 9 ...
##  $ lv2         : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ lv4         : int  13 13 13 13 13 13 13 24 24 24 ...
##  $ sidi        : Factor w/ 2 levels "N","Y": 2 1 2 2 1 1 1 1 1 1 ...
##  $ aspiration  : Factor w/ 4 levels "Natural","Super",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ fuelType1   : Factor w/ 4 levels "Diesel","Midgrade Gasoline",..: 4 4 4 4 4 4 4 4 4 4 ...
##  $ atvType     : Factor w/ 5 levels "Diesel","FFV",..: 4 3 4 4 4 4 4 4 4 4 ...
##  $ startStop   : Factor w/ 2 levels "N","Y": 1 2 1 1 1 1 1 1 1 1 ...
```

```r
class(cars2020)
```

```
## [1] "data.frame"
```

```r
typeof(cars2020) 
```

```
## [1] "list"
```

```r
length(cars2020)
```

```
## [1] 16
```

```r
names(cars2020) #display variable names
```

```
##  [1] "ï..make"      "model"        "mpg"          "transmission" "gears"       
##  [6] "drive"        "displ"        "cylinders"    "class"        "lv2"         
## [11] "lv4"          "sidi"         "aspiration"   "fuelType1"    "atvType"     
## [16] "startStop"
```

```r
#attributes(cars2020) #names(cars2020), class(cars2020), row.names(cars2020)
```

## Missing data


```r
which(!complete.cases(cars2020))
```

```
## integer(0)
```

## Descriptive Statistics


```r
library(knitr)
library(mosaic)
library(psych)


names(cars2020)[1] <- "car_make" #rename by index column name with base r functions
#names(cars2020)[names(cars2020) == "ï..make"] <- "car_make"

#summary statistics
summary(cars2020) ##summarizes the dataset
```

```
##           car_make             model           mpg           transmission
##  BMW          :111   Sierra 4WD   :   9   Min.   :10.59   Automatic:812  
##  Mercedes-Benz: 89   Silverado 4WD:   9   1st Qu.:19.32   CVT      :128  
##  Chevrolet    : 79   Camaro       :   8   Median :22.63   Manual   :224  
##  Ford         : 73   Sierra 2WD   :   8   Mean   :23.55                  
##  Toyota       : 64   Silverado 2WD:   8   3rd Qu.:26.36                  
##  GMC          : 53   Civic 2Dr    :   6   Max.   :57.78                  
##  (Other)      :695   (Other)      :1116                                  
##      gears           drive         displ         cylinders            class    
##  Min.   : 1.000   4WD   :164   Min.   :1.000   Min.   : 3.00   Sm SUV    :237  
##  1st Qu.: 6.000   AWD   :378   1st Qu.:2.000   1st Qu.: 4.00   Midsize   :171  
##  Median : 8.000   FWD   :298   Median :3.000   Median : 6.00   Std SUV   :139  
##  Mean   : 7.303   PT 4WD: 30   Mean   :3.077   Mean   : 5.57   Compact   :129  
##  3rd Qu.: 8.000   RWD   :294   3rd Qu.:3.600   3rd Qu.: 6.00   Subcompact:118  
##  Max.   :10.000                Max.   :8.000   Max.   :16.00   Large     :103  
##                                                                (Other)   :267  
##       lv2              lv4         sidi          aspiration 
##  Min.   : 0.000   Min.   : 0.000   N:217   Natural    :494  
##  1st Qu.: 0.000   1st Qu.: 0.000   Y:947   Super      : 41  
##  Median : 0.000   Median : 0.000           Super+Turbo:  6  
##  Mean   : 1.587   Mean   : 5.253           Turbo      :623  
##  3rd Qu.: 0.000   3rd Qu.:13.000                            
##  Max.   :22.000   Max.   :47.000                            
##                                                             
##              fuelType1             atvType     startStop
##  Diesel           : 20   Diesel        :  20   N:492    
##  Midgrade Gasoline: 12   FFV           :  24   Y:672    
##  Premium Gasoline :593   Hybrid        :  78            
##  Regular Gasoline :539   None          :1006            
##                          Plug-in Hybrid:  36            
##                                                         
## 
```

```r
describe(cars2020)
```

```
##               vars    n   mean     sd median trimmed    mad   min    max  range
## car_make*        1 1164  21.67  12.11  20.00   21.39  16.31  1.00  43.00  42.00
## model*           2 1164 420.10 243.69 415.50  420.82 324.69  1.00 834.00 833.00
## mpg              3 1164  23.55   6.40  22.63   22.86   5.21 10.59  57.78  47.19
## transmission*    4 1164   1.49   0.80   1.00    1.37   0.00  1.00   3.00   2.00
## gears            5 1164   7.30   1.97   8.00    7.51   1.48  1.00  10.00   9.00
## drive*           6 1164   2.92   1.39   3.00    2.91   1.48  1.00   5.00   4.00
## displ            7 1164   3.08   1.29   3.00    2.92   1.48  1.00   8.00   7.00
## cylinders        8 1164   5.57   1.83   6.00    5.37   2.97  3.00  16.00  13.00
## class*           9 1164   8.27   4.74  10.00    8.41   5.93  1.00  15.00  14.00
## lv2             10 1164   1.59   3.94   0.00    0.48   0.00  0.00  22.00  22.00
## lv4             11 1164   5.25   8.25   0.00    3.79   0.00  0.00  47.00  47.00
## sidi*           12 1164   1.81   0.39   2.00    1.89   0.00  1.00   2.00   1.00
## aspiration*     13 1164   2.65   1.47   4.00    2.69   0.00  1.00   4.00   3.00
## fuelType1*      14 1164   3.42   0.61   3.00    3.45   0.00  1.00   4.00   3.00
## atvType*        15 1164   3.87   0.56   4.00    3.99   0.00  1.00   5.00   4.00
## startStop*      16 1164   1.58   0.49   2.00    1.60   0.00  1.00   2.00   1.00
##                skew kurtosis   se
## car_make*      0.13    -1.20 0.35
## model*         0.01    -1.25 7.14
## mpg            1.64     4.74 0.19
## transmission*  1.16    -0.43 0.02
## gears         -1.62     3.36 0.06
## drive*         0.39    -1.13 0.04
## displ          0.97     0.25 0.04
## cylinders      1.14     1.76 0.05
## class*        -0.26    -1.45 0.14
## lv2            2.33     4.19 0.12
## lv4            1.51     2.28 0.24
## sidi*         -1.61     0.59 0.01
## aspiration*   -0.19    -1.93 0.04
## fuelType1*    -0.98     2.17 0.02
## atvType*      -3.00    11.86 0.02
## startStop*    -0.31    -1.90 0.01
```

```r
#1. Dolar sign Syntax

table(cars2020$car_make) 
```

```
## 
##             Acura        Alfa Romeo      Aston Martin              Audi 
##                18                 7                 4                40 
##           Bentley               BMW           Bugatti             Buick 
##                 8               111                 1                14 
##          Cadillac         Chevrolet          Chrysler             Dodge 
##                24                79                 6                25 
##           Ferrari              Fiat              Ford           Genesis 
##                 7                 4                73                15 
##               GMC             Honda           Hyundai          Infiniti 
##                53                45                39                14 
##            Jaguar              Jeep             Karma               Kia 
##                29                36                 1                37 
##       Lamborghini        Land Rover             Lexus           Lincoln 
##                 7                23                37                23 
##             Lotus          Maserati             Mazda     Mercedes-Benz 
##                 4                10                23                89 
##              MINI        Mitsubishi            Nissan           Porsche 
##                17                16                30                40 
##               Ram       Rolls-Royce Roush Performance            Subaru 
##                14                 8                 4                24 
##            Toyota        Volkswagen             Volvo 
##                64                20                21
```

```r
table(cars2020$transmission)
```

```
## 
## Automatic       CVT    Manual 
##       812       128       224
```

```r
table(cars2020$drive)
```

```
## 
##    4WD    AWD    FWD PT 4WD    RWD 
##    164    378    298     30    294
```

```r
table(cars2020$class)
```

```
## 
##          Compact            Large     Mid St Wagon          Midsize 
##              129              103               11              171 
##      Minicompact          Minivan    Passenger Van  Sm Pickup Truck 
##               32                8                2               21 
##      Sm St Wagon           Sm SUV              SPV Std Pickup Truck 
##               30              237               24               87 
##          Std SUV       Subcompact       Two Seater 
##              139              118               52
```

```r
table(cars2020$sidi)
```

```
## 
##   N   Y 
## 217 947
```

```r
table(cars2020$aspiration)
```

```
## 
##     Natural       Super Super+Turbo       Turbo 
##         494          41           6         623
```

```r
table(cars2020$fuelType1)
```

```
## 
##            Diesel Midgrade Gasoline  Premium Gasoline  Regular Gasoline 
##                20                12               593               539
```

```r
table(cars2020$atvType)
```

```
## 
##         Diesel            FFV         Hybrid           None Plug-in Hybrid 
##             20             24             78           1006             36
```

```r
table(cars2020$startStop)
```

```
## 
##   N   Y 
## 492 672
```

```r
summary(cars2020$mpg)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   10.59   19.32   22.63   23.55   26.36   57.78
```

```r
summary(cars2020$gears)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   1.000   6.000   8.000   7.303   8.000  10.000
```

```r
summary(cars2020$displ)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   1.000   2.000   3.000   3.077   3.600   8.000
```

```r
summary(cars2020$cylinders)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    3.00    4.00    6.00    5.57    6.00   16.00
```

```r
#2. FormulaSyntax

## one categorical

tally(~car_make, data=cars2020)
```

```
## car_make
##             Acura        Alfa Romeo      Aston Martin              Audi 
##                18                 7                 4                40 
##           Bentley               BMW           Bugatti             Buick 
##                 8               111                 1                14 
##          Cadillac         Chevrolet          Chrysler             Dodge 
##                24                79                 6                25 
##           Ferrari              Fiat              Ford           Genesis 
##                 7                 4                73                15 
##               GMC             Honda           Hyundai          Infiniti 
##                53                45                39                14 
##            Jaguar              Jeep             Karma               Kia 
##                29                36                 1                37 
##       Lamborghini        Land Rover             Lexus           Lincoln 
##                 7                23                37                23 
##             Lotus          Maserati             Mazda     Mercedes-Benz 
##                 4                10                23                89 
##              MINI        Mitsubishi            Nissan           Porsche 
##                17                16                30                40 
##               Ram       Rolls-Royce Roush Performance            Subaru 
##                14                 8                 4                24 
##            Toyota        Volkswagen             Volvo 
##                64                20                21
```

```r
tally(~transmission, data=cars2020)
```

```
## transmission
## Automatic       CVT    Manual 
##       812       128       224
```

```r
tally(~drive, data=cars2020)
```

```
## drive
##    4WD    AWD    FWD PT 4WD    RWD 
##    164    378    298     30    294
```

```r
tally(~class, data=cars2020)
```

```
## class
##          Compact            Large     Mid St Wagon          Midsize 
##              129              103               11              171 
##      Minicompact          Minivan    Passenger Van  Sm Pickup Truck 
##               32                8                2               21 
##      Sm St Wagon           Sm SUV              SPV Std Pickup Truck 
##               30              237               24               87 
##          Std SUV       Subcompact       Two Seater 
##              139              118               52
```

```r
tally(~sidi, data=cars2020)
```

```
## sidi
##   N   Y 
## 217 947
```

```r
tally(~aspiration, data=cars2020)
```

```
## aspiration
##     Natural       Super Super+Turbo       Turbo 
##         494          41           6         623
```

```r
tally(~fuelType1, data=cars2020)
```

```
## fuelType1
##            Diesel Midgrade Gasoline  Premium Gasoline  Regular Gasoline 
##                20                12               593               539
```

```r
tally(~atvType, data=cars2020)
```

```
## atvType
##         Diesel            FFV         Hybrid           None Plug-in Hybrid 
##             20             24             78           1006             36
```

```r
tally(~startStop, data=cars2020)
```

```
## startStop
##   N   Y 
## 492 672
```

```r
## Two categoraical

tally(car_make~transmission, data=cars2020)
```

```
##                    transmission
## car_make            Automatic CVT Manual
##   Acura                    12   0      6
##   Alfa Romeo                6   0      1
##   Aston Martin              4   0      0
##   Audi                     18   0     22
##   Bentley                   3   0      5
##   BMW                      91   0     20
##   Bugatti                   0   0      1
##   Buick                    12   2      0
##   Cadillac                 24   0      0
##   Chevrolet                70   3      6
##   Chrysler                  5   1      0
##   Dodge                    20   0      5
##   Ferrari                   0   0      7
##   Fiat                      3   0      1
##   Ford                     58   6      9
##   Genesis                  14   0      1
##   GMC                      53   0      0
##   Honda                    11  25      9
##   Hyundai                  17   4     18
##   Infiniti                 10   4      0
##   Jaguar                   29   0      0
##   Jeep                     31   0      5
##   Karma                     1   0      0
##   Kia                      20   5     12
##   Lamborghini               1   0      6
##   Land Rover               23   0      0
##   Lexus                    27  10      0
##   Lincoln                  22   1      0
##   Lotus                     2   0      2
##   Maserati                 10   0      0
##   Mazda                    20   0      3
##   Mercedes-Benz            75   0     14
##   MINI                      8   0      9
##   Mitsubishi                2  12      2
##   Nissan                    7  20      3
##   Porsche                   6   0     34
##   Ram                      14   0      0
##   Rolls-Royce               8   0      0
##   Roush Performance         3   0      1
##   Subaru                    1  14      9
##   Toyota                   36  21      7
##   Volkswagen               14   0      6
##   Volvo                    21   0      0
```

```r
tally(car_make~drive, data=cars2020)
```

```
##                    drive
## car_make            4WD AWD FWD PT 4WD RWD
##   Acura               0   9   9      0   0
##   Alfa Romeo          0   3   0      0   4
##   Aston Martin        0   0   0      0   4
##   Audi                0  38   2      0   0
##   Bentley             0   8   0      0   0
##   BMW                 0  65   2      0  44
##   Bugatti             0   1   0      0   0
##   Buick               0   8   6      0   0
##   Cadillac            1  12   4      0   7
##   Chevrolet          28   6  17      0  28
##   Chrysler            0   1   3      0   2
##   Dodge               0   5   2      0  18
##   Ferrari             0   0   0      1   6
##   Fiat                0   1   1      0   2
##   Ford                0   8  18     16  31
##   Genesis             0   7   0      0   8
##   GMC                25   4   5      0  19
##   Honda               2   6  37      0   0
##   Hyundai             0   7  32      0   0
##   Infiniti            1   6   2      0   5
##   Jaguar              0  19   0      0  10
##   Jeep               12  16   7      0   1
##   Karma               0   0   0      0   1
##   Kia                 0   8  27      0   2
##   Lamborghini         0   5   0      0   2
##   Land Rover         14   9   0      0   0
##   Lexus               2  15   8      0  12
##   Lincoln             0  10   7      3   3
##   Lotus               0   0   0      0   4
##   Maserati            0   6   0      0   4
##   Mazda               8   0  13      0   2
##   Mercedes-Benz      48   0   4      0  37
##   MINI                0   6  11      0   0
##   Mitsubishi          6   1   9      0   0
##   Nissan              3   6  14      2   5
##   Porsche             6  27   0      0   7
##   Ram                 6   0   1      0   7
##   Rolls-Royce         0   2   0      0   6
##   Roush Performance   1   0   0      0   3
##   Subaru              0  21   0      0   3
##   Toyota              1  12  36      8   7
##   Volkswagen          0   5  15      0   0
##   Volvo               0  15   6      0   0
```

```r
tally(car_make~class, data=cars2020)
```

```
##                    class
## car_make            Compact Large Mid St Wagon Midsize Minicompact Minivan
##   Acura                   7     0            0       2           0       0
##   Alfa Romeo              0     0            0       3           0       0
##   Aston Martin            0     0            0       0           3       0
##   Audi                    3     4            0       8           0       0
##   Bentley                 0     0            0       1           2       0
##   BMW                    24     8            0      15           0       0
##   Bugatti                 0     0            0       0           0       0
##   Buick                   0     0            0       3           0       0
##   Cadillac                6     2            0       6           0       0
##   Chevrolet               1     2            0       2           0       0
##   Chrysler                0     3            0       0           0       3
##   Dodge                   0     6            0      12           0       1
##   Ferrari                 0     0            0       0           3       0
##   Fiat                    0     0            0       0           0       0
##   Ford                    0     0            0       8           0       0
##   Genesis                 5    10            0       0           0       0
##   GMC                     0     0            0       0           0       0
##   Honda                   6    10            0      10           0       1
##   Hyundai                 7    10            0       8           0       0
##   Infiniti                0     0            0       4           0       0
##   Jaguar                  4     0            2       5           0       0
##   Jeep                    0     0            0       0           0       0
##   Karma                   0     0            0       0           0       0
##   Kia                     1     6            0      11           0       1
##   Lamborghini             0     0            0       0           0       0
##   Land Rover              0     0            0       0           0       0
##   Lexus                   7     0            0      11           0       0
##   Lincoln                 0     6            0       5           0       0
##   Lotus                   0     0            0       0           4       0
##   Maserati                0     3            0       3           0       0
##   Mazda                   7     0            0       5           0       0
##   Mercedes-Benz          16     9            2       5           0       0
##   MINI                    0     0            0       9           3       0
##   Mitsubishi              4     0            0       0           0       0
##   Nissan                  2     0            2       9           0       0
##   Porsche                 0    18            0       0          12       0
##   Ram                     0     0            0       0           0       0
##   Rolls-Royce             1     4            2       1           0       0
##   Roush Performance       0     0            0       0           0       0
##   Subaru                  3     2            0       4           3       0
##   Toyota                 14     0            0      18           2       2
##   Volkswagen              8     0            0       1           0       0
##   Volvo                   3     0            3       2           0       0
##                    class
## car_make            Passenger Van Sm Pickup Truck Sm St Wagon Sm SUV SPV
##   Acura                         0               0           0      8   0
##   Alfa Romeo                    0               0           0      3   0
##   Aston Martin                  0               0           0      0   0
##   Audi                          0               0           1      4   0
##   Bentley                       0               0           0      0   0
##   BMW                           0               0           0     10   0
##   Bugatti                       0               0           0      0   0
##   Buick                         0               0           1      8   0
##   Cadillac                      0               0           0      8   0
##   Chevrolet                     0               8           1     11   4
##   Chrysler                      0               0           0      0   0
##   Dodge                         0               0           0      1   0
##   Ferrari                       0               0           0      0   0
##   Fiat                          0               0           1      1   0
##   Ford                          2               0           0     11   8
##   Genesis                       0               0           0      0   0
##   GMC                           0               6           0      4   4
##   Honda                         0               1           7      9   0
##   Hyundai                       0               0           0     12   0
##   Infiniti                      0               0           0      4   0
##   Jaguar                        0               0           0      6   0
##   Jeep                          0               0           0     29   0
##   Karma                         0               0           0      0   0
##   Kia                           0               0           8     10   0
##   Lamborghini                   0               0           0      0   0
##   Land Rover                    0               0           0      9   0
##   Lexus                         0               0           0      8   0
##   Lincoln                       0               0           0      6   0
##   Lotus                         0               0           0      0   0
##   Maserati                      0               0           0      0   0
##   Mazda                         0               0           0      9   0
##   Mercedes-Benz                 0               0           0     14   6
##   MINI                          0               0           0      0   0
##   Mitsubishi                    0               0           0     12   0
##   Nissan                        0               0           2      5   1
##   Porsche                       0               0           0      4   0
##   Ram                           0               0           0      0   1
##   Rolls-Royce                   0               0           0      0   0
##   Roush Performance             0               0           0      0   0
##   Subaru                        0               0           4      6   0
##   Toyota                        0               6           0     11   0
##   Volkswagen                    0               0           2      8   0
##   Volvo                         0               0           3      6   0
##                    class
## car_make            Std Pickup Truck Std SUV Subcompact Two Seater
##   Acura                            0       0          0          1
##   Alfa Romeo                       0       0          0          1
##   Aston Martin                     0       0          0          1
##   Audi                             0       5         12          3
##   Bentley                          0       3          2          0
##   BMW                              0      14         37          3
##   Bugatti                          0       0          0          1
##   Buick                            0       2          0          0
##   Cadillac                         0       2          0          0
##   Chevrolet                       23      14         12          1
##   Chrysler                         0       0          0          0
##   Dodge                            0       5          0          0
##   Ferrari                          0       0          0          4
##   Fiat                             0       0          0          2
##   Ford                            19      10         14          1
##   Genesis                          0       0          0          0
##   GMC                             22      17          0          0
##   Honda                            1       0          0          0
##   Hyundai                          0       2          0          0
##   Infiniti                         0       2          4          0
##   Jaguar                           0       0          0         12
##   Jeep                             2       5          0          0
##   Karma                            0       0          1          0
##   Kia                              0       0          0          0
##   Lamborghini                      0       1          0          6
##   Land Rover                       0      14          0          0
##   Lexus                            0       4          7          0
##   Lincoln                          0       6          0          0
##   Lotus                            0       0          0          0
##   Maserati                         0       4          0          0
##   Mazda                            0       0          0          2
##   Mercedes-Benz                    0       6         21         10
##   MINI                             0       0          5          0
##   Mitsubishi                       0       0          0          0
##   Nissan                           3       2          1          3
##   Porsche                          0       6          0          0
##   Ram                             13       0          0          0
##   Rolls-Royce                      0       0          0          0
##   Roush Performance                2       0          2          0
##   Subaru                           0       2          0          0
##   Toyota                           2       8          0          1
##   Volkswagen                       0       1          0          0
##   Volvo                            0       4          0          0
```

```r
tally(car_make~sidi, data=cars2020)
```

```
##                    sidi
## car_make              N   Y
##   Acura               1  17
##   Alfa Romeo          0   7
##   Aston Martin        4   0
##   Audi                0  40
##   Bentley             0   8
##   BMW                 0 111
##   Bugatti             1   0
##   Buick               2  12
##   Cadillac            0  24
##   Chevrolet          13  66
##   Chrysler            6   0
##   Dodge              25   0
##   Ferrari             0   7
##   Fiat                3   1
##   Ford               22  51
##   Genesis             0  15
##   GMC                 5  48
##   Honda              15  30
##   Hyundai             9  30
##   Infiniti            0  14
##   Jaguar              0  29
##   Jeep               25  11
##   Karma               0   1
##   Kia                 7  30
##   Lamborghini         2   5
##   Land Rover          3  20
##   Lexus               3  34
##   Lincoln             3  20
##   Lotus               4   0
##   Maserati            0  10
##   Mazda               0  23
##   Mercedes-Benz       2  87
##   MINI                0  17
##   Mitsubishi         12   4
##   Nissan             13  17
##   Porsche             0  40
##   Ram                14   0
##   Rolls-Royce         0   8
##   Roush Performance   2   2
##   Subaru              1  23
##   Toyota             20  44
##   Volkswagen          0  20
##   Volvo               0  21
```

```r
tally(car_make~aspiration, data=cars2020)
```

```
##                    aspiration
## car_make            Natural Super Super+Turbo Turbo
##   Acura                  13     0           0     5
##   Alfa Romeo              0     0           0     7
##   Aston Martin            0     0           0     4
##   Audi                    2     0           0    38
##   Bentley                 0     0           0     8
##   BMW                     0     0           0   111
##   Bugatti                 0     0           0     1
##   Buick                   5     0           0     9
##   Cadillac                7     0           0    17
##   Chevrolet              54     2           0    23
##   Chrysler                6     0           0     0
##   Dodge                  20     5           0     0
##   Ferrari                 2     0           0     5
##   Fiat                    0     0           0     4
##   Ford                   31     1           0    41
##   Genesis                 6     0           0     9
##   GMC                    38     0           0    15
##   Honda                  27     0           0    18
##   Hyundai                27     0           0    12
##   Infiniti                4     0           0    10
##   Jaguar                  0    14           0    15
##   Jeep                   23     1           0    12
##   Karma                   0     0           0     1
##   Kia                    25     0           0    12
##   Lamborghini             6     0           0     1
##   Land Rover              0    10           0    13
##   Lexus                  30     0           0     7
##   Lincoln                 3     0           0    20
##   Lotus                   0     4           0     0
##   Maserati                0     0           0    10
##   Mazda                  19     0           0     4
##   Mercedes-Benz           5     0           0    84
##   MINI                    0     0           0    17
##   Mitsubishi             12     0           0     4
##   Nissan                 28     0           0     2
##   Porsche                 0     0           0    40
##   Ram                    12     0           0     2
##   Rolls-Royce             0     0           0     8
##   Roush Performance       0     4           0     0
##   Subaru                 17     0           0     7
##   Toyota                 63     0           0     1
##   Volkswagen              4     0           0    16
##   Volvo                   5     0           6    10
```

```r
tally(car_make~fuelType1, data=cars2020)
```

```
##                    fuelType1
## car_make            Diesel Midgrade Gasoline Premium Gasoline Regular Gasoline
##   Acura                  0                 0               18                0
##   Alfa Romeo             0                 0                7                0
##   Aston Martin           0                 0                4                0
##   Audi                   0                 0               34                6
##   Bentley                0                 0                8                0
##   BMW                    0                 0              111                0
##   Bugatti                0                 0                1                0
##   Buick                  0                 0                4               10
##   Cadillac               0                 0               19                5
##   Chevrolet              5                 0               17               57
##   Chrysler               0                 1                0                5
##   Dodge                  0                 4               13                8
##   Ferrari                0                 0                7                0
##   Fiat                   0                 0                3                1
##   Ford                   4                 0                3               66
##   Genesis                0                 0               13                2
##   GMC                    5                 0                8               40
##   Honda                  0                 0                5               40
##   Hyundai                0                 0                0               39
##   Infiniti               0                 0               14                0
##   Jaguar                 0                 0               29                0
##   Jeep                   1                 1                2               32
##   Karma                  0                 0                1                0
##   Kia                    0                 0                5               32
##   Lamborghini            0                 0                7                0
##   Land Rover             3                 0               20                0
##   Lexus                  0                 0               26               11
##   Lincoln                0                 0                0               23
##   Lotus                  0                 0                4                0
##   Maserati               0                 0               10                0
##   Mazda                  0                 0                2               21
##   Mercedes-Benz          0                 0               89                0
##   MINI                   0                 0               17                0
##   Mitsubishi             0                 0                1               15
##   Nissan                 0                 0                8               22
##   Porsche                0                 0               40                0
##   Ram                    2                 6                0                6
##   Rolls-Royce            0                 0                8                0
##   Roush Performance      0                 0                4                0
##   Subaru                 0                 0                6               18
##   Toyota                 0                 0                3               61
##   Volkswagen             0                 0                2               18
##   Volvo                  0                 0               20                1
```

```r
tally(car_make~startStop, data=cars2020)
```

```
##                    startStop
## car_make              N   Y
##   Acura               9   9
##   Alfa Romeo          1   6
##   Aston Martin        4   0
##   Audi                4  36
##   Bentley             0   8
##   BMW                 0 111
##   Bugatti             1   0
##   Buick               5   9
##   Cadillac            2  22
##   Chevrolet          49  30
##   Chrysler            3   3
##   Dodge              23   2
##   Ferrari             0   7
##   Fiat                3   1
##   Ford               23  50
##   Genesis            13   2
##   GMC                27  26
##   Honda              38   7
##   Hyundai            29  10
##   Infiniti           14   0
##   Jaguar              0  29
##   Jeep                7  29
##   Karma               0   1
##   Kia                26  11
##   Lamborghini         4   3
##   Land Rover          0  23
##   Lexus              28   9
##   Lincoln            10  13
##   Lotus               4   0
##   Maserati            0  10
##   Mazda              23   0
##   Mercedes-Benz       3  86
##   MINI                7  10
##   Mitsubishi         15   1
##   Nissan             30   0
##   Porsche             0  40
##   Ram                 8   6
##   Rolls-Royce         8   0
##   Roush Performance   4   0
##   Subaru             19   5
##   Toyota             44  20
##   Volkswagen          4  16
##   Volvo               0  21
```

```r
library(kableExtra)
kable(cbind(tally(car_make~transmission, data=cars2020), tally(car_make~sidi,
 data=cars2020), tally(car_make~startStop, data=cars2020)), align = "cccrrrr", 
  caption = "Group Rows")%>%
  add_header_above(c(" ", "Transmission" = 3, "Spark Ignited Direct Ignition" = 2,
                     "start-stop technology" = 2))
```

<table>
<caption>Group Rows</caption>
 <thead>
<tr>
<th style="border-bottom:hidden" colspan="1"></th>
<th style="border-bottom:hidden; padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="3"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">Transmission</div></th>
<th style="border-bottom:hidden; padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">Spark Ignited Direct Ignition</div></th>
<th style="border-bottom:hidden; padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">start-stop technology</div></th>
</tr>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:center;"> Automatic </th>
   <th style="text-align:center;"> CVT </th>
   <th style="text-align:center;"> Manual </th>
   <th style="text-align:right;"> N </th>
   <th style="text-align:right;"> Y </th>
   <th style="text-align:right;"> N </th>
   <th style="text-align:right;"> Y </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Acura </td>
   <td style="text-align:center;"> 12 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 6 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 9 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Alfa Romeo </td>
   <td style="text-align:center;"> 6 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 6 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Aston Martin </td>
   <td style="text-align:center;"> 4 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Audi </td>
   <td style="text-align:center;"> 18 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 22 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 40 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 36 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bentley </td>
   <td style="text-align:center;"> 3 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 5 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 8 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> BMW </td>
   <td style="text-align:center;"> 91 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 20 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 111 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 111 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bugatti </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Buick </td>
   <td style="text-align:center;"> 12 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 9 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cadillac </td>
   <td style="text-align:center;"> 24 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 24 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 22 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chevrolet </td>
   <td style="text-align:center;"> 70 </td>
   <td style="text-align:center;"> 3 </td>
   <td style="text-align:center;"> 6 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 66 </td>
   <td style="text-align:right;"> 49 </td>
   <td style="text-align:right;"> 30 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chrysler </td>
   <td style="text-align:center;"> 5 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dodge </td>
   <td style="text-align:center;"> 20 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 5 </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ferrari </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 7 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 7 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fiat </td>
   <td style="text-align:center;"> 3 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ford </td>
   <td style="text-align:center;"> 58 </td>
   <td style="text-align:center;"> 6 </td>
   <td style="text-align:center;"> 9 </td>
   <td style="text-align:right;"> 22 </td>
   <td style="text-align:right;"> 51 </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:right;"> 50 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Genesis </td>
   <td style="text-align:center;"> 14 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> GMC </td>
   <td style="text-align:center;"> 53 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 48 </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:right;"> 26 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Honda </td>
   <td style="text-align:center;"> 11 </td>
   <td style="text-align:center;"> 25 </td>
   <td style="text-align:center;"> 9 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 38 </td>
   <td style="text-align:right;"> 7 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Hyundai </td>
   <td style="text-align:center;"> 17 </td>
   <td style="text-align:center;"> 4 </td>
   <td style="text-align:center;"> 18 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Infiniti </td>
   <td style="text-align:center;"> 10 </td>
   <td style="text-align:center;"> 4 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Jaguar </td>
   <td style="text-align:center;"> 29 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 29 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Jeep </td>
   <td style="text-align:center;"> 31 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 5 </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 29 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Karma </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kia </td>
   <td style="text-align:center;"> 20 </td>
   <td style="text-align:center;"> 5 </td>
   <td style="text-align:center;"> 12 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:right;"> 11 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lamborghini </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 6 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Land Rover </td>
   <td style="text-align:center;"> 23 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 23 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lexus </td>
   <td style="text-align:center;"> 27 </td>
   <td style="text-align:center;"> 10 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:right;"> 9 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lincoln </td>
   <td style="text-align:center;"> 22 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 13 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lotus </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Maserati </td>
   <td style="text-align:center;"> 10 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mazda </td>
   <td style="text-align:center;"> 20 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 3 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mercedes-Benz </td>
   <td style="text-align:center;"> 75 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 14 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 87 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 86 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MINI </td>
   <td style="text-align:center;"> 8 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 9 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mitsubishi </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:center;"> 12 </td>
   <td style="text-align:center;"> 2 </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nissan </td>
   <td style="text-align:center;"> 7 </td>
   <td style="text-align:center;"> 20 </td>
   <td style="text-align:center;"> 3 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Porsche </td>
   <td style="text-align:center;"> 6 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 34 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 40 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 40 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ram </td>
   <td style="text-align:center;"> 14 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 6 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Rolls-Royce </td>
   <td style="text-align:center;"> 8 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Roush Performance </td>
   <td style="text-align:center;"> 3 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Subaru </td>
   <td style="text-align:center;"> 1 </td>
   <td style="text-align:center;"> 14 </td>
   <td style="text-align:center;"> 9 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 5 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Toyota </td>
   <td style="text-align:center;"> 36 </td>
   <td style="text-align:center;"> 21 </td>
   <td style="text-align:center;"> 7 </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 20 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Volkswagen </td>
   <td style="text-align:center;"> 14 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 6 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 16 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Volvo </td>
   <td style="text-align:center;"> 21 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:center;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 21 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 21 </td>
  </tr>
</tbody>
</table>

```r
#latex, html, markdown, pandoc, and rst


##one continous variable

favstats(~mpg, data=cars2020)[c("max", "mean","sd", "n")]
```

```
##      max     mean      sd    n
##  57.7824 23.55156 6.40129 1164
```

```r
favstats(~mpg, data=cars2020)
```

```
##      min      Q1  median      Q3     max     mean      sd    n missing
##  10.5921 19.3234 22.6291 26.3573 57.7824 23.55156 6.40129 1164       0
```

```r
favstats(~gears, data=cars2020)
```

```
##  min Q1 median Q3 max     mean       sd    n missing
##    1  6      8  8  10 7.303265 1.972718 1164       0
```

```r
favstats(~displ, data=cars2020)
```

```
##  min Q1 median  Q3 max     mean      sd    n missing
##    1  2      3 3.6   8 3.076546 1.29394 1164       0
```

```r
favstats(~cylinders, data=cars2020)
```

```
##  min Q1 median Q3 max     mean       sd    n missing
##    3  4      6  6  16 5.569588 1.826848 1164       0
```

```r
##one continous one categorical

favstats(mpg~ car_make, data=cars2020)
```

```
##             car_make     min       Q1   median       Q3     max     mean
## 1              Acura 21.0000 22.94900 23.25380 25.75972 28.0000 24.07169
## 2         Alfa Romeo 19.1410 22.36360 25.32660 26.24905 27.9406 24.23336
## 3       Aston Martin 17.0315 17.54953 19.06770 20.41378 20.4155 18.89560
## 4               Audi 15.2575 20.38198 22.65465 25.99520 30.1793 22.71765
## 5            Bentley 13.6613 14.66160 16.16640 18.79032 18.9656 16.49755
## 6                BMW 15.0983 18.81510 22.69820 25.15485 29.6330 22.32187
## 7            Bugatti 10.5921 10.59210 10.59210 10.59210 10.5921 10.59210
## 8              Buick 20.0162 22.45845 24.54265 26.71392 30.7314 24.78502
## 9           Cadillac 16.5637 20.35020 22.20880 24.23393 27.1593 22.17932
## 10         Chevrolet 14.9623 17.00000 19.61160 23.30990 32.9630 20.72874
## 11          Chrysler 19.0620 21.61542 22.24520 22.64615 29.5207 22.87640
## 12             Dodge 15.0483 16.68870 17.76080 20.77540 22.7798 18.36674
## 13           Ferrari 13.3412 14.85070 16.61150 16.90515 18.1715 15.94799
## 14              Fiat 24.9382 25.71632 27.63775 29.39140 29.6662 27.46997
## 15              Ford 14.0853 18.86980 22.85180 24.81560 42.0000 23.24922
## 16           Genesis 17.7991 19.19875 20.09290 20.70120 24.8653 20.26011
## 17               GMC 14.9623 16.94260 18.15210 21.53040 27.4108 19.33331
## 18             Honda 20.5738 25.95250 30.25000 33.23830 51.9827 30.68666
## 19           Hyundai 21.1618 24.89410 29.50960 32.19735 57.7824 31.29421
## 20          Infiniti 15.3391 21.70990 21.97385 22.41270 26.1476 21.62992
## 21            Jaguar 18.1891 20.86350 23.42660 25.66260 28.4037 23.14366
## 22              Jeep 13.3061 20.44058 22.19925 24.75897 26.9515 22.08787
## 23             Karma 25.8000 25.80000 25.80000 25.80000 25.8000 25.80000
## 24               Kia 19.9185 22.95260 27.05040 31.34930 50.4101 28.96986
## 25       Lamborghini 10.7137 12.43430 14.98990 14.99780 15.0057 13.65336
## 26        Land Rover 15.1147 18.09170 20.24570 22.40790 24.3843 20.11077
## 27             Lexus 13.8684 21.00000 22.82060 25.78180 44.0943 24.77066
## 28           Lincoln 17.5802 19.45870 20.98220 23.06780 41.0000 21.87267
## 29             Lotus 18.9178 19.42592 19.71800 19.88577 20.0210 19.59370
## 30          Maserati 15.3607 17.11340 17.75225 19.16457 19.3234 17.71110
## 31             Mazda 22.6002 26.74620 28.32850 29.67095 34.9351 28.36609
## 32     Mercedes-Benz 13.8444 19.69810 21.72890 23.77550 28.4696 21.69211
## 33              MINI 25.8460 27.37610 28.90900 29.79170 31.3799 28.61675
## 34        Mitsubishi 22.0000 25.31330 26.07675 29.26265 38.6074 28.41024
## 35            Nissan 15.0000 20.92748 24.69735 29.75000 35.0880 25.06172
## 36           Porsche 16.5714 19.93535 20.05000 20.98852 22.5030 20.27934
## 37               Ram 16.8732 17.69468 19.36585 22.39708 26.0473 20.31741
## 38       Rolls-Royce 13.8915 13.89150 14.24320 14.32477 14.3910 14.15570
## 39 Roush Performance 12.6756 12.67560 13.45995 14.50355 15.2813 13.71920
## 40            Subaru 18.6117 23.61950 26.33855 29.86630 35.1000 26.60871
## 41            Toyota 14.2639 22.44510 29.04465 34.45507 55.7000 30.08026
## 42        Volkswagen 18.5818 21.28947 24.96860 27.80837 34.0525 25.16221
## 43             Volvo 20.7216 24.41490 25.04910 26.80000 30.3000 25.61402
##            sd   n missing
## 1   2.1752314  18       0
## 2   3.3010997   7       0
## 3   1.7762265   4       0
## 4   3.8987561  40       0
## 5   2.2336328   8       0
## 6   3.8143732 111       0
## 7          NA   1       0
## 8   2.9760769  14       0
## 9   2.9424019  24       0
## 10  4.7512070  79       0
## 11  3.5118677   6       0
## 12  2.4610517  25       0
## 13  1.8588942   7       0
## 14  2.3674463   4       0
## 15  6.3652742  73       0
## 16  1.8665636  15       0
## 17  3.1824577  53       0
## 18  7.0546366  45       0
## 19  9.2785626  39       0
## 20  2.8580110  14       0
## 21  3.1570395  29       0
## 22  3.1179285  36       0
## 23         NA   1       0
## 24  8.5169456  37       0
## 25  1.9721285   7       0
## 26  2.6820479  23       0
## 27  6.4340354  37       0
## 28  4.6757040  23       0
## 29  0.4831969   4       0
## 30  1.5327897  10       0
## 31  2.8371422  23       0
## 32  3.3337919  89       0
## 33  1.8526389  17       0
## 34  5.2112248  16       0
## 35  5.7281523  30       0
## 36  1.3217165  40       0
## 37  2.9959636  14       0
## 38  0.2280787   8       0
## 39  1.2772481   4       0
## 40  3.8769619  24       0
## 41 10.6627305  64       0
## 42  4.9693528  20       0
## 43  2.4995829  21       0
```

```r
favstats(mpg~ transmission, data=cars2020)
```

```
##   transmission     min       Q1   median       Q3     max     mean       sd   n
## 1    Automatic 12.6756 18.74643 21.83710 24.51057 35.0000 21.79283 3.955858 812
## 2          CVT 20.5416 28.21187 31.70975 35.66655 55.7000 33.04857 7.548859 128
## 3       Manual 10.5921 19.26342 23.07110 28.29508 57.7824 24.50010 7.771829 224
##   missing
## 1       0
## 2       0
## 3       0
```

```r
favstats(mpg~ drive, data=cars2020)
```

```
##    drive     min       Q1   median       Q3     max     mean       sd   n
## 1    4WD 12.6756 17.16615 20.03900 22.56383 29.1658 20.21910 3.668326 164
## 2    AWD 10.5921 19.90560 22.39135 24.93400 40.1750 22.36506 4.331883 378
## 3    FWD 14.9383 24.82985 28.40920 32.44302 57.7824 29.81908 7.405999 298
## 4 PT 4WD 13.3849 17.63443 18.83200 21.48560 50.0000 20.77963 7.196986  30
## 5    RWD 12.6756 17.76080 20.22140 23.27003 29.7239 20.86606 3.779483 294
##   missing
## 1       0
## 2       0
## 3       0
## 4       0
## 5       0
```

```r
favstats(mpg~ class, data=cars2020)
```

```
##               class     min       Q1   median       Q3     max     mean
## 1           Compact 13.8915 23.19480 27.01130 30.91110 52.0000 27.49157
## 2             Large 13.8915 19.28750 21.00000 25.06490 57.7824 23.37516
## 3      Mid St Wagon 14.3027 19.68190 22.90020 23.84745 26.0000 21.30856
## 4           Midsize 14.1837 22.58530 26.14910 30.37180 55.7000 27.71872
## 5       Minicompact 13.3849 19.43788 20.00000 21.08908 31.3799 20.92383
## 6           Minivan 19.9421 20.43483 21.79360 22.29650 29.5207 22.30169
## 7     Passenger Van 15.8294 16.02777 16.22615 16.42453 16.6229 16.22615
## 8   Sm Pickup Truck 16.6991 19.41600 20.64570 21.85340 23.3948 20.57527
## 9       Sm St Wagon 23.4414 26.55780 28.81495 30.94125 50.4101 30.88478
## 10           Sm SUV 16.1587 21.99030 23.80190 25.94230 40.5365 24.16257
## 11              SPV 14.9623 16.56610 21.83010 22.79840 25.6715 20.45617
## 12 Std Pickup Truck 12.6756 17.00000 18.63500 20.30245 26.5885 19.04348
## 13          Std SUV 13.3061 16.68870 17.74250 20.93030 35.0205 18.94294
## 14       Subcompact 14.0853 19.70100 22.69175 24.98710 32.9630 22.74225
## 15       Two Seater 10.5921 16.72445 20.12650 23.74470 29.7239 20.30205
##           sd   n missing
## 1  5.9860021 129       0
## 2  7.9158575 103       0
## 3  3.9909745  11       0
## 4  8.3817478 171       0
## 5  3.9945048  32       0
## 6  3.0812029   8       0
## 7  0.5610892   2       0
## 8  1.6946128  21       0
## 9  7.1356223  30       0
## 10 3.7389130 237       0
## 11 3.9407017  24       0
## 12 2.9037640  87       0
## 13 3.7249644 139       0
## 14 4.1334848 118       0
## 15 5.1427132  52       0
```

```r
favstats(mpg~ aspiration, data=cars2020)
```

```
##    aspiration     min       Q1  median       Q3     max     mean       sd   n
## 1     Natural 10.7137 18.66238 22.6291 29.25325 57.7824 24.85538 8.365734 494
## 2       Super 12.6756 15.57740 18.1891 20.02100 22.6919 17.88819 2.823748  41
## 3 Super+Turbo 20.7216 23.05193 24.2162 24.74740 25.0491 23.63190 1.648968   6
## 4       Turbo 10.5921 20.04260 22.9512 25.69235 36.0339 22.88966 4.087568 623
##   missing
## 1       0
## 2       0
## 3       0
## 4       0
```

```r
favstats(mpg~ fuelType1, data=cars2020)
```

```
##           fuelType1     min       Q1  median       Q3     max     mean       sd
## 1            Diesel 19.4160 23.21720 24.0249 24.46850 26.5885 23.82984 1.628788
## 2 Midgrade Gasoline 16.6887 16.91152 17.3101 19.02563 19.0620 17.81245 1.057577
## 3  Premium Gasoline 10.5921 18.48220 21.6894 24.70460 32.0168 21.58884 4.146422
## 4  Regular Gasoline 14.2639 20.54740 24.2002 29.63450 57.7824 25.82837 7.705009
##     n missing
## 1  20       0
## 2  12       0
## 3 593       0
## 4 539       0
```

```r
favstats(mpg~ atvType, data=cars2020)
```

```
##          atvType     min       Q1   median       Q3     max     mean        sd
## 1         Diesel 19.4160 23.21720 24.02490 24.46850 26.5885 23.82984  1.628788
## 2            FFV 15.7301 16.36830 17.56585 18.78630 25.6715 18.31084  2.724908
## 3         Hybrid 15.2575 21.50252 27.79440 42.89655 57.7824 32.35495 12.246403
## 4           None 10.5921 19.21265 22.45000 25.96990 38.6074 22.79603  4.936485
## 5 Plug-in Hybrid 18.7319 22.50300 26.57015 30.30000 54.4329 28.92968  9.372533
##      n missing
## 1   20       0
## 2   24       0
## 3   78       0
## 4 1006       0
## 5   36       0
```

```r
##one continous two categorical
favstats(mpg~ car_make+transmission, data=cars2020)
```

```
##           car_make.transmission     min       Q1   median       Q3     max
## 1               Acura.Automatic 21.3495 22.91180 23.02550 23.39275 24.0000
## 2          Alfa Romeo.Automatic 19.1410 21.33900 24.86970 25.48912 26.9548
## 3        Aston Martin.Automatic 17.0315 17.54953 19.06770 20.41378 20.4155
## 4                Audi.Automatic 15.2575 18.02725 21.05285 22.41137 24.2823
## 5             Bentley.Automatic 13.6613 15.49950 17.33770 18.03480 18.7319
## 6                 BMW.Automatic 15.0983 19.59150 24.38790 25.68145 29.6330
## 7             Bugatti.Automatic      NA       NA       NA       NA      NA
## 8               Buick.Automatic 20.0162 21.96215 24.38220 25.88982 27.5574
## 9            Cadillac.Automatic 16.5637 20.35020 22.20880 24.23393 27.1593
## 10          Chevrolet.Automatic 14.9623 16.96363 19.32325 22.21872 28.8278
## 11           Chrysler.Automatic 19.0620 21.40550 22.24520 22.24520 22.7798
## 12              Dodge.Automatic 15.0483 16.86540 18.41140 21.20870 22.7798
## 13            Ferrari.Automatic      NA       NA       NA       NA      NA
## 14               Fiat.Automatic 24.9382 25.45695 25.97570 27.63775 29.2998
## 15               Ford.Automatic 15.8294 19.00000 22.26840 24.33380 30.2418
## 16            Genesis.Automatic 17.7991 19.00538 20.04385 20.23080 24.8653
## 17                GMC.Automatic 14.9623 16.94260 18.15210 21.53040 27.4108
## 18              Honda.Automatic 20.5738 21.50430 22.05810 22.57630 27.0324
## 19            Hyundai.Automatic 21.1618 23.25740 24.63670 29.74900 31.7911
## 20           Infiniti.Automatic 15.3391 21.19285 21.90640 22.02340 23.0508
## 21             Jaguar.Automatic 18.1891 20.86350 23.42660 25.66260 28.4037
## 22               Jeep.Automatic 13.3061 20.73180 22.20860 24.45115 26.9515
## 23              Karma.Automatic 25.8000 25.80000 25.80000 25.80000 25.8000
## 24                Kia.Automatic 19.9185 20.88543 23.00380 24.27717 28.7747
## 25        Lamborghini.Automatic 13.9329 13.93290 13.93290 13.93290 13.9329
## 26         Land Rover.Automatic 15.1147 18.09170 20.24570 22.40790 24.3843
## 27              Lexus.Automatic 13.8684 21.00000 21.82850 23.28730 25.5162
## 28            Lincoln.Automatic 17.5802 19.35515 20.94720 22.95000 24.6965
## 29              Lotus.Automatic 19.5953 19.65665 19.71800 19.77935 19.8407
## 30           Maserati.Automatic 15.3607 17.11340 17.75225 19.16457 19.3234
## 31              Mazda.Automatic 22.6002 26.47297 28.11020 29.64447 34.9351
## 32      Mercedes-Benz.Automatic 13.8444 20.05020 21.71460 23.23305 28.3488
## 33               MINI.Automatic 25.8460 26.12343 26.88320 28.38115 29.4601
## 34         Mitsubishi.Automatic 22.0000 22.75000 23.50000 24.25000 25.0000
## 35             Nissan.Automatic 15.0000 16.53575 17.79710 19.46065 21.9494
## 36            Porsche.Automatic 16.5714 17.51053 19.43030 20.12097 20.4628
## 37                Ram.Automatic 16.8732 17.69468 19.36585 22.39708 26.0473
## 38        Rolls-Royce.Automatic 13.8915 13.89150 14.24320 14.32477 14.3910
## 39  Roush Performance.Automatic 12.6756 12.67560 12.67560 13.45995 14.2443
## 40             Subaru.Automatic 27.4825 27.48250 27.48250 27.48250 27.4825
## 41             Toyota.Automatic 14.2639 20.00207 25.02840 28.21743 35.0000
## 42         Volkswagen.Automatic 18.5818 19.43052 22.56705 24.99790 34.0525
## 43              Volvo.Automatic 20.7216 24.41490 25.04910 26.80000 30.3000
## 44                    Acura.CVT      NA       NA       NA       NA      NA
## 45               Alfa Romeo.CVT      NA       NA       NA       NA      NA
## 46             Aston Martin.CVT      NA       NA       NA       NA      NA
## 47                     Audi.CVT      NA       NA       NA       NA      NA
## 48                  Bentley.CVT      NA       NA       NA       NA      NA
## 49                      BMW.CVT      NA       NA       NA       NA      NA
## 50                  Bugatti.CVT      NA       NA       NA       NA      NA
## 51                    Buick.CVT 27.9466 28.64280 29.33900 30.03520 30.7314
## 52                 Cadillac.CVT      NA       NA       NA       NA      NA
## 53                Chevrolet.CVT 31.5018 32.23240 32.96300 32.96300 32.9630
## 54                 Chrysler.CVT 29.5207 29.52070 29.52070 29.52070 29.5207
## 55                    Dodge.CVT      NA       NA       NA       NA      NA
## 56                  Ferrari.CVT      NA       NA       NA       NA      NA
## 57                     Fiat.CVT      NA       NA       NA       NA      NA
## 58                     Ford.CVT 39.7753 40.65237 41.25000 41.65000 42.0000
## 59                  Genesis.CVT      NA       NA       NA       NA      NA
## 60                      GMC.CVT      NA       NA       NA       NA      NA
## 61                    Honda.CVT 28.2825 31.34820 33.02740 35.60820 51.9827
## 62                  Hyundai.CVT 31.5397 33.38493 34.61895 35.48525 36.2273
## 63                 Infiniti.CVT 21.6894 22.14885 23.69655 25.35523 26.1476
## 64                   Jaguar.CVT      NA       NA       NA       NA      NA
## 65                     Jeep.CVT      NA       NA       NA       NA      NA
## 66                    Karma.CVT      NA       NA       NA       NA      NA
## 67                      Kia.CVT 29.6510 31.34930 33.33950 34.70780 35.8416
## 68              Lamborghini.CVT      NA       NA       NA       NA      NA
## 69               Land Rover.CVT      NA       NA       NA       NA      NA
## 70                    Lexus.CVT 25.7818 29.15452 30.37870 37.38065 44.0943
## 71                  Lincoln.CVT 41.0000 41.00000 41.00000 41.00000 41.0000
## 72                    Lotus.CVT      NA       NA       NA       NA      NA
## 73                 Maserati.CVT      NA       NA       NA       NA      NA
## 74                    Mazda.CVT      NA       NA       NA       NA      NA
## 75            Mercedes-Benz.CVT      NA       NA       NA       NA      NA
## 76                     MINI.CVT      NA       NA       NA       NA      NA
## 77               Mitsubishi.CVT 25.0000 25.53288 26.07675 27.08755 38.6074
## 78                   Nissan.CVT 21.4539 23.97770 28.28915 31.16633 35.0880
## 79                  Porsche.CVT      NA       NA       NA       NA      NA
## 80                      Ram.CVT      NA       NA       NA       NA      NA
## 81              Rolls-Royce.CVT      NA       NA       NA       NA      NA
## 82        Roush Performance.CVT      NA       NA       NA       NA      NA
## 83                   Subaru.CVT 20.5416 26.08980 29.29480 30.39447 35.1000
## 84                   Toyota.CVT 28.8126 34.27930 40.17500 50.00000 55.7000
## 85               Volkswagen.CVT      NA       NA       NA       NA      NA
## 86                    Volvo.CVT      NA       NA       NA       NA      NA
## 87                 Acura.Manual 21.0000 26.47780 26.92955 27.69037 28.0000
## 88            Alfa Romeo.Manual 27.9406 27.94060 27.94060 27.94060 27.9406
## 89          Aston Martin.Manual      NA       NA       NA       NA      NA
## 90                  Audi.Manual 15.9153 24.24600 25.51915 26.74235 30.1793
## 91               Bentley.Manual 14.6616 14.66160 14.99510 18.96560 18.9656
## 92                   BMW.Manual 18.5025 18.81510 19.99745 21.65480 25.3226
## 93               Bugatti.Manual 10.5921 10.59210 10.59210 10.59210 10.5921
## 94                 Buick.Manual      NA       NA       NA       NA      NA
## 95              Cadillac.Manual      NA       NA       NA       NA      NA
## 96             Chevrolet.Manual 15.7384 18.89895 21.53390 29.83953 32.5457
## 97              Chrysler.Manual      NA       NA       NA       NA      NA
## 98                 Dodge.Manual 15.5775 15.57750 16.91490 16.91490 17.9612
## 99               Ferrari.Manual 13.3412 14.85070 16.61150 16.90515 18.1715
## 100                 Fiat.Manual 29.6662 29.66620 29.66620 29.66620 29.6662
## 101                 Ford.Manual 14.0853 16.19760 18.34570 22.85180 24.3461
## 102              Genesis.Manual 21.6779 21.67790 21.67790 21.67790 21.6779
## 103                  GMC.Manual      NA       NA       NA       NA      NA
## 104                Honda.Manual 24.5785 29.07740 29.71430 30.00000 32.0168
## 105              Hyundai.Manual 24.7112 28.41645 30.05905 44.42865 57.7824
## 106             Infiniti.Manual      NA       NA       NA       NA      NA
## 107               Jaguar.Manual      NA       NA       NA       NA      NA
## 108                 Jeep.Manual 18.7070 19.33980 19.90110 25.03660 26.1675
## 109                Karma.Manual      NA       NA       NA       NA      NA
## 110                  Kia.Manual 27.0504 29.92597 36.05195 43.98442 50.4101
## 111          Lamborghini.Manual 10.7137 11.94925 14.98990 15.00175 15.0057
## 112           Land Rover.Manual      NA       NA       NA       NA      NA
## 113                Lexus.Manual      NA       NA       NA       NA      NA
## 114              Lincoln.Manual      NA       NA       NA       NA      NA
## 115                Lotus.Manual 18.9178 19.19360 19.46940 19.74520 20.0210
## 116             Maserati.Manual      NA       NA       NA       NA      NA
## 117                Mazda.Manual 29.0373 29.11010 29.18290 31.63065 34.0784
## 118        Mercedes-Benz.Manual 16.7621 17.61758 25.27310 26.44533 28.4696
## 119                 MINI.Manual 27.5119 29.03130 29.79170 31.37990 31.3799
## 120           Mitsubishi.Manual 35.0000 35.34950 35.69900 36.04850 36.3980
## 121               Nissan.Manual 18.3595 19.15845 19.95740 24.97870 30.0000
## 122              Porsche.Manual 18.2114 20.00000 20.13870 21.00000 22.5030
## 123                  Ram.Manual      NA       NA       NA       NA      NA
## 124          Rolls-Royce.Manual      NA       NA       NA       NA      NA
## 125    Roush Performance.Manual 15.2813 15.28130 15.28130 15.28130 15.2813
## 126               Subaru.Manual 18.6117 23.14220 24.93740 26.22350 26.4104
## 127               Toyota.Manual 18.3106 21.10525 31.46640 32.46950 34.2683
## 128           Volkswagen.Manual 27.0113 27.21503 27.89105 30.19742 34.0383
## 129                Volvo.Manual      NA       NA       NA       NA      NA
##         mean         sd  n missing
## 1   23.01335  0.7240967 12       0
## 2   23.61548  3.1416333  6       0
## 3   18.89560  1.7762265  4       0
## 4   20.16287  2.6788769 18       0
## 5   16.57697  2.6195004  3       0
## 6   22.73364  4.0014768 91       0
## 7        NaN         NA  0       0
## 8   24.02603  2.3907689 12       0
## 9   22.17932  2.9424019 24       0
## 10  19.97375  3.8098039 70       0
## 11  21.54754  1.4739410  5       0
## 12  18.81113  2.5285027 20       0
## 13       NaN         NA  0       0
## 14  26.73790  2.2785083  3       0
## 15  22.04350  3.3541903 58       0
## 16  20.15884  1.8937776 14       0
## 17  19.33331  3.1824577 53       0
## 18  22.62464  2.0395735 11       0
## 19  25.85100  3.5502762 17       0
## 20  20.75888  2.7029904 10       0
## 21  23.14366  3.1570395 29       0
## 22  22.12939  3.1149947 31       0
## 23  25.80000         NA  1       0
## 24  23.00007  2.4033674 20       0
## 25  13.93290         NA  1       0
## 26  20.11077  2.6820479 23       0
## 27  21.69416  2.6580654 27       0
## 28  21.00324  2.1655734 22       0
## 29  19.71800  0.1735240  2       0
## 30  17.71110  1.5327897 10       0
## 31  28.00607  2.7217048 20       0
## 32  21.52698  2.9986884 75       0
## 33  27.30197  1.4057506  8       0
## 34  23.50000  2.1213203  2       0
## 35  18.10561  2.4793187  7       0
## 36  18.85277  1.6811553  6       0
## 37  20.31741  2.9959636 14       0
## 38  14.15570  0.2280787  8       0
## 39  13.19850  0.9056894  3       0
## 40  27.48250         NA  1       0
## 41  23.93826  5.9176532 36       0
## 42  23.46119  4.7631624 14       0
## 43  25.61402  2.4995829 21       0
## 44       NaN         NA  0       0
## 45       NaN         NA  0       0
## 46       NaN         NA  0       0
## 47       NaN         NA  0       0
## 48       NaN         NA  0       0
## 49       NaN         NA  0       0
## 50       NaN         NA  0       0
## 51  29.33900  1.9691510  2       0
## 52       NaN         NA  0       0
## 53  32.47593  0.8436242  3       0
## 54  29.52070         NA  1       0
## 55       NaN         NA  0       0
## 56       NaN         NA  0       0
## 57       NaN         NA  0       0
## 58  41.08530  0.8259620  6       0
## 59       NaN         NA  0       0
## 60       NaN         NA  0       0
## 61  34.82176  6.2429633 25       0
## 62  34.25122  2.0243420  4       0
## 63  23.80752  2.1507034  4       0
## 64       NaN         NA  0       0
## 65       NaN         NA  0       0
## 66       NaN         NA  0       0
## 67  32.97784  2.5022334  5       0
## 68       NaN         NA  0       0
## 69       NaN         NA  0       0
## 70  33.07721  6.3337566 10       0
## 71  41.00000         NA  1       0
## 72       NaN         NA  0       0
## 73       NaN         NA  0       0
## 74       NaN         NA  0       0
## 75       NaN         NA  0       0
## 76       NaN         NA  0       0
## 77  28.01383  4.7243765 12       0
## 78  27.83977  4.1436982 20       0
## 79       NaN         NA  0       0
## 80       NaN         NA  0       0
## 81       NaN         NA  0       0
## 82       NaN         NA  0       0
## 83  28.10805  4.0325070 14       0
## 84  41.53163  8.6027520 21       0
## 85       NaN         NA  0       0
## 86       NaN         NA  0       0
## 87  26.18838  2.6209508  6       0
## 88  27.94060         NA  1       0
## 89       NaN         NA  0       0
## 90  24.80793  3.4996936 22       0
## 91  16.44990  2.3005418  5       0
## 92  20.44831  1.9700537 20       0
## 93  10.59210         NA  1       0
## 94       NaN         NA  0       0
## 95       NaN         NA  0       0
## 96  23.66343  7.1031392  6       0
## 97       NaN         NA  0       0
## 98  16.58920  1.0175484  5       0
## 99  15.94799  1.8588942  7       0
## 100 29.66620         NA  1       0
## 101 19.12871  3.9785122  9       0
## 102 21.67790         NA  1       0
## 103      NaN         NA  0       0
## 104 29.05384  2.4074971  9       0
## 105 35.77791 11.2641938 18       0
## 106      NaN         NA  0       0
## 107      NaN         NA  0       0
## 108 21.83040  3.4918143  5       0
## 109      NaN         NA  0       0
## 110 37.24952  8.8465150 12       0
## 111 13.60677  2.1561343  6       0
## 112      NaN         NA  0       0
## 113      NaN         NA  0       0
## 114      NaN         NA  0       0
## 115 19.46940  0.7800802  2       0
## 116      NaN         NA  0       0
## 117 30.76620  2.8693730  3       0
## 118 22.57678  4.8008565 14       0
## 119 29.78544  1.3675572  9       0
## 120 35.69900  0.9885353  2       0
## 121 22.77230  6.3101550  3       0
## 122 20.53109  1.0959089 34       0
## 123      NaN         NA  0       0
## 124      NaN         NA  0       0
## 125 15.28130         NA  1       0
## 126 24.17932  2.4739036  9       0
## 127 27.31354  6.9695383  7       0
## 128 29.13127  2.8009049  6       0
## 129      NaN         NA  0       0
```

```r
#favstats(mpg~ car_make+aspiration, data=cars2020)
#favstats(mpg~ car_make+fuelType1, data=cars2020)
```


