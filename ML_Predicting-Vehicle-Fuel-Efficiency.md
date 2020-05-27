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
favstats(mpg~ car_make+aspiration, data=cars2020)
```

```
##               car_make.aspiration     min       Q1   median       Q3     max
## 1                   Acura.Natural 21.3495 23.00000 23.31090 26.87230 28.0000
## 2              Alfa Romeo.Natural      NA       NA       NA       NA      NA
## 3            Aston Martin.Natural      NA       NA       NA       NA      NA
## 4                    Audi.Natural 15.9153 15.91530 15.91530 15.91530 15.9153
## 5                 Bentley.Natural      NA       NA       NA       NA      NA
## 6                     BMW.Natural      NA       NA       NA       NA      NA
## 7                 Bugatti.Natural      NA       NA       NA       NA      NA
## 8                   Buick.Natural 20.0162 21.16480 21.63800 23.62320 24.7031
## 9                Cadillac.Natural 16.5637 18.58955 20.34840 20.71205 21.1796
## 10              Chevrolet.Natural 14.9623 16.88395 17.59140 20.15185 32.9630
## 11               Chrysler.Natural 19.0620 21.61542 22.24520 22.64615 29.5207
## 12                  Dodge.Natural 15.1196 17.55167 18.51160 21.20870 22.7798
## 13                Ferrari.Natural 13.3412 13.35213 13.36305 13.37397 13.3849
## 14                   Fiat.Natural      NA       NA       NA       NA      NA
## 15                   Ford.Natural 15.8294 18.34470 22.26840 25.67150 42.0000
## 16                Genesis.Natural 17.7991 18.15273 18.70215 19.79075 21.1622
## 17                    GMC.Natural 14.9623 16.74050 17.21345 18.48713 23.0837
## 18                  Honda.Natural 20.5738 22.40200 30.25000 33.18840 51.9827
## 19                Hyundai.Natural 21.1618 24.85685 29.90530 34.61895 57.7824
## 20               Infiniti.Natural 15.3391 15.93520 18.91165 21.84255 22.3020
## 21                 Jaguar.Natural      NA       NA       NA       NA      NA
## 22                   Jeep.Natural 14.9466 20.12200 21.26260 24.52690 26.1675
## 23                  Karma.Natural      NA       NA       NA       NA      NA
## 24                    Kia.Natural 20.4440 23.32430 28.77470 35.84160 50.4101
## 25            Lamborghini.Natural 10.7137 11.94925 14.98990 15.00175 15.0057
## 26             Land Rover.Natural      NA       NA       NA       NA      NA
## 27                  Lexus.Natural 13.8684 21.00000 22.72250 28.73418 44.0943
## 28                Lincoln.Natural 19.2516 19.70620 20.16080 30.58040 41.0000
## 29                  Lotus.Natural      NA       NA       NA       NA      NA
## 30               Maserati.Natural      NA       NA       NA       NA      NA
## 31                  Mazda.Natural 26.2462 27.72020 29.16580 29.75610 34.9351
## 32          Mercedes-Benz.Natural 20.9556 22.55170 23.36070 23.46070 24.0100
## 33                   MINI.Natural      NA       NA       NA       NA      NA
## 34             Mitsubishi.Natural 22.0000 25.16500 26.25040 35.34950 38.6074
## 35                 Nissan.Natural 15.0000 21.27842 24.69735 30.05203 35.0880
## 36                Porsche.Natural      NA       NA       NA       NA      NA
## 37                    Ram.Natural 16.8732 17.33150 18.87310 21.38210 23.7776
## 38            Rolls-Royce.Natural      NA       NA       NA       NA      NA
## 39      Roush Performance.Natural      NA       NA       NA       NA      NA
## 40                 Subaru.Natural 22.9745 26.22350 28.68950 30.24890 35.1000
## 41                 Toyota.Natural 14.2639 22.07740 29.27670 34.63085 55.7000
## 42             Volkswagen.Natural 18.5818 18.58180 18.89640 19.21100 19.2110
## 43                  Volvo.Natural 26.6000 26.80000 29.70000 30.30000 30.3000
## 44                    Acura.Super      NA       NA       NA       NA      NA
## 45               Alfa Romeo.Super      NA       NA       NA       NA      NA
## 46             Aston Martin.Super      NA       NA       NA       NA      NA
## 47                     Audi.Super      NA       NA       NA       NA      NA
## 48                  Bentley.Super      NA       NA       NA       NA      NA
## 49                      BMW.Super      NA       NA       NA       NA      NA
## 50                  Bugatti.Super      NA       NA       NA       NA      NA
## 51                    Buick.Super      NA       NA       NA       NA      NA
## 52                 Cadillac.Super      NA       NA       NA       NA      NA
## 53                Chevrolet.Super 15.5519 15.59852 15.64515 15.69177 15.7384
## 54                 Chrysler.Super      NA       NA       NA       NA      NA
## 55                    Dodge.Super 15.0483 15.36730 15.57750 15.57750 16.2222
## 56                  Ferrari.Super      NA       NA       NA       NA      NA
## 57                     Fiat.Super      NA       NA       NA       NA      NA
## 58                     Ford.Super 14.0853 14.08530 14.08530 14.08530 14.0853
## 59                  Genesis.Super      NA       NA       NA       NA      NA
## 60                      GMC.Super      NA       NA       NA       NA      NA
## 61                    Honda.Super      NA       NA       NA       NA      NA
## 62                  Hyundai.Super      NA       NA       NA       NA      NA
## 63                 Infiniti.Super      NA       NA       NA       NA      NA
## 64                   Jaguar.Super 18.1891 18.44890 20.82105 21.83710 22.6919
## 65                     Jeep.Super 13.3061 13.30610 13.30610 13.30610 13.3061
## 66                    Karma.Super      NA       NA       NA       NA      NA
## 67                      Kia.Super      NA       NA       NA       NA      NA
## 68              Lamborghini.Super      NA       NA       NA       NA      NA
## 69               Land Rover.Super 15.1147 16.67377 17.90440 18.55190 20.2457
## 70                    Lexus.Super      NA       NA       NA       NA      NA
## 71                  Lincoln.Super      NA       NA       NA       NA      NA
## 72                    Lotus.Super 18.9178 19.42592 19.71800 19.88577 20.0210
## 73                 Maserati.Super      NA       NA       NA       NA      NA
## 74                    Mazda.Super      NA       NA       NA       NA      NA
## 75            Mercedes-Benz.Super      NA       NA       NA       NA      NA
## 76                     MINI.Super      NA       NA       NA       NA      NA
## 77               Mitsubishi.Super      NA       NA       NA       NA      NA
## 78                   Nissan.Super      NA       NA       NA       NA      NA
## 79                  Porsche.Super      NA       NA       NA       NA      NA
## 80                      Ram.Super      NA       NA       NA       NA      NA
## 81              Rolls-Royce.Super      NA       NA       NA       NA      NA
## 82        Roush Performance.Super 12.6756 12.67560 13.45995 14.50355 15.2813
## 83                   Subaru.Super      NA       NA       NA       NA      NA
## 84                   Toyota.Super      NA       NA       NA       NA      NA
## 85               Volkswagen.Super      NA       NA       NA       NA      NA
## 86                    Volvo.Super      NA       NA       NA       NA      NA
## 87              Acura.Super+Turbo      NA       NA       NA       NA      NA
## 88         Alfa Romeo.Super+Turbo      NA       NA       NA       NA      NA
## 89       Aston Martin.Super+Turbo      NA       NA       NA       NA      NA
## 90               Audi.Super+Turbo      NA       NA       NA       NA      NA
## 91            Bentley.Super+Turbo      NA       NA       NA       NA      NA
## 92                BMW.Super+Turbo      NA       NA       NA       NA      NA
## 93            Bugatti.Super+Turbo      NA       NA       NA       NA      NA
## 94              Buick.Super+Turbo      NA       NA       NA       NA      NA
## 95           Cadillac.Super+Turbo      NA       NA       NA       NA      NA
## 96          Chevrolet.Super+Turbo      NA       NA       NA       NA      NA
## 97           Chrysler.Super+Turbo      NA       NA       NA       NA      NA
## 98              Dodge.Super+Turbo      NA       NA       NA       NA      NA
## 99            Ferrari.Super+Turbo      NA       NA       NA       NA      NA
## 100              Fiat.Super+Turbo      NA       NA       NA       NA      NA
## 101              Ford.Super+Turbo      NA       NA       NA       NA      NA
## 102           Genesis.Super+Turbo      NA       NA       NA       NA      NA
## 103               GMC.Super+Turbo      NA       NA       NA       NA      NA
## 104             Honda.Super+Turbo      NA       NA       NA       NA      NA
## 105           Hyundai.Super+Turbo      NA       NA       NA       NA      NA
## 106          Infiniti.Super+Turbo      NA       NA       NA       NA      NA
## 107            Jaguar.Super+Turbo      NA       NA       NA       NA      NA
## 108              Jeep.Super+Turbo      NA       NA       NA       NA      NA
## 109             Karma.Super+Turbo      NA       NA       NA       NA      NA
## 110               Kia.Super+Turbo      NA       NA       NA       NA      NA
## 111       Lamborghini.Super+Turbo      NA       NA       NA       NA      NA
## 112        Land Rover.Super+Turbo      NA       NA       NA       NA      NA
## 113             Lexus.Super+Turbo      NA       NA       NA       NA      NA
## 114           Lincoln.Super+Turbo      NA       NA       NA       NA      NA
## 115             Lotus.Super+Turbo      NA       NA       NA       NA      NA
## 116          Maserati.Super+Turbo      NA       NA       NA       NA      NA
## 117             Mazda.Super+Turbo      NA       NA       NA       NA      NA
## 118     Mercedes-Benz.Super+Turbo      NA       NA       NA       NA      NA
## 119              MINI.Super+Turbo      NA       NA       NA       NA      NA
## 120        Mitsubishi.Super+Turbo      NA       NA       NA       NA      NA
## 121            Nissan.Super+Turbo      NA       NA       NA       NA      NA
## 122           Porsche.Super+Turbo      NA       NA       NA       NA      NA
## 123               Ram.Super+Turbo      NA       NA       NA       NA      NA
## 124       Rolls-Royce.Super+Turbo      NA       NA       NA       NA      NA
## 125 Roush Performance.Super+Turbo      NA       NA       NA       NA      NA
## 126            Subaru.Super+Turbo      NA       NA       NA       NA      NA
## 127            Toyota.Super+Turbo      NA       NA       NA       NA      NA
## 128        Volkswagen.Super+Turbo      NA       NA       NA       NA      NA
## 129             Volvo.Super+Turbo 20.7216 23.05193 24.21620 24.74740 25.0491
## 130                   Acura.Turbo 21.0000 22.93200 23.00000 23.63830 24.0000
## 131              Alfa Romeo.Turbo 19.1410 22.36360 25.32660 26.24905 27.9406
## 132            Aston Martin.Turbo 17.0315 17.54953 19.06770 20.41378 20.4155
## 133                    Audi.Turbo 15.2575 21.56350 22.93240 25.99520 30.1793
## 134                 Bentley.Turbo 13.6613 14.66160 16.16640 18.79032 18.9656
## 135                     BMW.Turbo 15.0983 18.81510 22.69820 25.15485 29.6330
## 136                 Bugatti.Turbo 10.5921 10.59210 10.59210 10.59210 10.5921
## 137                   Buick.Turbo 22.0702 24.38220 25.91000 27.55740 30.7314
## 138                Cadillac.Turbo 17.0723 21.41760 23.19480 24.70460 27.1593
## 139               Chevrolet.Turbo 19.4160 22.79015 24.99250 26.96420 31.5018
## 140                Chrysler.Turbo      NA       NA       NA       NA      NA
## 141                   Dodge.Turbo      NA       NA       NA       NA      NA
## 142                 Ferrari.Turbo 16.3165 16.61150 16.81630 16.99400 18.1715
## 143                    Fiat.Turbo 24.9382 25.71632 27.63775 29.39140 29.6662
## 144                    Ford.Turbo 14.1252 19.01110 23.00000 24.22060 30.2418
## 145                 Genesis.Turbo 19.5855 19.99480 20.20260 21.67790 24.8653
## 146                     GMC.Turbo 20.0390 21.70925 23.39480 24.03375 27.4108
## 147                   Honda.Turbo 24.5785 29.17858 30.67410 33.12358 35.6082
## 148                 Hyundai.Turbo 21.9071 25.34128 28.29750 29.70075 36.0339
## 149                Infiniti.Turbo 21.0000 21.88575 22.01560 22.90050 26.1476
## 150                  Jaguar.Turbo 23.4266 23.95120 25.66260 26.67775 28.4037
## 151                    Jeep.Turbo 21.0000 22.02700 23.42915 24.93047 26.9515
## 152                   Karma.Turbo 25.8000 25.80000 25.80000 25.80000 25.8000
## 153                     Kia.Turbo 19.9185 21.25202 24.17085 27.90480 30.5243
## 154             Lamborghini.Turbo 13.9329 13.93290 13.93290 13.93290 13.9329
## 155              Land Rover.Turbo 19.2332 20.85280 22.25060 22.87910 24.3843
## 156                   Lexus.Turbo 21.1633 23.28730 24.29540 24.35575 24.6917
## 157                 Lincoln.Turbo 17.5802 19.52092 20.99500 23.03390 24.6965
## 158                   Lotus.Turbo      NA       NA       NA       NA      NA
## 159                Maserati.Turbo 15.3607 17.11340 17.75225 19.16457 19.3234
## 160                   Mazda.Turbo 22.6002 23.57078 24.14345 24.89813 26.4147
## 161           Mercedes-Benz.Turbo 13.8444 19.62680 21.71390 23.79833 28.4696
## 162                    MINI.Turbo 25.8460 27.37610 28.90900 29.79170 31.3799
## 163              Mitsubishi.Turbo 25.3444 25.83610 26.07675 26.36512 27.0000
## 164                  Nissan.Turbo 18.3595 20.92133 23.48315 26.04498 28.6068
## 165                 Porsche.Turbo 16.5714 19.93535 20.05000 20.98852 22.5030
## 166                     Ram.Turbo 24.2793 24.72130 25.16330 25.60530 26.0473
## 167             Rolls-Royce.Turbo 13.8915 13.89150 14.24320 14.32477 14.3910
## 168       Roush Performance.Turbo      NA       NA       NA       NA      NA
## 169                  Subaru.Turbo 18.6117 21.29745 22.93360 24.47780 26.9190
## 170                  Toyota.Turbo 26.4025 26.40250 26.40250 26.40250 26.4025
## 171              Volkswagen.Turbo 20.0891 23.31885 26.97935 28.77008 34.0525
## 172                   Volvo.Turbo 22.9931 24.44073 24.99780 26.53265 27.3890
##         mean          sd   n missing
## 1   24.51694  2.34138277  13       0
## 2        NaN          NA   0       0
## 3        NaN          NA   0       0
## 4   15.91530  0.00000000   2       0
## 5        NaN          NA   0       0
## 6        NaN          NA   0       0
## 7        NaN          NA   0       0
## 8   22.22906  1.90021386   5       0
## 9   19.52784  1.87177452   7       0
## 10  19.23372  4.33109818  54       0
## 11  22.87640  3.51186774   6       0
## 12  19.06879  2.24004011  20       0
## 13  13.36305  0.03090057   2       0
## 14       NaN          NA   0       0
## 15  24.84607  8.72058030  31       0
## 16  19.08147  1.30493980   6       0
## 17  17.79261  1.91708783  38       0
## 18  30.73820  8.79974404  27       0
## 19  32.82231 10.56418148  27       0
## 20  18.86610  3.63689783   4       0
## 21       NaN          NA   0       0
## 22  21.69147  2.96039754  23       0
## 23       NaN          NA   0       0
## 24  31.06313  9.37600397  25       0
## 25  13.60677  2.15613434   6       0
## 26       NaN          NA   0       0
## 27  25.03593  7.11932562  30       0
## 28  26.80413 12.30238326   3       0
## 29       NaN          NA   0       0
## 30       NaN          NA   0       0
## 31  29.21675  2.24225106  19       0
## 32  22.86774  1.18906788   5       0
## 33       NaN          NA   0       0
## 34  29.17217  5.86279711  12       0
## 35  25.17448  5.75324963  28       0
## 36       NaN          NA   0       0
## 37  19.50977  2.34190494  12       0
## 38       NaN          NA   0       0
## 39       NaN          NA   0       0
## 40  28.15261  3.13669865  17       0
## 41  30.13863 10.73806173  63       0
## 42  18.89640  0.36326879   4       0
## 43  28.74000  1.87962762   5       0
## 44       NaN          NA   0       0
## 45       NaN          NA   0       0
## 46       NaN          NA   0       0
## 47       NaN          NA   0       0
## 48       NaN          NA   0       0
## 49       NaN          NA   0       0
## 50       NaN          NA   0       0
## 51       NaN          NA   0       0
## 52       NaN          NA   0       0
## 53  15.64515  0.13187541   2       0
## 54       NaN          NA   0       0
## 55  15.55856  0.42955825   5       0
## 56       NaN          NA   0       0
## 57       NaN          NA   0       0
## 58  14.08530          NA   1       0
## 59       NaN          NA   0       0
## 60       NaN          NA   0       0
## 61       NaN          NA   0       0
## 62       NaN          NA   0       0
## 63       NaN          NA   0       0
## 64  20.43468  1.76954670  14       0
## 65  13.30610          NA   1       0
## 66       NaN          NA   0       0
## 67       NaN          NA   0       0
## 68       NaN          NA   0       0
## 69  17.76042  1.73604786  10       0
## 70       NaN          NA   0       0
## 71       NaN          NA   0       0
## 72  19.59370  0.48319687   4       0
## 73       NaN          NA   0       0
## 74       NaN          NA   0       0
## 75       NaN          NA   0       0
## 76       NaN          NA   0       0
## 77       NaN          NA   0       0
## 78       NaN          NA   0       0
## 79       NaN          NA   0       0
## 80       NaN          NA   0       0
## 81       NaN          NA   0       0
## 82  13.71920  1.27724813   4       0
## 83       NaN          NA   0       0
## 84       NaN          NA   0       0
## 85       NaN          NA   0       0
## 86       NaN          NA   0       0
## 87       NaN          NA   0       0
## 88       NaN          NA   0       0
## 89       NaN          NA   0       0
## 90       NaN          NA   0       0
## 91       NaN          NA   0       0
## 92       NaN          NA   0       0
## 93       NaN          NA   0       0
## 94       NaN          NA   0       0
## 95       NaN          NA   0       0
## 96       NaN          NA   0       0
## 97       NaN          NA   0       0
## 98       NaN          NA   0       0
## 99       NaN          NA   0       0
## 100      NaN          NA   0       0
## 101      NaN          NA   0       0
## 102      NaN          NA   0       0
## 103      NaN          NA   0       0
## 104      NaN          NA   0       0
## 105      NaN          NA   0       0
## 106      NaN          NA   0       0
## 107      NaN          NA   0       0
## 108      NaN          NA   0       0
## 109      NaN          NA   0       0
## 110      NaN          NA   0       0
## 111      NaN          NA   0       0
## 112      NaN          NA   0       0
## 113      NaN          NA   0       0
## 114      NaN          NA   0       0
## 115      NaN          NA   0       0
## 116      NaN          NA   0       0
## 117      NaN          NA   0       0
## 118      NaN          NA   0       0
## 119      NaN          NA   0       0
## 120      NaN          NA   0       0
## 121      NaN          NA   0       0
## 122      NaN          NA   0       0
## 123      NaN          NA   0       0
## 124      NaN          NA   0       0
## 125      NaN          NA   0       0
## 126      NaN          NA   0       0
## 127      NaN          NA   0       0
## 128      NaN          NA   0       0
## 129 23.63190  1.64896835   6       0
## 130 22.91406  1.15921552   5       0
## 131 24.23336  3.30109974   7       0
## 132 18.89560  1.77622654   4       0
## 133 23.07567  3.65911306  38       0
## 134 16.49755  2.23363280   8       0
## 135 22.32187  3.81437324 111       0
## 136 10.59210          NA   1       0
## 137 26.20500  2.49715944   9       0
## 138 23.27111  2.60564610  17       0
## 139 24.68084  3.26786135  23       0
## 140      NaN          NA   0       0
## 141      NaN          NA   0       0
## 142 16.98196  0.71110323   5       0
## 143 27.46997  2.36744632   4       0
## 144 22.26535  3.29026793  41       0
## 145 21.04587  1.81557752   9       0
## 146 23.23641  2.26714295  15       0
## 147 30.60935  3.22020578  18       0
## 148 27.85600  3.87219297  12       0
## 149 22.73545  1.62375092  10       0
## 150 25.67205  1.68470212  15       0
## 151 23.57945  1.90724436  12       0
## 152 25.80000          NA   1       0
## 153 24.60888  3.85878650  12       0
## 154 13.93290          NA   1       0
## 155 21.91872  1.66821111  13       0
## 156 23.63379  1.24282434   7       0
## 157 21.13294  2.22863488  20       0
## 158      NaN          NA   0       0
## 159 17.71110  1.53278968  10       0
## 160 24.32545  1.58449283   4       0
## 161 21.62214  3.40988896  84       0
## 162 28.61675  1.85263887  17       0
## 163 26.12448  0.68102802   4       0
## 164 23.48315  7.24593532   2       0
## 165 20.27934  1.32171653  40       0
## 166 25.16330  1.25016479   2       0
## 167 14.15570  0.22807865   8       0
## 168      NaN          NA   0       0
## 169 22.85926  2.86812653   7       0
## 170 26.40250          NA   1       0
## 171 26.72866  4.26221469  16       0
## 172 25.24031  1.61254344  10       0
```

```r
favstats(mpg~ car_make+fuelType1, data=cars2020)
```

```
##                      car_make.fuelType1     min       Q1   median       Q3
## 1                          Acura.Diesel      NA       NA       NA       NA
## 2                     Alfa Romeo.Diesel      NA       NA       NA       NA
## 3                   Aston Martin.Diesel      NA       NA       NA       NA
## 4                           Audi.Diesel      NA       NA       NA       NA
## 5                        Bentley.Diesel      NA       NA       NA       NA
## 6                            BMW.Diesel      NA       NA       NA       NA
## 7                        Bugatti.Diesel      NA       NA       NA       NA
## 8                          Buick.Diesel      NA       NA       NA       NA
## 9                       Cadillac.Diesel      NA       NA       NA       NA
## 10                     Chevrolet.Diesel 19.4160 22.31860 23.39480 25.13320
## 11                      Chrysler.Diesel      NA       NA       NA       NA
## 12                         Dodge.Diesel      NA       NA       NA       NA
## 13                       Ferrari.Diesel      NA       NA       NA       NA
## 14                          Fiat.Diesel      NA       NA       NA       NA
## 15                          Ford.Diesel 21.8271 23.20087 23.94095 24.22310
## 16                       Genesis.Diesel      NA       NA       NA       NA
## 17                           GMC.Diesel 22.3186 23.39480 23.82670 23.82670
## 18                         Honda.Diesel      NA       NA       NA       NA
## 19                       Hyundai.Diesel      NA       NA       NA       NA
## 20                      Infiniti.Diesel      NA       NA       NA       NA
## 21                        Jaguar.Diesel      NA       NA       NA       NA
## 22                          Jeep.Diesel 24.7211 24.72110 24.72110 24.72110
## 23                         Karma.Diesel      NA       NA       NA       NA
## 24                           Kia.Diesel      NA       NA       NA       NA
## 25                   Lamborghini.Diesel      NA       NA       NA       NA
## 26                    Land Rover.Diesel 22.6844 23.53435 24.38430 24.38430
## 27                         Lexus.Diesel      NA       NA       NA       NA
## 28                       Lincoln.Diesel      NA       NA       NA       NA
## 29                         Lotus.Diesel      NA       NA       NA       NA
## 30                      Maserati.Diesel      NA       NA       NA       NA
## 31                         Mazda.Diesel      NA       NA       NA       NA
## 32                 Mercedes-Benz.Diesel      NA       NA       NA       NA
## 33                          MINI.Diesel      NA       NA       NA       NA
## 34                    Mitsubishi.Diesel      NA       NA       NA       NA
## 35                        Nissan.Diesel      NA       NA       NA       NA
## 36                       Porsche.Diesel      NA       NA       NA       NA
## 37                           Ram.Diesel 24.2793 24.72130 25.16330 25.60530
## 38                   Rolls-Royce.Diesel      NA       NA       NA       NA
## 39             Roush Performance.Diesel      NA       NA       NA       NA
## 40                        Subaru.Diesel      NA       NA       NA       NA
## 41                        Toyota.Diesel      NA       NA       NA       NA
## 42                    Volkswagen.Diesel      NA       NA       NA       NA
## 43                         Volvo.Diesel      NA       NA       NA       NA
## 44              Acura.Midgrade Gasoline      NA       NA       NA       NA
## 45         Alfa Romeo.Midgrade Gasoline      NA       NA       NA       NA
## 46       Aston Martin.Midgrade Gasoline      NA       NA       NA       NA
## 47               Audi.Midgrade Gasoline      NA       NA       NA       NA
## 48            Bentley.Midgrade Gasoline      NA       NA       NA       NA
## 49                BMW.Midgrade Gasoline      NA       NA       NA       NA
## 50            Bugatti.Midgrade Gasoline      NA       NA       NA       NA
## 51              Buick.Midgrade Gasoline      NA       NA       NA       NA
## 52           Cadillac.Midgrade Gasoline      NA       NA       NA       NA
## 53          Chevrolet.Midgrade Gasoline      NA       NA       NA       NA
## 54           Chrysler.Midgrade Gasoline 19.0620 19.06200 19.06200 19.06200
## 55              Dodge.Midgrade Gasoline 16.6887 16.86540 17.99315 19.06200
## 56            Ferrari.Midgrade Gasoline      NA       NA       NA       NA
## 57               Fiat.Midgrade Gasoline      NA       NA       NA       NA
## 58               Ford.Midgrade Gasoline      NA       NA       NA       NA
## 59            Genesis.Midgrade Gasoline      NA       NA       NA       NA
## 60                GMC.Midgrade Gasoline      NA       NA       NA       NA
## 61              Honda.Midgrade Gasoline      NA       NA       NA       NA
## 62            Hyundai.Midgrade Gasoline      NA       NA       NA       NA
## 63           Infiniti.Midgrade Gasoline      NA       NA       NA       NA
## 64             Jaguar.Midgrade Gasoline      NA       NA       NA       NA
## 65               Jeep.Midgrade Gasoline 16.6887 16.68870 16.68870 16.68870
## 66              Karma.Midgrade Gasoline      NA       NA       NA       NA
## 67                Kia.Midgrade Gasoline      NA       NA       NA       NA
## 68        Lamborghini.Midgrade Gasoline      NA       NA       NA       NA
## 69         Land Rover.Midgrade Gasoline      NA       NA       NA       NA
## 70              Lexus.Midgrade Gasoline      NA       NA       NA       NA
## 71            Lincoln.Midgrade Gasoline      NA       NA       NA       NA
## 72              Lotus.Midgrade Gasoline      NA       NA       NA       NA
## 73           Maserati.Midgrade Gasoline      NA       NA       NA       NA
## 74              Mazda.Midgrade Gasoline      NA       NA       NA       NA
## 75      Mercedes-Benz.Midgrade Gasoline      NA       NA       NA       NA
## 76               MINI.Midgrade Gasoline      NA       NA       NA       NA
## 77         Mitsubishi.Midgrade Gasoline      NA       NA       NA       NA
## 78             Nissan.Midgrade Gasoline      NA       NA       NA       NA
## 79            Porsche.Midgrade Gasoline      NA       NA       NA       NA
## 80                Ram.Midgrade Gasoline 16.8732 17.08340 17.31010 18.38775
## 81        Rolls-Royce.Midgrade Gasoline      NA       NA       NA       NA
## 82  Roush Performance.Midgrade Gasoline      NA       NA       NA       NA
## 83             Subaru.Midgrade Gasoline      NA       NA       NA       NA
## 84             Toyota.Midgrade Gasoline      NA       NA       NA       NA
## 85         Volkswagen.Midgrade Gasoline      NA       NA       NA       NA
## 86              Volvo.Midgrade Gasoline      NA       NA       NA       NA
## 87               Acura.Premium Gasoline 21.0000 22.94900 23.25380 25.75972
## 88          Alfa Romeo.Premium Gasoline 19.1410 22.36360 25.32660 26.24905
## 89        Aston Martin.Premium Gasoline 17.0315 17.54953 19.06770 20.41378
## 90                Audi.Premium Gasoline 15.2575 18.83697 22.50730 24.95518
## 91             Bentley.Premium Gasoline 13.6613 14.66160 16.16640 18.79032
## 92                 BMW.Premium Gasoline 15.0983 18.81510 22.69820 25.15485
## 93             Bugatti.Premium Gasoline 10.5921 10.59210 10.59210 10.59210
## 94               Buick.Premium Gasoline 22.0702 23.80420 24.38220 24.76415
## 95            Cadillac.Premium Gasoline 16.5637 20.96725 23.19480 24.48955
## 96           Chevrolet.Premium Gasoline 15.4336 16.86560 17.25350 23.35810
## 97            Chrysler.Premium Gasoline      NA       NA       NA       NA
## 98               Dodge.Premium Gasoline 15.0483 15.57750 16.91490 17.76080
## 99             Ferrari.Premium Gasoline 13.3412 14.85070 16.61150 16.90515
## 100               Fiat.Premium Gasoline 24.9382 27.11900 29.29980 29.48300
## 101               Ford.Premium Gasoline 14.0853 14.10525 14.12520 15.16140
## 102            Genesis.Premium Gasoline 17.7991 18.81200 19.99480 20.24020
## 103                GMC.Premium Gasoline 15.9496 16.91412 17.17410 18.68960
## 104              Honda.Premium Gasoline 24.5785 30.00000 30.00000 31.98370
## 105            Hyundai.Premium Gasoline      NA       NA       NA       NA
## 106           Infiniti.Premium Gasoline 15.3391 21.70990 21.97385 22.41270
## 107             Jaguar.Premium Gasoline 18.1891 20.86350 23.42660 25.66260
## 108               Jeep.Premium Gasoline 13.3061 13.71623 14.12635 14.53647
## 109              Karma.Premium Gasoline 25.8000 25.80000 25.80000 25.80000
## 110                Kia.Premium Gasoline 19.9185 20.10770 20.86390 24.14150
## 111        Lamborghini.Premium Gasoline 10.7137 12.43430 14.98990 14.99780
## 112         Land Rover.Premium Gasoline 15.1147 17.99805 19.73945 21.17800
## 113              Lexus.Premium Gasoline 13.8684 21.00000 22.82060 24.38593
## 114            Lincoln.Premium Gasoline      NA       NA       NA       NA
## 115              Lotus.Premium Gasoline 18.9178 19.42592 19.71800 19.88577
## 116           Maserati.Premium Gasoline 15.3607 17.11340 17.75225 19.16457
## 117              Mazda.Premium Gasoline 29.0373 29.20895 29.38060 29.55225
## 118      Mercedes-Benz.Premium Gasoline 13.8444 19.69810 21.72890 23.77550
## 119               MINI.Premium Gasoline 25.8460 27.37610 28.90900 29.79170
## 120         Mitsubishi.Premium Gasoline 22.0000 22.00000 22.00000 22.00000
## 121             Nissan.Premium Gasoline 17.0000 18.07625 19.15845 21.05135
## 122            Porsche.Premium Gasoline 16.5714 19.93535 20.05000 20.98852
## 123                Ram.Premium Gasoline      NA       NA       NA       NA
## 124        Rolls-Royce.Premium Gasoline 13.8915 13.89150 14.24320 14.32477
## 125  Roush Performance.Premium Gasoline 12.6756 12.67560 13.45995 14.50355
## 126             Subaru.Premium Gasoline 18.6117 21.14982 23.05835 23.61950
## 127             Toyota.Premium Gasoline 23.7198 25.06115 26.40250 26.70090
## 128         Volkswagen.Premium Gasoline 23.4414 23.83785 24.23430 24.63075
## 129              Volvo.Premium Gasoline 20.7216 24.23242 24.98645 26.94725
## 130              Acura.Regular Gasoline      NA       NA       NA       NA
## 131         Alfa Romeo.Regular Gasoline      NA       NA       NA       NA
## 132       Aston Martin.Regular Gasoline      NA       NA       NA       NA
## 133               Audi.Regular Gasoline 22.1236 24.77180 25.38350 25.99520
## 134            Bentley.Regular Gasoline      NA       NA       NA       NA
## 135                BMW.Regular Gasoline      NA       NA       NA       NA
## 136            Bugatti.Regular Gasoline      NA       NA       NA       NA
## 137              Buick.Regular Gasoline 20.0162 22.13430 25.29310 27.41352
## 138           Cadillac.Regular Gasoline 20.0050 20.34840 20.35080 21.07330
## 139          Chevrolet.Regular Gasoline 14.9623 17.00000 19.83080 22.45370
## 140           Chrysler.Regular Gasoline 21.4055 22.24520 22.24520 22.77980
## 141              Dodge.Regular Gasoline 19.9421 21.05118 21.40550 21.78523
## 142            Ferrari.Regular Gasoline      NA       NA       NA       NA
## 143               Fiat.Regular Gasoline 25.9757 25.97570 25.97570 25.97570
## 144               Ford.Regular Gasoline 15.8294 19.00000 22.92590 25.05903
## 145            Genesis.Regular Gasoline 20.1170 20.37830 20.63960 20.90090
## 146                GMC.Regular Gasoline 14.9623 16.90445 18.07605 20.26892
## 147              Honda.Regular Gasoline 20.5738 25.88335 30.25000 33.31468
## 148            Hyundai.Regular Gasoline 21.1618 24.89410 29.50960 32.19735
## 149           Infiniti.Regular Gasoline      NA       NA       NA       NA
## 150             Jaguar.Regular Gasoline      NA       NA       NA       NA
## 151               Jeep.Regular Gasoline 18.7070 20.75360 22.51255 24.87260
## 152              Karma.Regular Gasoline      NA       NA       NA       NA
## 153                Kia.Regular Gasoline 20.4440 23.25698 28.19020 33.68158
## 154        Lamborghini.Regular Gasoline      NA       NA       NA       NA
## 155         Land Rover.Regular Gasoline      NA       NA       NA       NA
## 156              Lexus.Regular Gasoline 21.2370 22.30735 25.51620 35.76130
## 157            Lincoln.Regular Gasoline 17.5802 19.45870 20.98220 23.06780
## 158              Lotus.Regular Gasoline      NA       NA       NA       NA
## 159           Maserati.Regular Gasoline      NA       NA       NA       NA
## 160              Mazda.Regular Gasoline 22.6002 26.49240 28.32850 29.61800
## 161      Mercedes-Benz.Regular Gasoline      NA       NA       NA       NA
## 162               MINI.Regular Gasoline      NA       NA       NA       NA
## 163         Mitsubishi.Regular Gasoline 25.0000 25.47005 26.15350 31.17510
## 164             Nissan.Regular Gasoline 15.0000 22.99288 28.28915 30.75238
## 165            Porsche.Regular Gasoline      NA       NA       NA       NA
## 166                Ram.Regular Gasoline 18.7200 20.09915 21.52220 22.39708
## 167        Rolls-Royce.Regular Gasoline      NA       NA       NA       NA
## 168  Roush Performance.Regular Gasoline      NA       NA       NA       NA
## 169             Subaru.Regular Gasoline 22.0533 25.91593 27.80425 30.20012
## 170             Toyota.Regular Gasoline 14.2639 21.34200 29.82150 34.98240
## 171         Volkswagen.Regular Gasoline 18.5818 20.48922 25.92870 27.97373
## 172              Volvo.Regular Gasoline 26.7102 26.71020 26.71020 26.71020
##         max     mean         sd   n missing
## 1        NA      NaN         NA   0       0
## 2        NA      NaN         NA   0       0
## 3        NA      NaN         NA   0       0
## 4        NA      NaN         NA   0       0
## 5        NA      NaN         NA   0       0
## 6        NA      NaN         NA   0       0
## 7        NA      NaN         NA   0       0
## 8        NA      NaN         NA   0       0
## 9        NA      NaN         NA   0       0
## 10  26.5885 23.37022  2.7480807   5       0
## 11       NA      NaN         NA   0       0
## 12       NA      NaN         NA   0       0
## 13       NA      NaN         NA   0       0
## 14       NA      NaN         NA   0       0
## 15  24.2231 23.48302  1.1355478   4       0
## 16       NA      NaN         NA   0       0
## 17  25.9462 23.86260  1.3178334   5       0
## 18       NA      NaN         NA   0       0
## 19       NA      NaN         NA   0       0
## 20       NA      NaN         NA   0       0
## 21       NA      NaN         NA   0       0
## 22  24.7211 24.72110         NA   1       0
## 23       NA      NaN         NA   0       0
## 24       NA      NaN         NA   0       0
## 25       NA      NaN         NA   0       0
## 26  24.3843 23.81767  0.9814377   3       0
## 27       NA      NaN         NA   0       0
## 28       NA      NaN         NA   0       0
## 29       NA      NaN         NA   0       0
## 30       NA      NaN         NA   0       0
## 31       NA      NaN         NA   0       0
## 32       NA      NaN         NA   0       0
## 33       NA      NaN         NA   0       0
## 34       NA      NaN         NA   0       0
## 35       NA      NaN         NA   0       0
## 36       NA      NaN         NA   0       0
## 37  26.0473 25.16330  1.2501648   2       0
## 38       NA      NaN         NA   0       0
## 39       NA      NaN         NA   0       0
## 40       NA      NaN         NA   0       0
## 41       NA      NaN         NA   0       0
## 42       NA      NaN         NA   0       0
## 43       NA      NaN         NA   0       0
## 44       NA      NaN         NA   0       0
## 45       NA      NaN         NA   0       0
## 46       NA      NaN         NA   0       0
## 47       NA      NaN         NA   0       0
## 48       NA      NaN         NA   0       0
## 49       NA      NaN         NA   0       0
## 50       NA      NaN         NA   0       0
## 51       NA      NaN         NA   0       0
## 52       NA      NaN         NA   0       0
## 53       NA      NaN         NA   0       0
## 54  19.0620 19.06200         NA   1       0
## 55  19.0620 17.93425  1.3057608   4       0
## 56       NA      NaN         NA   0       0
## 57       NA      NaN         NA   0       0
## 58       NA      NaN         NA   0       0
## 59       NA      NaN         NA   0       0
## 60       NA      NaN         NA   0       0
## 61       NA      NaN         NA   0       0
## 62       NA      NaN         NA   0       0
## 63       NA      NaN         NA   0       0
## 64       NA      NaN         NA   0       0
## 65  16.6887 16.68870         NA   1       0
## 66       NA      NaN         NA   0       0
## 67       NA      NaN         NA   0       0
## 68       NA      NaN         NA   0       0
## 69       NA      NaN         NA   0       0
## 70       NA      NaN         NA   0       0
## 71       NA      NaN         NA   0       0
## 72       NA      NaN         NA   0       0
## 73       NA      NaN         NA   0       0
## 74       NA      NaN         NA   0       0
## 75       NA      NaN         NA   0       0
## 76       NA      NaN         NA   0       0
## 77       NA      NaN         NA   0       0
## 78       NA      NaN         NA   0       0
## 79       NA      NaN         NA   0       0
## 80  19.0135 17.71028  0.9210832   6       0
## 81       NA      NaN         NA   0       0
## 82       NA      NaN         NA   0       0
## 83       NA      NaN         NA   0       0
## 84       NA      NaN         NA   0       0
## 85       NA      NaN         NA   0       0
## 86       NA      NaN         NA   0       0
## 87  28.0000 24.07169  2.1752314  18       0
## 88  27.9406 24.23336  3.3010997   7       0
## 89  20.4155 18.89560  1.7762265   4       0
## 90  29.9608 22.20203  3.8839980  34       0
## 91  18.9656 16.49755  2.2336328   8       0
## 92  29.6330 22.32187  3.8143732 111       0
## 93  10.5921 10.59210         NA   1       0
## 94  25.9100 24.18615  1.5838534   4       0
## 95  27.1593 22.59719  3.1812291  19       0
## 96  25.7929 19.36634  3.7807240  17       0
## 97       NA      NaN         NA   0       0
## 98  17.9612 16.59589  1.1472230  13       0
## 99  18.1715 15.94799  1.8588942   7       0
## 100 29.6662 27.96807  2.6303291   3       0
## 101 16.1976 14.80270  1.2081836   3       0
## 102 24.8653 20.20172  1.9978795  13       0
## 103 24.0000 18.54331  3.0983010   8       0
## 104 32.0168 29.71580  3.0410262   5       0
## 105      NA      NaN         NA   0       0
## 106 26.1476 21.62992  2.8580110  14       0
## 107 28.4037 23.14366  3.1570395  29       0
## 108 14.9466 14.12635  1.1600087   2       0
## 109 25.8000 25.80000         NA   1       0
## 110 24.5081 21.90794  2.2382123   5       0
## 111 15.0057 13.65336  1.9721285   7       0
## 112 23.1444 19.55473  2.3943033  20       0
## 113 29.7574 22.67582  3.8383679  26       0
## 114      NA      NaN         NA   0       0
## 115 20.0210 19.59370  0.4831969   4       0
## 116 19.3234 17.71110  1.5327897  10       0
## 117 29.7239 29.38060  0.4854995   2       0
## 118 28.4696 21.69211  3.3337919  89       0
## 119 31.3799 28.61675  1.8526389  17       0
## 120 22.0000 22.00000         NA   1       0
## 121 24.2133 19.77475  2.4380597   8       0
## 122 22.5030 20.27934  1.3217165  40       0
## 123      NA      NaN         NA   0       0
## 124 14.3910 14.15570  0.2280787   8       0
## 125 15.2813 13.71920  1.2772481   4       0
## 126 27.4825 22.75518  3.0219593   6       0
## 127 26.9993 25.70720  1.7468147   3       0
## 128 25.0272 24.23430  1.1213299   2       0
## 129 30.3000 25.55921  2.5515384  20       0
## 130      NA      NaN         NA   0       0
## 131      NA      NaN         NA   0       0
## 132      NA      NaN         NA   0       0
## 133 30.1793 25.63948  2.6355337   6       0
## 134      NA      NaN         NA   0       0
## 135      NA      NaN         NA   0       0
## 136      NA      NaN         NA   0       0
## 137 30.7314 25.02457  3.4255128  10       0
## 138 21.1796 20.59142  0.5096590   5       0
## 139 32.9630 20.90336  5.0597854  57       0
## 140 29.5207 23.63928  3.3243984   5       0
## 141 22.7798 21.46061  0.9534033   8       0
## 142      NA      NaN         NA   0       0
## 143 25.9757 25.97570         NA   1       0
## 144 42.0000 23.61898  6.4296693  66       0
## 145 21.1622 20.63960  0.7390680   2       0
## 146 27.4108 18.92514  2.9451645  40       0
## 147 51.9827 30.80802  7.4205090  40       0
## 148 57.7824 31.29421  9.2785626  39       0
## 149      NA      NaN         NA   0       0
## 150      NA      NaN         NA   0       0
## 151 26.9515 22.67190  2.3080139  32       0
## 152      NA      NaN         NA   0       0
## 153 50.4101 30.07328  8.6192352  32       0
## 154      NA      NaN         NA   0       0
## 155      NA      NaN         NA   0       0
## 156 44.0943 29.72209  8.5917302  11       0
## 157 41.0000 21.87267  4.6757040  23       0
## 158      NA      NaN         NA   0       0
## 159      NA      NaN         NA   0       0
## 160 34.9351 28.26947  2.9546240  21       0
## 161      NA      NaN         NA   0       0
## 162      NA      NaN         NA   0       0
## 163 38.6074 28.83759  5.0956727  15       0
## 164 35.0880 26.98425  5.3674491  22       0
## 165      NA      NaN         NA   0       0
## 166 23.7776 21.30925  1.8563433   6       0
## 167      NA      NaN         NA   0       0
## 168      NA      NaN         NA   0       0
## 169 35.1000 27.89322  3.2652402  18       0
## 170 55.7000 30.29533 10.8753663  61       0
## 171 34.0525 25.26531  5.2357601  18       0
## 172 26.7102 26.71020         NA   1       0
```


