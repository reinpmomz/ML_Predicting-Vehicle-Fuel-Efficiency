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
cars2020 #The input data consists of 1,164 observations of 14 variables
```

```
##                ï..make                               model     mpg transmission
## 1               Toyota                             Corolla 34.2793          CVT
## 2               Toyota                      Corolla Hybrid 52.0000          CVT
## 3               Toyota                             Corolla 31.8162       Manual
## 4               Toyota                         Corolla XSE 33.6766          CVT
## 5               Toyota                             Corolla 33.0496          CVT
## 6               Toyota                             Corolla 33.1228       Manual
## 7               Toyota                         Corolla XLE 32.2043          CVT
## 8                  Kia                                Soul 29.6510          CVT
## 9                  Kia                   Soul Eco dynamics 31.3493          CVT
## 10                 Kia                                Soul 27.0504       Manual
## 11                 Kia                                Soul 28.8021       Manual
## 12                 Kia                        Sportage FWD 25.6059    Automatic
## 13                 Kia                        Sportage FWD 22.9526    Automatic
## 14                 Kia                       Telluride FWD 22.5318    Automatic
## 15                 Kia                        Sportage AWD 23.3388    Automatic
## 16                 Kia                        Sportage AWD 21.3814    Automatic
## 17                 Kia                       Telluride AWD 20.8926    Automatic
## 18       Mercedes-Benz                       GLE350 4matic 22.0525    Automatic
## 19       Mercedes-Benz                       GLE450 4matic 21.1292    Automatic
## 20              Jaguar                        F-Type Coupe 25.6626    Automatic
## 21              Jaguar                  F-Type Convertible 25.6626    Automatic
## 22              Jaguar                                  XE 28.4037    Automatic
## 23              Jaguar                              XE 30t 26.6827    Automatic
## 24              Jaguar                              XE AWD 27.6136    Automatic
## 25              Jaguar                          XE AWD 30t 25.0846    Automatic
## 26              Jaguar                                  XF 28.4037    Automatic
## 27              Jaguar                              XF 30t 26.4949    Automatic
## 28              Jaguar                              XF AWD 26.6728    Automatic
## 29              Jaguar                          XF AWD 30t 25.4630    Automatic
## 30              Jaguar                   XF Sportbrake AWD 24.0099    Automatic
## 31              Jaguar                              E-Pace 23.8925    Automatic
## 32              Jaguar                         E-Pace P300 23.4266    Automatic
## 33              Jaguar                              F-Pace 23.8056    Automatic
## 34              Jaguar                          F-Pace 30t 23.8019    Automatic
## 35          Land Rover                   Range Rover Velar 23.1444    Automatic
## 36          Land Rover              Range Rover Velar P300 22.2506    Automatic
## 37              Jaguar                        F-Type Coupe 22.6919    Automatic
## 38              Jaguar                  F-Type Convertible 22.6919    Automatic
## 39              Jaguar                      F-Type S Coupe 21.8371    Automatic
## 40              Jaguar                F-Type S Convertible 21.8371    Automatic
## 41              Jaguar                  F-Type S AWD Coupe 20.8635    Automatic
## 42              Jaguar            F-Type S AWD Convertible 20.8635    Automatic
## 43              Jaguar                              XF AWD 22.6633    Automatic
## 44              Jaguar                   XF Sportbrake AWD 20.7786    Automatic
## 45              Jaguar                              F-Pace 19.8739    Automatic
## 46              Jaguar                              F-Pace 18.1891    Automatic
## 47          Land Rover                              Evoque 22.5652    Automatic
## 48             Hyundai                        Palisade AWD 21.1618    Automatic
## 49                 BMW                          230i Coupe 27.2748    Automatic
## 50             Hyundai                             Elantra 34.0000          CVT
## 51             Hyundai                          Elantra SE 35.2379          CVT
## 52              Toyota                            GR Supra 26.4025    Automatic
## 53                 BMW                          230i Coupe 24.9272       Manual
## 54                 BMW                   230i xDrive Coupe 24.3879    Automatic
## 55                 BMW                    230i Convertible 27.0724    Automatic
## 56                 BMW             230i xDrive Convertible 24.3879    Automatic
## 57                 BMW           M240i Coupe M Performance 24.9871    Automatic
## 58                 BMW           M240i Coupe M Performance 21.6548       Manual
## 59                 BMW    M240i xDrive Coupe M Performance 24.8442    Automatic
## 60                 BMW                         M240i Coupe 24.9871    Automatic
## 61                 BMW                         M240i Coupe 21.6548       Manual
## 62                 BMW                  M240i xDrive Coupe 24.8442    Automatic
## 63                 BMW                   M240i Convertible 24.9871    Automatic
## 64                 BMW                   M240i Convertible 21.6548       Manual
## 65                 BMW            M240i xDrive Convertible 24.8442    Automatic
## 66                 BMW                M2 Competition Coupe 20.1642       Manual
## 67                 BMW                M2 Competition Coupe 18.8151       Manual
## 68                 BMW                    430i Convertible 27.0724    Automatic
## 69                 BMW             430i xDrive Convertible 25.7088    Automatic
## 70                 BMW                    440i Convertible 24.4977    Automatic
## 71                 BMW             440i xDrive Convertible 22.5081    Automatic
## 72                 BMW                      M4 Convertible 19.8307       Manual
## 73                 BMW                      M4 Convertible 18.5025       Manual
## 74                 BMW          M4 Convertible Competition 19.8307       Manual
## 75                 BMW          M4 Convertible Competition 18.5025       Manual
## 76               Acura                             TLX FWD 26.9868       Manual
## 77               Acura                      TLX FWD A-SPEC 26.3463       Manual
## 78               Acura                             TLX FWD 23.7694    Automatic
## 79               Acura                      TLX FWD A-SPEC 23.3109    Automatic
## 80               Acura                             TLX AWD 23.0510    Automatic
## 81               Acura                      TLX AWD A-SPEC 22.8512    Automatic
## 82                 BMW                                330i 29.6330    Automatic
## 83                 BMW                        M340i xDrive 24.8442    Automatic
## 84                 BMW                               M340i 24.9871    Automatic
## 85                 BMW                          430i Coupe 27.0724    Automatic
## 86                 BMW                          430i Coupe 25.3226       Manual
## 87                 BMW                   430i xDrive Coupe 24.3879    Automatic
## 88                 BMW                     430i Gran Coupe 27.0724    Automatic
## 89                 BMW              430i xDrive Gran Coupe 24.3879    Automatic
## 90                 BMW                          440i Coupe 24.9871    Automatic
## 91                 BMW                          440i Coupe 21.6548       Manual
## 92                 BMW                   440i xDrive Coupe 24.8442    Automatic
## 93                 BMW                   440i xDrive Coupe 21.4433       Manual
## 94                 BMW                     440i Gran Coupe 24.9871    Automatic
## 95                 BMW              440i xDrive Gran Coupe 24.8442    Automatic
## 96                 BMW                            M4 Coupe 20.1642       Manual
## 97                 BMW                            M4 Coupe 18.8151       Manual
## 98                 BMW                               M4 CS 18.8151       Manual
## 99                 BMW                M4 Coupe Competition 20.1642       Manual
## 100                BMW                M4 Coupe Competition 18.8151       Manual
## 101                BMW                                740i 24.4977    Automatic
## 102                BMW                         740i xDrive 22.5081    Automatic
## 103                BMW                         750i xDrive 19.6254    Automatic
## 104                BMW                        M760i xDrive 15.5255    Automatic
## 105               Jeep                Gladiator Pickup 4WD 19.0841    Automatic
## 106               Jeep                Gladiator Pickup 4WD 18.7070       Manual
## 107         Land Rover                   Range Rover Velar 17.3748    Automatic
## 108                BMW                         745e xDrive 21.8005    Automatic
## 109                BMW                             Z4 M40i 26.4025    Automatic
## 110             Nissan                                370Z 21.9494    Automatic
## 111             Nissan                                370Z 19.9574       Manual
## 112             Nissan                       370Z Roadster 20.7520    Automatic
## 113            Genesis                             G70 AWD 23.0588    Automatic
## 114            Genesis                             G70 RWD 24.8653    Automatic
## 115            Genesis                             G70 RWD 21.6779       Manual
## 116            Hyundai                            Veloster 29.7490    Automatic
## 117            Hyundai                            Veloster 28.2369       Manual
## 118            Hyundai                            Veloster 30.2742       Manual
## 119            Hyundai                            Veloster 28.9551       Manual
## 120            Hyundai                          Veloster N 24.7112       Manual
## 121             Toyota                          Sienna 2WD 21.3420    Automatic
## 122             Toyota                          Sienna AWD 20.1322    Automatic
## 123            Hyundai                        Palisade FWD 21.9908    Automatic
## 124       Aston Martin                          Vantage V8 20.4155    Automatic
## 125               Audi                              R8 AWD 15.9153       Manual
## 126        Lamborghini                             Huracan 14.9899       Manual
## 127        Lamborghini                      Huracan Spyder 14.9899       Manual
## 128       Aston Martin                            DB11 V12 17.7222    Automatic
## 129       Aston Martin                             DB11 V8 20.4132    Automatic
## 130            Genesis                             G70 AWD 19.5855    Automatic
## 131            Genesis                             G70 RWD 20.0929    Automatic
## 132            Hyundai                              Accent 36.2273          CVT
## 133            Hyundai                              Accent 32.6036       Manual
## 134            Hyundai                             Elantra 36.0339       Manual
## 135                Kia                              Sedona 20.5357    Automatic
## 136              Acura                             RDX FWD 24.0000    Automatic
## 137              Acura                      RDX FWD A-SPEC 23.6383    Automatic
## 138              Acura                             RDX AWD 23.0000    Automatic
## 139              Acura                      RDX AWD A-SPEC 22.9320    Automatic
## 140            Hyundai                            Kona AWD 27.4730       Manual
## 141            Hyundai                            Kona FWD 29.5096       Manual
## 142            Hyundai                            Kona FWD 29.7575    Automatic
## 143            Hyundai                            Kona AWD 27.5263    Automatic
## 144          Chevrolet                              Malibu 25.7929    Automatic
## 145            Genesis                             G80 AWD 20.1170    Automatic
## 146            Genesis                             G80 RWD 21.1622    Automatic
## 147           Cadillac                             XT6 FWD 20.3508    Automatic
## 148           Cadillac                             XT6 AWD 20.0050    Automatic
## 149             Subaru                              Ascent 22.9336          CVT
## 150             Subaru              Ascent Limited/Touring 22.0533          CVT
## 151            Hyundai                             Elantra 29.9053       Manual
## 152                Kia                       Optima Hybrid 41.9107       Manual
## 153              Buick                        Envision FWD 24.7031    Automatic
## 154            Hyundai                          Tucson FWD 25.0770    Automatic
## 155            Hyundai                          Tucson FWD 24.6367    Automatic
## 156              Buick                        Envision AWD 23.6232    Automatic
## 157              Buick                        Envision AWD 22.0702    Automatic
## 158            Hyundai                          Tucson AWD 23.3296    Automatic
## 159            Hyundai                          Tucson AWD 23.2574    Automatic
## 160         Land Rover              Range Rover Velar P380 20.2457    Automatic
## 161         Land Rover                   Range Rover Velar 20.2457    Automatic
## 162             Jaguar                    F-Type AWD Coupe 18.4489    Automatic
## 163             Jaguar              F-Type AWD Convertible 18.4489    Automatic
## 164          Chevrolet                               Spark 32.9630          CVT
## 165          Chevrolet                               Spark 32.5457       Manual
## 166          Chevrolet                         Spark ACTIV 32.0000       Manual
## 167          Chevrolet                         Spark ACTIV 32.9630          CVT
## 168              Lexus                                RC F 19.2380    Automatic
## 169            Hyundai                             Elantra 29.1092       Manual
## 170                Kia                               Forte 30.3006       Manual
## 171                Kia                               Forte 27.6057       Manual
## 172                Kia                            Forte FE 34.7078          CVT
## 173                Kia                               Forte 33.3395          CVT
## 174                Kia                               Forte 31.0015       Manual
## 175            Genesis                             G80 AWD 19.6948    Automatic
## 176            Genesis                             G80 RWD 20.2026    Automatic
## 177            Genesis                             G80 AWD 17.7991    Automatic
## 178            Genesis                             G80 RWD 18.8120    Automatic
## 179                Kia                              Optima 30.5243       Manual
## 180                Kia                              Optima 24.2002    Automatic
## 181                Kia                              Optima 27.2397    Automatic
## 182                Kia                           Optima FE 28.7747    Automatic
## 183               Ford             Transit Connect Van 2WD 25.1953    Automatic
## 184               Ford             Transit Connect Van FFV 25.1953    Automatic
## 185               Ford       Transit Connect Wagon LWB FWD 25.6715    Automatic
## 186               Ford       Transit Connect Wagon LWB FFV 25.6715    Automatic
## 187               Ford             Transit Connect Van 2WD 22.2684    Automatic
## 188               Ford       Transit Connect Wagon LWB FWD 22.2055    Automatic
## 189               Ford                Transit Connect USPS 22.2684    Automatic
## 190            Hyundai                        Santa Fe FWD 22.5215    Automatic
## 191            Hyundai                        Santa Fe FWD 24.5725    Automatic
## 192              Lexus                              NX 300 24.6917    Automatic
## 193         Mitsubishi                       Outlander 2WD 27.3502          CVT
## 194            Hyundai                        Santa Fe AWD 21.9071    Automatic
## 195            Hyundai                        Santa Fe AWD 23.5361    Automatic
## 196              Lexus                         NX 300h AWD 31.0000          CVT
## 197              Lexus                          NX 300 AWD 24.4161    Automatic
## 198              Lexus                  NX 300 AWD F Sport 23.6858    Automatic
## 199         Mitsubishi                       Outlander 4WD 25.9027          CVT
## 200               Ford                        Explorer RWD 24.2206    Automatic
## 201            Lincoln                         Aviator RWD 21.0078    Automatic
## 202               Ford                        Explorer AWD 23.2015    Automatic
## 203            Lincoln                         Aviator AWD 19.7885    Automatic
## 204             Toyota                         Prius Prime 54.4329          CVT
## 205              Volvo                             S60 FWD 27.3890    Automatic
## 206              Volvo                             S60 AWD 25.0491    Automatic
## 207          Chevrolet                              Malibu 31.5018          CVT
## 208              Volvo                             S90 AWD 24.7474    Automatic
## 209              Volvo                             V60 FWD 27.3890    Automatic
## 210              Volvo                          V60 CC AWD 24.9238    Automatic
## 211              Volvo                             V90 FWD 26.0000    Automatic
## 212              Volvo                             V90 AWD 24.7474    Automatic
## 213              Volvo                          V90 CC AWD 23.6850    Automatic
## 214          Chevrolet                         Equinox FWD 28.1539    Automatic
## 215          Chevrolet                         Equinox FWD 25.3669    Automatic
## 216                GMC                         Terrain FWD 27.4108    Automatic
## 217              Volvo                            XC60 FWD 24.4149    Automatic
## 218              Volvo                            XC40 FWD 26.7102    Automatic
## 219          Chevrolet                         Equinox AWD 27.3399    Automatic
## 220          Chevrolet                         Equinox AWD 24.1864    Automatic
## 221                GMC                         Terrain AWD 26.2618    Automatic
## 222                GMC                         Terrain AWD 23.0000    Automatic
## 223              Volvo                            XC60 AWD 22.8409    Automatic
## 224              Volvo                            XC40 AWD 25.0718    Automatic
## 225              Volvo                            XC90 FWD 24.5182    Automatic
## 226               Ford                        Explorer AWD 20.4611    Automatic
## 227               Ford                    Explorer FFV AWD 18.7074    Automatic
## 228      Mercedes-Benz                        AMG GT Coupe 18.1314       Manual
## 229      Mercedes-Benz                     AMG GT Roadster 18.1314       Manual
## 230              Buick                          Encore FWD 26.9819    Automatic
## 231          Chevrolet                            Trax FWD 28.0161    Automatic
## 232              Buick                          Encore AWD 25.8831    Automatic
## 233          Chevrolet                            Trax AWD 25.8831    Automatic
## 234              Volvo                            XC60 AWD 22.9931    Automatic
## 235              Buick                         Enclave FWD 21.1648    Automatic
## 236          Chevrolet                        Traverse FWD 21.1648    Automatic
## 237              Buick                         Enclave AWD 20.0162    Automatic
## 238          Chevrolet                        Traverse AWD 20.0908    Automatic
## 239              Volvo                            XC90 AWD 22.9931    Automatic
## 240                Kia               Optima Plug-in Hybrid 41.1024       Manual
## 241         Alfa Romeo                                  4C 27.9406       Manual
## 242          Chevrolet                               Sonic 28.8278    Automatic
## 243             Nissan                              Altima 32.0000          CVT
## 244             Nissan                  Altima SR/Platinum 30.9338          CVT
## 245             Nissan                          Altima AWD 30.2081          CVT
## 246             Nissan              Altima AWD SR/Platinum 29.0000          CVT
## 247             Nissan                  Altima SR/Platinum 28.6068          CVT
## 248                Kia                                K900 20.8639    Automatic
## 249          Chevrolet                             Sonic 5 28.8278    Automatic
## 250           Cadillac                             XT4 FWD 26.4849    Automatic
## 251           Infiniti                            QX60 FWD 22.3020          CVT
## 252           Cadillac                             XT4 AWD 24.2745    Automatic
## 253           Infiniti                            QX60 AWD 21.6894          CVT
## 254             Subaru                                 BRZ 23.7786       Manual
## 255             Subaru                                 BRZ 27.4825    Automatic
## 256             Subaru                              BRZ tS 22.9745       Manual
## 257          Chevrolet                              Camaro 15.5519    Automatic
## 258          Chevrolet                              Camaro 15.7384       Manual
## 259          Chevrolet                              Camaro 23.3581       Manual
## 260          Chevrolet                              Camaro 24.9925    Automatic
## 261          Chevrolet                              Camaro 19.7097       Manual
## 262              Lexus                              LC 500 18.6475    Automatic
## 263              Lexus                             LC 500h 29.6181          CVT
## 264                Kia                                 Rio 35.8416          CVT
## 265             Subaru                                 WRX 23.1422       Manual
## 266             Subaru                                 WRX 20.5416          CVT
## 267             Subaru                                 WRX 18.6117       Manual
## 268              Buick                               Regal 25.9100    Automatic
## 269              Buick                           Regal AWD 24.3822    Automatic
## 270            Hyundai                          Elantra GT 27.4890    Automatic
## 271            Hyundai                          Elantra GT 27.6399       Manual
## 272            Hyundai                          Elantra GT 25.5513       Manual
## 273             Subaru                          Legacy AWD 30.0538          CVT
## 274             Subaru                          Legacy AWD 26.9190          CVT
## 275              Buick                     Regal TourX AWD 24.3822    Automatic
## 276         Mitsubishi                   Eclipse Cross 2WD 26.0000          CVT
## 277         Mitsubishi                Eclipse Cross ES 2WD 27.0000          CVT
## 278         Mitsubishi                Eclipse Cross ES 4WD 26.1535          CVT
## 279             Subaru                        Forester AWD 28.6895          CVT
## 280             Subaru                         Outback AWD 28.7858          CVT
## 281             Subaru                         Outback AWD 25.8134          CVT
## 282           Cadillac                        Escalade 2WD 17.1741    Automatic
## 283          Chevrolet                     Tahoe C1500 2WD 17.5914    Automatic
## 284          Chevrolet                  Suburban C1500 2WD 17.5914    Automatic
## 285          Chevrolet                     Tahoe C1500 2WD 17.5914    Automatic
## 286          Chevrolet                  Suburban C1500 2WD 17.5914    Automatic
## 287          Chevrolet                     Tahoe C1500 2WD 17.1741    Automatic
## 288                GMC                     Yukon C1500 2WD 17.5914    Automatic
## 289                GMC                  Yukon C1500 XL 2WD 17.5914    Automatic
## 290                GMC                     Yukon C1500 2WD 17.5914    Automatic
## 291                GMC                  Yukon C1500 XL 2WD 17.5914    Automatic
## 292                GMC                     Yukon C1500 2WD 17.1741    Automatic
## 293                GMC                  Yukon C1500 XL 2WD 17.1741    Automatic
## 294           Cadillac                        Escalade 4WD 16.5637    Automatic
## 295          Chevrolet                     Tahoe K1500 4WD 16.9426    Automatic
## 296          Chevrolet                     Tahoe K1500 4WD 17.3200    Automatic
## 297          Chevrolet                  Suburban K1500 4WD 16.3683    Automatic
## 298          Chevrolet                     Tahoe K1500 4WD 16.8656    Automatic
## 299                GMC                  Yukon K1500 XL 4WD 16.3683    Automatic
## 300                GMC                     Yukon K1500 4WD 17.3200    Automatic
## 301                GMC                  Yukon K1500 XL 4WD 15.9496    Automatic
## 302                GMC                     Yukon K1500 4WD 16.8656    Automatic
## 303               Audi                       R8 Spyder AWD 15.9153       Manual
## 304      Mercedes-Benz                      AMG GT C Coupe 17.4463       Manual
## 305                Kia                         Stinger AWD 24.1415    Automatic
## 306                Kia                         Stinger RWD 24.5081    Automatic
## 307                Kia                         Stinger AWD 19.9185    Automatic
## 308                Kia                         Stinger RWD 20.1077    Automatic
## 309                Kia                         Sorento FWD 24.5180    Automatic
## 310                Kia                         Sorento FWD 21.6670    Automatic
## 311                Kia                         Sorento AWD 23.0550    Automatic
## 312                Kia                         Sorento AWD 20.4440    Automatic
## 313          Chevrolet                  Suburban C1500 2WD 17.1741    Automatic
## 314          Chevrolet                  Suburban K1500 4WD 15.9496    Automatic
## 315            Porsche                       Cayenne Turbo 16.9743    Automatic
## 316               Fiat                          124 Spider 29.2998    Automatic
## 317               Fiat                          124 Spider 29.6662       Manual
## 318             Nissan                                GT-R 18.3595       Manual
## 319            Genesis                             G90 AWD 19.9948    Automatic
## 320            Genesis                             G90 RWD 20.2402    Automatic
## 321            Genesis                             G90 AWD 18.0062    Automatic
## 322            Genesis                             G90 RWD 18.5923    Automatic
## 323          Chevrolet                        Colorado 2WD 23.3948    Automatic
## 324          Chevrolet                        Colorado 2WD 20.1722    Automatic
## 325          Chevrolet                        Colorado 2WD 21.8534    Automatic
## 326                GMC                          Canyon 2WD 23.3948    Automatic
## 327                GMC                          Canyon 2WD 20.1722    Automatic
## 328                GMC                          Canyon 2WD 21.8534    Automatic
## 329          Chevrolet                    Colorado ZR2 4WD 19.4160    Automatic
## 330          Chevrolet                        Colorado 4WD 22.3186    Automatic
## 331          Chevrolet                        Colorado 4WD 19.3190    Automatic
## 332          Chevrolet                    Colorado ZR2 4WD 16.6991    Automatic
## 333          Chevrolet                        Colorado 4WD 20.7689    Automatic
## 334                GMC                          Canyon 4WD 22.3186    Automatic
## 335                GMC                          Canyon 4WD 19.3190    Automatic
## 336                GMC                          Canyon 4WD 20.7689    Automatic
## 337               Ford                        EcoSport FWD 27.9226    Automatic
## 338               Ford                        EcoSport AWD 25.0000    Automatic
## 339          Chevrolet                  Suburban K1500 4WD 16.2524    Automatic
## 340                GMC                  Yukon K1500 XL 4WD 16.2524    Automatic
## 341                GMC                     Yukon K1500 4WD 16.9426    Automatic
## 342         Land Rover                         Range Rover 24.3843    Automatic
## 343         Land Rover                   Range Rover Sport 24.3843    Automatic
## 344         Land Rover                         Range Rover 18.0917    Automatic
## 345         Land Rover                     Range Rover LWB 18.0917    Automatic
## 346         Land Rover                     Range Rover SVA 15.5774    Automatic
## 347         Land Rover                 Range Rover LWB SVA 15.1147    Automatic
## 348         Land Rover                   Range Rover Sport 18.7053    Automatic
## 349         Land Rover               Range Rover Sport SVR 16.4401    Automatic
## 350                BMW                        Z4 sDrive30i 27.2748    Automatic
## 351             Jaguar                F-Type SVR AWD Coupe 18.4489    Automatic
## 352             Jaguar          F-Type SVR AWD Convertible 18.4489    Automatic
## 353          Chevrolet                              Camaro 22.4537    Automatic
## 354          Chevrolet                              Camaro 19.6116    Automatic
## 355              Buick                           Regal AWD 21.6380    Automatic
## 356              Honda                             Insight 51.9827          CVT
## 357              Honda                     Insight Touring 47.9750          CVT
## 358               Fiat                               500 L 24.9382    Automatic
## 359                Ram                            1500 2WD 21.8024    Automatic
## 360                Ram                            1500 2WD 19.0135    Automatic
## 361                Ram                            1500 2WD 17.2673    Automatic
## 362                Ram                        1500 HFE 2WD 22.5953    Automatic
## 363                Ram                            1500 4WD 21.2420    Automatic
## 364                Ram                            1500 4WD 18.7327    Automatic
## 365                Ram                            1500 4WD 17.0221    Automatic
## 366              Acura                             MDX FWD 23.0000    Automatic
## 367               Jeep                        Cherokee FWD 25.7892    Automatic
## 368               Jeep                        Cherokee FWD 25.2188    Automatic
## 369               Jeep                        Cherokee FWD 22.9779    Automatic
## 370             Nissan                           Rogue FWD 28.5783          CVT
## 371              Acura                             MDX AWD 22.0612    Automatic
## 372              Acura                      MDX AWD A-SPEC 21.3495    Automatic
## 373              Acura                      MDX Hybrid AWD 26.8723       Manual
## 374                BMW                                X3 M 16.1587    Automatic
## 375                BMW                    X3 M Competition 16.1587    Automatic
## 376                BMW                                X4 M 16.1587    Automatic
## 377                BMW                    X4 M Competition 16.1587    Automatic
## 378               Jeep                        Cherokee 4WD 24.0852    Automatic
## 379               Jeep                        Cherokee 4WD 24.1812    Automatic
## 380               Jeep                        Cherokee 4WD 22.1899    Automatic
## 381               Jeep        Cherokee 4WD Active Drive II 23.0251    Automatic
## 382               Jeep        Cherokee 4WD Active Drive II 21.2626    Automatic
## 383               Jeep              Cherokee Trailhawk 4WD 22.2086    Automatic
## 384               Jeep              Cherokee Trailhawk 4WD 20.6882    Automatic
## 385               Jeep                        Wrangler 4WD 20.4192    Automatic
## 386               Jeep              Wrangler Unlimited 4WD 20.3429    Automatic
## 387               Jeep              Wrangler Unlimited 4WD 20.4477    Automatic
## 388      Mercedes-Benz                              GLE350 22.4215    Automatic
## 389             Nissan                           Rogue AWD 27.4629          CVT
## 390            Porsche                               Macan 21.1897       Manual
## 391            Porsche                             Macan S 20.1000       Manual
## 392            Porsche                             Cayenne 20.4628    Automatic
## 393            Porsche                           Cayenne S 19.7414    Automatic
## 394            Ferrari                           488 Pista 16.8163       Manual
## 395            Ferrari                    488 Pista Spider 16.6115       Manual
## 396            Ferrari                       812 Superfast 13.3412       Manual
## 397            Ferrari                           GTC4Lusso 13.3849       Manual
## 398            Ferrari                         GTC4Lusso T 16.9940       Manual
## 399            Ferrari                           Portofino 18.1715       Manual
## 400               MINI                  Cooper Convertible 31.3799       Manual
## 401               MINI                Cooper S Convertible 29.0313       Manual
## 402               MINI       John Cooper Works Convertible 28.2052    Automatic
## 403                BMW                          840i Coupe 25.4747    Automatic
## 404                BMW                   840i xDrive Coupe 23.1997    Automatic
## 405                BMW                    840i Convertible 24.4737    Automatic
## 406                BMW             840i xDrive Convertible 22.6982    Automatic
## 407                BMW                  M850i xDrive Coupe 20.3576    Automatic
## 408                BMW            M850i xDrive Convertible 19.5915    Automatic
## 409      Mercedes-Benz           AMG E53 4matic Plus Coupe 23.3607    Automatic
## 410      Mercedes-Benz     AMG E53 4matic Plus Convertible 22.5517    Automatic
## 411               MINI               Cooper Hardtop 2 door 31.3799       Manual
## 412               MINI               Cooper Hardtop 4 door 31.3799       Manual
## 413               MINI             Cooper S Hardtop 2 door 29.7917       Manual
## 414               MINI             Cooper S Hardtop 4 door 29.7917       Manual
## 415               MINI           John Cooper Works Hardtop 28.9090    Automatic
## 416                BMW                         330i xDrive 28.2689    Automatic
## 417              Mazda                        3 4-Door 4WD 28.3285    Automatic
## 418      Mercedes-Benz               AMG CLS53 4matic Plus 23.4607    Automatic
## 419      Mercedes-Benz                              CLS450 26.4979    Automatic
## 420      Mercedes-Benz                       CLS450 4matic 25.7655    Automatic
## 421      Mercedes-Benz               AMG GT 53 4matic Plus 20.9556    Automatic
## 422             Toyota                             Prius c 46.2390          CVT
## 423              Acura                                 RLX 23.1967    Automatic
## 424              Acura                          RLX Hybrid 28.0000       Manual
## 425                BMW                        X2 xDrive28i 26.5937    Automatic
## 426                BMW                        X2 sDrive28i 26.7696    Automatic
## 427                BMW                             X2 M35i 26.1360    Automatic
## 428                BMW                                530i 28.0063    Automatic
## 429                BMW                         530i xDrive 26.6200    Automatic
## 430                BMW                         540i xDrive 24.5634    Automatic
## 431                BMW                        M550i xDrive 20.3576    Automatic
## 432                BMW                     840i Gran Coupe 24.4737    Automatic
## 433                BMW              840i xDrive Gran Coupe 22.6982    Automatic
## 434                BMW             M850i xDrive Gran Coupe 19.5915    Automatic
## 435           Maserati                          Ghibli SQ4 18.6881    Automatic
## 436           Maserati                              Ghibli 19.3234    Automatic
## 437           Maserati                            Ghibli S 19.3234    Automatic
## 438              Mazda                        3 5-Door 2WD 29.1829       Manual
## 439              Mazda                        3 5-Door 2WD 29.7883    Automatic
## 440              Mazda                        3 5-Door 4WD 27.0000    Automatic
## 441      Mercedes-Benz                 AMG E53 4matic Plus 24.0100    Automatic
## 442               MINI                   Cooper Countryman 28.7714       Manual
## 443               MINI              Cooper Countryman All4 27.3761    Automatic
## 444               MINI                    Cooper S Clubman 29.0313       Manual
## 445               MINI            Cooper S Countryman All4 25.8460    Automatic
## 446               MINI      John Cooper Works Clubman All4 26.0968    Automatic
## 447               MINI                 JCW Countryman All4 26.1323    Automatic
## 448                BMW                        X1 xDrive28i 25.7833    Automatic
## 449                BMW                           Alpina B7 19.5915    Automatic
## 450           Cadillac                             CT6 AWD 21.1796    Automatic
## 451          Chevrolet                              Impala 21.9191    Automatic
## 452          Chevrolet                              Impala 21.6458    Automatic
## 453              Honda                           Civic 5Dr 32.0168       Manual
## 454              Honda                           Civic 5Dr 34.2168          CVT
## 455              Honda                           Civic 5Dr 31.9837          CVT
## 456           Maserati                    Quattroporte GTS 17.1931    Automatic
## 457           Maserati                    Quattroporte SQ4 18.3114    Automatic
## 458           Maserati                      Quattroporte S 19.3234    Automatic
## 459                GMC                         Terrain FWD 24.0000    Automatic
## 460              Honda                           Pilot FWD 22.7022    Automatic
## 461              Honda                           Pilot FWD 22.3536    Automatic
## 462         Mitsubishi                 Outlander Sport 2WD 26.5981          CVT
## 463                BMW                        X3 xDrive30i 25.6541    Automatic
## 464                BMW                             X3 M40i 23.1538    Automatic
## 465                BMW                        X4 xDrive30i 24.9692    Automatic
## 466                BMW                             X4 M40i 23.1538    Automatic
## 467              Honda                           Pilot AWD 22.0581    Automatic
## 468              Honda                           Pilot AWD 21.1417    Automatic
## 469               Jeep                        Wrangler 4WD 22.8165    Automatic
## 470               Jeep                        Wrangler 4WD 19.9011       Manual
## 471               Jeep              Wrangler Unlimited 4WD 21.4822    Automatic
## 472               Jeep              Wrangler Unlimited 4WD 21.0000    Automatic
## 473               Jeep              Wrangler Unlimited 4WD 19.3398       Manual
## 474         Mitsubishi                 Outlander Sport 4WD 25.5957          CVT
## 475         Mitsubishi                   Eclipse Cross 4WD 25.3444          CVT
## 476              Dodge                         Durango RWD 21.4537    Automatic
## 477              Dodge                         Durango RWD 16.9243    Automatic
## 478               Jeep                  Grand Cherokee 2WD 21.4537    Automatic
## 479                BMW                        X5 xDrive50i 18.1312    Automatic
## 480                BMW                             X5 M50i 18.1312    Automatic
## 481                BMW                             X6 M50i 18.1312    Automatic
## 482                BMW                        X7 xDrive50i 17.2627    Automatic
## 483                BMW                             X7 M50i 17.2627    Automatic
## 484              Dodge                         Durango AWD 20.7754    Automatic
## 485              Dodge                         Durango AWD 16.6887    Automatic
## 486               Jeep                  Grand Cherokee 4WD 20.7754    Automatic
## 487               Jeep                  Grand Cherokee 4WD 16.6887    Automatic
## 488         Land Rover                           Discovery 17.7171    Automatic
## 489         Land Rover                           Discovery 22.6844    Automatic
## 490           Maserati                           Levante S 17.1134    Automatic
## 491           Maserati                             Levante 17.1134    Automatic
## 492           Maserati                         Levante GTS 15.3607    Automatic
## 493           Maserati                      Levante Trofeo 15.3607    Automatic
## 494      Mercedes-Benz                       GLS450 4matic 20.6956    Automatic
## 495      Mercedes-Benz                               SL550 20.2956    Automatic
## 496      Mercedes-Benz                               SL450 22.4994    Automatic
## 497          Chevrolet                              Camaro 18.6287       Manual
## 498      Mercedes-Benz                          E450 Coupe 22.9182    Automatic
## 499      Mercedes-Benz                   E450 4matic Coupe 21.7390    Automatic
## 500      Mercedes-Benz                    E450 Convertible 22.6853    Automatic
## 501      Mercedes-Benz             E450 4matic Convertible 21.8041    Automatic
## 502             Nissan                               Versa 35.0880          CVT
## 503             Nissan                               Versa 30.0000       Manual
## 504               Ford                          Fusion FWD 27.1647    Automatic
## 505               Ford                          Fusion AWD 23.1356    Automatic
## 506               Ford                          Fusion FWD 24.8156    Automatic
## 507               Ford                          Fusion FWD 24.3707    Automatic
## 508               Ford                   Fusion Hybrid FWD 42.0000          CVT
## 509               Ford                  Fusion Hybrid Taxi 41.0000          CVT
## 510            Lincoln                             MKZ AWD 23.1356    Automatic
## 511            Lincoln                             MKZ FWD 23.8346    Automatic
## 512            Lincoln                             MKZ AWD 19.9357    Automatic
## 513            Lincoln                             MKZ FWD 20.9122    Automatic
## 514            Lincoln                      MKZ Hybrid FWD 41.0000          CVT
## 515      Mercedes-Benz                         E450 4matic 22.7114    Automatic
## 516               MINI                 Cooper S Countryman 27.5119       Manual
## 517             Toyota                          Avalon XLE 25.9247    Automatic
## 518             Toyota                              Avalon 24.9381    Automatic
## 519             Toyota                       Avalon Hybrid 43.2262          CVT
## 520             Toyota                   Avalon Hybrid XLE 43.6328          CVT
## 521      Mercedes-Benz                                S450 21.9730    Automatic
## 522      Mercedes-Benz                         S450 4matic 21.7132    Automatic
## 523      Mercedes-Benz         E450 4matic (station wagon) 21.8116    Automatic
## 524          Chevrolet                       Silverado 2WD 17.0000    Automatic
## 525          Chevrolet                       Silverado 2WD 19.3275    Automatic
## 526          Chevrolet                       Silverado 2WD 20.0462    Automatic
## 527          Chevrolet                       Silverado 2WD 26.5885    Automatic
## 528          Chevrolet                       Silverado 2WD 16.9515    Automatic
## 529          Chevrolet                       Silverado 2WD 16.3070    Automatic
## 530          Chevrolet                       Silverado 2WD 21.0999    Automatic
## 531                GMC                          Sierra 2WD 17.0000    Automatic
## 532                GMC                          Sierra 2WD 19.2176    Automatic
## 533                GMC                          Sierra 2WD 20.0462    Automatic
## 534                GMC                          Sierra 2WD 25.9462    Automatic
## 535                GMC                          Sierra 2WD 16.9515    Automatic
## 536                GMC                          Sierra 2WD 16.3070    Automatic
## 537                GMC                          Sierra 2WD 21.0999    Automatic
## 538             Toyota                          Tundra 2WD 14.9383    Automatic
## 539          Chevrolet                       Silverado 4WD 18.0000    Automatic
## 540          Chevrolet             Silverado 4WD TrailBoss 18.2856    Automatic
## 541          Chevrolet                       Silverado 4WD 18.6350    Automatic
## 542          Chevrolet             Silverado 4WD TrailBoss 18.0000    Automatic
## 543          Chevrolet                       Silverado 4WD 20.0462    Automatic
## 544          Chevrolet                       Silverado 4WD 17.2229    Automatic
## 545          Chevrolet                       Silverado 4WD 16.9390    Automatic
## 546          Chevrolet             Silverado 4WD TrailBoss 15.8234    Automatic
## 547          Chevrolet                       Silverado 4WD 15.7301    Automatic
## 548          Chevrolet                       Silverado 4WD 25.1332    Automatic
## 549          Chevrolet                       Silverado 4WD 20.0390    Automatic
## 550          Chevrolet                       Silverado 4WD 16.7240    Automatic
## 551          Chevrolet             Silverado 4WD TrailBoss 16.0000    Automatic
## 552                GMC                          Sierra 4WD 18.1521    Automatic
## 553                GMC                      Sierra 4WD AT4 18.2856    Automatic
## 554                GMC                          Sierra 4WD 18.5543    Automatic
## 555                GMC                      Sierra 4WD AT4 18.0000    Automatic
## 556                GMC                          Sierra 4WD 20.0462    Automatic
## 557                GMC                          Sierra 4WD 16.9303    Automatic
## 558                GMC                      Sierra 4WD AT4 17.2528    Automatic
## 559                GMC                          Sierra 4WD 16.7900    Automatic
## 560                GMC                      Sierra 4WD AT4 16.0000    Automatic
## 561                GMC                          Sierra 4WD 23.8267    Automatic
## 562                GMC                      Sierra 4WD AT4 23.8267    Automatic
## 563                GMC                          Sierra 4WD 20.0390    Automatic
## 564                GMC                          Sierra 4WD 16.7240    Automatic
## 565             Toyota                          Tundra 4WD 14.4695    Automatic
## 566          Chevrolet           Silverado Cab Chassis 2WD 17.0000    Automatic
## 567          Chevrolet           Silverado Cab Chassis 4WD 15.0000    Automatic
## 568          Chevrolet           Silverado Cab Chassis 2WD 15.2644    Automatic
## 569          Chevrolet           Silverado Cab Chassis 4WD 14.9623    Automatic
## 570                GMC              Sierra Cab Chassis 2WD 17.0000    Automatic
## 571                GMC              Sierra Cab Chassis 4WD 14.9623    Automatic
## 572                GMC              Sierra Cab Chassis 4WD 15.0000    Automatic
## 573                GMC              Sierra Cab Chassis 2WD 15.2644    Automatic
## 574           Cadillac                             XT5 FWD 21.0733    Automatic
## 575          Chevrolet                          Blazer FWD 23.0837    Automatic
## 576               Jeep                         Compass FWD 25.4013    Automatic
## 577               Jeep                         Compass FWD 26.1675       Manual
## 578      Mercedes-Benz                              GLC300 24.2628    Automatic
## 579                BMW                        X3 sDrive30i 26.5740    Automatic
## 580           Cadillac                             XT5 AWD 20.3484    Automatic
## 581               Jeep                         Compass 4WD 24.8726    Automatic
## 582               Jeep                         Compass 4WD 25.0366       Manual
## 583      Mercedes-Benz                       GLC300 4matic 23.5319    Automatic
## 584                GMC                          Acadia FWD 23.0837    Automatic
## 585                GMC                          Acadia FWD 21.5304    Automatic
## 586             Toyota                         Sequoia 2WD 14.8192    Automatic
## 587                GMC                          Acadia AWD 20.5591    Automatic
## 588             Toyota                         Sequoia 4WD 14.3605    Automatic
## 589            Ferrari                          F8 Tributo 16.3165       Manual
## 590              Honda                           Civic 2Dr 30.0000       Manual
## 591              Mazda                        3 4-Door 2WD 29.6180    Automatic
## 592              Mazda                        3 4-Door 2WD 30.1770    Automatic
## 593      Mercedes-Benz                         C300 4matic 26.2415    Automatic
## 594      Mercedes-Benz                                C300 27.9657    Automatic
## 595              Honda                           Civic 4Dr 30.0000       Manual
## 596               MINI               Cooper S Clubman All4 26.3903    Automatic
## 597          Chevrolet                       Silverado 2WD 19.8308    Automatic
## 598                GMC                          Sierra 2WD 19.8308    Automatic
## 599           Chrysler                            Pacifica 22.2452    Automatic
## 600           Chrysler                             Voyager 22.2452    Automatic
## 601              Honda                             Odyssey 22.4504    Automatic
## 602      Mercedes-Benz                 GLC300 4matic Coupe 23.7303    Automatic
## 603              Volvo                            XC90 AWD 20.7216    Automatic
## 604               Ford        Fusion Energi Plug-in Hybrid 41.7000          CVT
## 605               Ford         Fusion Special Service PHEV 41.5000          CVT
## 606       Aston Martin                                 DBS 17.0315    Automatic
## 607              Lotus                               Evora 18.9178       Manual
## 608              Lotus                               Evora 19.5953    Automatic
## 609              Lotus                            Evora GT 20.0210       Manual
## 610              Lotus                            Evora GT 19.8407    Automatic
## 611      Mercedes-Benz                          C300 Coupe 25.1372    Automatic
## 612      Mercedes-Benz                   C300 4matic Coupe 24.6703    Automatic
## 613      Mercedes-Benz             C300 4matic Convertible 23.7755    Automatic
## 614      Mercedes-Benz                    C300 Convertible 23.8668    Automatic
## 615              Lexus                              ES 350 25.5162    Automatic
## 616              Lexus                      ES 350 F Sport 25.0685    Automatic
## 617              Lexus                             ES 300h 44.0943          CVT
## 618             Nissan                              Maxima 24.2133          CVT
## 619             Toyota                               Prius 52.2434          CVT
## 620             Toyota                           Prius AWD 50.0000          CVT
## 621             Toyota                           Prius Eco 55.7000          CVT
## 622            Porsche                            Panamera 22.0000       Manual
## 623            Porsche                          Panamera 4 22.3176       Manual
## 624            Porsche                Panamera 4 Executive 22.3176       Manual
## 625            Porsche                       Panamera 4 ST 21.9285       Manual
## 626            Porsche                         Panamera 4S 21.0000       Manual
## 627            Porsche               Panamera 4S Executive 21.0000       Manual
## 628            Porsche                      Panamera 4S ST 20.0000       Manual
## 629            Porsche                        Panamera GTS 18.8765       Manual
## 630            Porsche                     Panamera GTS ST 18.2114       Manual
## 631            Porsche                      Panamera Turbo 20.9847       Manual
## 632            Porsche                 Panamera Turbo Exec 20.9847       Manual
## 633            Porsche                   Panamera Turbo ST 19.7064       Manual
## 634      Mercedes-Benz              Metris (Passenger Van) 21.4657    Automatic
## 635      Mercedes-Benz                  Metris (Cargo Van) 22.3437    Automatic
## 636      Mercedes-Benz             Metris (Cargo Van, LWB) 22.4720    Automatic
## 637      Mercedes-Benz              Metris (Passenger Van) 20.8058    Automatic
## 638      Mercedes-Benz                  Metris (Cargo Van) 21.7289    Automatic
## 639      Mercedes-Benz             Metris (Cargo Van, LWB) 21.9313    Automatic
## 640                Ram                      Promaster City 23.7776    Automatic
## 641               Ford                    Explorer HEV RWD 27.6521    Automatic
## 642             Toyota                         4Runner 2WD 17.4613    Automatic
## 643               Ford                    Explorer HEV AWD 24.5194    Automatic
## 644             Toyota                         4Runner 4WD 17.0999    Automatic
## 645             Toyota              Land Cruiser Wagon 4WD 14.2639    Automatic
## 646             Toyota                         4Runner 4WD 17.0999    Automatic
## 647           Chrysler                     Pacifica Hybrid 29.5207          CVT
## 648              Volvo                        S60 AWD PHEV 30.3000    Automatic
## 649              Volvo                        S90 AWD PHEV 29.7000    Automatic
## 650              Volvo                        V60 AWD PHEV 30.3000    Automatic
## 651              Volvo                       XC60 AWD PHEV 26.6000    Automatic
## 652              Volvo                       XC90 AWD PHEV 26.8000    Automatic
## 653             Toyota                                  86 26.9993    Automatic
## 654             Toyota                                  86 23.7198       Manual
## 655               Ford                             Mustang 24.3461       Manual
## 656               Ford                             Mustang 25.0787    Automatic
## 657               Ford                             Mustang 18.3457       Manual
## 658               Ford                 Mustang Convertible 23.0000       Manual
## 659               Ford                 Mustang Convertible 23.0430    Automatic
## 660               Ford                             Mustang 18.8281    Automatic
## 661               Ford                 Mustang Convertible 18.3437    Automatic
## 662               Ford                    Mustang HO Coupe 23.0000    Automatic
## 663               Ford              Mustang HO Convertible 23.0000    Automatic
## 664               Ford                    Mustang HO Coupe 22.8518       Manual
## 665               Ford              Mustang HO Convertible 21.8873       Manual
## 666      Mercedes-Benz                                E350 26.2664    Automatic
## 667                GMC                          Sierra 4WD 16.1974    Automatic
## 668          Chevrolet                          Blazer FWD 21.4398    Automatic
## 669              Lexus                            RX 350 L 21.9903    Automatic
## 670              Lexus                              RX 350 22.6244    Automatic
## 671          Chevrolet                          Blazer AWD 20.5591    Automatic
## 672         Land Rover                     Discovery Sport 20.8177    Automatic
## 673              Lexus                          RX 350 AWD 21.8285    Automatic
## 674              Lexus                        RX 350 L AWD 21.2370    Automatic
## 675              Dodge                     Durango SRT AWD 15.1196    Automatic
## 676               Jeep              Grand Cherokee SRT 4WD 14.9466    Automatic
## 677               Jeep        Grand Cherokee Trackhawk 4WD 13.3061    Automatic
## 678              Lexus                         RX 450h AWD 29.7574          CVT
## 679              Lexus                       RX 450h L AWD 29.0000          CVT
## 680              Lexus                              LX 570 13.8684    Automatic
## 681               Audi                 TT Roadster quattro 25.9952       Manual
## 682            Bentley          Continental GT Convertible 18.9656       Manual
## 683            Porsche                       911 Carrera S 20.0000       Manual
## 684            Porsche             911 Carrera S Cabriolet 20.0000       Manual
## 685            Porsche                      911 Carrera 4S 20.0000       Manual
## 686               Audi                          A3 quattro 24.7718       Manual
## 687               Audi                A3 Cabriolet quattro 24.7718       Manual
## 688               Audi                    TT Coupe quattro 25.9952       Manual
## 689               Audi                                  S3 24.6914       Manual
## 690               Audi                           TTS Coupe 25.0431       Manual
## 691            Bentley                      Continental GT 18.9656       Manual
## 692                BMW                            M8 Coupe 17.0858    Automatic
## 693                BMW                M8 Competition Coupe 17.0858    Automatic
## 694                BMW                      M8 Convertible 17.0858    Automatic
## 695                BMW          M8 Competition Convertible 17.0858    Automatic
## 696               Ford                Shelby GT350 Mustang 16.1976       Manual
## 697               Ford                     Mustang Bullitt 17.3194       Manual
## 698      Mercedes-Benz                       CLA250 4matic 26.8359       Manual
## 699      Mercedes-Benz                              CLA250 28.4696       Manual
## 700        Rolls-Royce                                Dawn 13.8915    Automatic
## 701             Toyota                               Yaris 34.2683       Manual
## 702             Toyota                               Yaris 35.0000    Automatic
## 703               Audi                          A6 quattro 27.0160       Manual
## 704                BMW                                  M5 17.0858    Automatic
## 705                BMW                      M5 Competition 17.0858    Automatic
## 706                BMW                       M8 Gran Coupe 17.0858    Automatic
## 707      Mercedes-Benz                         E350 4matic 24.9303    Automatic
## 708        Rolls-Royce                              Wraith 14.1837    Automatic
## 709             Subaru                      Impreza 4-Door 26.2667       Manual
## 710             Subaru                      Impreza 4-Door 31.3620          CVT
## 711             Subaru                Impreza Sport 4-Door 26.2235       Manual
## 712             Subaru                Impreza Sport 4-Door 30.4430          CVT
## 713             Toyota                               Camry 26.1901    Automatic
## 714             Toyota                           Camry XSE 25.6327    Automatic
## 715             Toyota                         Camry LE/SE 31.8036    Automatic
## 716             Toyota                       Camry XLE/XSE 31.2823    Automatic
## 717             Toyota                               Camry 33.9783    Automatic
## 718             Toyota                     Camry Hybrid LE 51.7504          CVT
## 719             Toyota                 Camry Hybrid XLE/SE 46.2351          CVT
## 720             Toyota                           Camry TRD 25.1187    Automatic
## 721                BMW                        X1 sDrive28i 27.1815    Automatic
## 722        Rolls-Royce                             Phantom 14.3910    Automatic
## 723        Rolls-Royce                         Phantom EWB 14.3910    Automatic
## 724        Rolls-Royce                               Ghost 13.8915    Automatic
## 725        Rolls-Royce                           Ghost EWB 13.8915    Automatic
## 726             Subaru                      Impreza 5-Door 26.4104       Manual
## 727             Subaru                      Impreza 5-Door 30.7650          CVT
## 728             Subaru                Impreza Sport 5-Door 25.2689       Manual
## 729             Subaru                Impreza Sport 5-Door 30.2489          CVT
## 730        Rolls-Royce                            Cullinan 14.3027    Automatic
## 731        Rolls-Royce                Cullinan Black Badge 14.3027    Automatic
## 732             Toyota                          Tacoma 2WD 20.6457    Automatic
## 733             Toyota                          Tacoma 2WD 21.0804    Automatic
## 734             Toyota                          Tacoma 4WD 19.8415    Automatic
## 735             Toyota                          Tacoma 4WD 18.4907       Manual
## 736             Toyota                          Tacoma 4WD 20.0556    Automatic
## 737             Toyota     Tacoma 4WD D-CAB MT TRD-ORP/PRO 18.3106       Manual
## 738               Jeep                        Renegade 2WD 26.9515    Automatic
## 739               Jeep                        Renegade 2WD 24.8726    Automatic
## 740            Lincoln                        Nautilus FWD 23.0000    Automatic
## 741      Mercedes-Benz                              GLA250 27.9066       Manual
## 742         Mitsubishi                 Outlander Sport 2WD 25.0000          CVT
## 743               Audi                          Q3 quattro 22.1236    Automatic
## 744               Jeep                        Renegade 4WD 25.5586    Automatic
## 745               Jeep                        Renegade 4WD 23.9283    Automatic
## 746               Jeep              Renegade Trailhawk 4WD 23.8332    Automatic
## 747      Mercedes-Benz                       GLA250 4matic 25.4592       Manual
## 748         Mitsubishi                       Outlander 4WD 22.0000    Automatic
## 749         Mitsubishi                 Outlander Sport 4WD 25.2200          CVT
## 750             Subaru                       Crosstrek AWD 24.9374       Manual
## 751             Subaru                       Crosstrek AWD 29.8038          CVT
## 752             Nissan                          Armada 2WD 16.0715    Automatic
## 753            Bentley                            Bentayga 17.3377    Automatic
## 754                BMW                        X5 xDrive40i 22.2797    Automatic
## 755                BMW                        X6 xDrive40i 22.2797    Automatic
## 756                BMW                        X7 xDrive40i 21.7788    Automatic
## 757        Lamborghini                                Urus 13.9329    Automatic
## 758             Nissan                          Armada 4WD 15.0000    Automatic
## 759      Mercedes-Benz                   AMG GT C Roadster 16.8371       Manual
## 760            Porsche            911 Carrera 4S Cabriolet 20.0000       Manual
## 761              Honda           Accord 2.0T Sport/Touring 25.9525    Automatic
## 762           Infiniti                                 Q50 23.0508    Automatic
## 763           Infiniti                             Q50 AWD 21.8651    Automatic
## 764           Infiniti                       Q50 Red Sport 22.0312    Automatic
## 765           Infiniti                  Q50 Red Sport  AWD 21.9477    Automatic
## 766              Honda                              Accord 29.7143       Manual
## 767              Honda                              Accord 33.3031          CVT
## 768              Honda                              Accord 31.3482          CVT
## 769              Honda                              Accord 25.6759       Manual
## 770              Honda                              Accord 27.0324    Automatic
## 771               Ford                          Escape FWD 30.2418    Automatic
## 772               Ford                            Edge FWD 23.9752    Automatic
## 773            Lincoln                         Corsair FWD 24.6965    Automatic
## 774             Nissan                      Pathfinder 2WD 22.6338          CVT
## 775               Ford                            Edge AWD 23.0000    Automatic
## 776               Ford                            Edge AWD 21.2789    Automatic
## 777               Ford                            Edge AWD 23.0000    Automatic
## 778            Lincoln                        Nautilus AWD 22.0000    Automatic
## 779            Lincoln                        Nautilus AWD 21.2789    Automatic
## 780            Lincoln                         Corsair AWD 23.9170    Automatic
## 781             Nissan                      Pathfinder 4WD 22.1479          CVT
## 782             Nissan             Pathfinder 4WD Platinum 21.4539          CVT
## 783                BMW                        X5 sDrive40i 22.5876    Automatic
## 784                BMW                        X6 sDrive40i 22.5876    Automatic
## 785      Mercedes-Benz                      AMG GT R Coupe 16.7621       Manual
## 786      Mercedes-Benz                   AMG GT R Roadster 16.7621       Manual
## 787             Toyota                   Corolla Hatchback 31.4664       Manual
## 788             Toyota                   Corolla Hatchback 35.4774          CVT
## 789             Toyota               Corolla Hatchback XSE 33.1681          CVT
## 790            Hyundai                               Venue 31.5397          CVT
## 791            Hyundai                               Venue 30.2128       Manual
## 792             Nissan                          Murano FWD 23.2709          CVT
## 793             Nissan                          Murano AWD 22.9002          CVT
## 794           Infiniti                                QX50 26.1476          CVT
## 795         Volkswagen                   Atlas Cross Sport 22.1829    Automatic
## 796         Volkswagen                              Tiguan 24.9100    Automatic
## 797               Audi                                  Q5 24.2823    Automatic
## 798           Infiniti                            QX50 AWD 25.0911          CVT
## 799         Volkswagen                      Tiguan 4motion 22.9512    Automatic
## 800         Land Rover                    Range Rover MHEV 21.4153    Automatic
## 801              Lexus                              GX 460 16.2653    Automatic
## 802                BMW                                530e 27.0819    Automatic
## 803                BMW                         530e xDrive 25.3320    Automatic
## 804         Mitsubishi                      Outlander PHEV 25.0000    Automatic
## 805            Porsche           Panamera Turbo S e-Hybrid 20.4000       Manual
## 806            Porsche Panamera Turbo S e-Hybrid Executive 20.4000       Manual
## 807            Porsche        Panamera Turbo S e-Hybrid ST 20.4000       Manual
## 808               Audi                                  A3 30.1793       Manual
## 809      Mercedes-Benz                         A220 4matic 27.5000    Automatic
## 810      Mercedes-Benz                                A220 28.3488    Automatic
## 811              Lexus                          IS 300 AWD 21.0000    Automatic
## 812              Lexus                              IS 350 22.8206    Automatic
## 813              Lexus                          IS 350 AWD 21.0000    Automatic
## 814              Lexus                                GS F 18.7510    Automatic
## 815              Lexus                              IS 300 24.2954    Automatic
## 816      Mercedes-Benz               AMG GT 63 4matic Plus 17.1795    Automatic
## 817      Mercedes-Benz             AMG GT 63 S 4matic Plus 17.1261    Automatic
## 818         Volkswagen                                 GTI 27.0448       Manual
## 819         Volkswagen                               Jetta 27.7257       Manual
## 820         Volkswagen                               Jetta 28.0564       Manual
## 821                BMW                                540i 25.3700    Automatic
## 822              Lexus                              GS 350 22.8206    Automatic
## 823              Lexus                          GS 350 AWD 21.0000    Automatic
## 824              Lexus                      GS 350 F Sport 21.8089    Automatic
## 825              Lexus                             LS 500h 27.9367          CVT
## 826              Lexus                         LS 500h AWD 25.7818          CVT
## 827              Lexus                              LS 500 22.8888    Automatic
## 828              Lexus                          LS 500 AWD 21.1633    Automatic
## 829              Honda                       Accord Hybrid 47.5685          CVT
## 830         Alfa Romeo                         Stelvio AWD 19.1410    Automatic
## 831               Ford                Shelby GT500 Mustang 14.0853       Manual
## 832              Honda                           Civic 2Dr 32.7794          CVT
## 833              Honda                           Civic 2Dr 34.5202          CVT
## 834              Honda                           Civic 2Dr 29.0774       Manual
## 835              Honda                           Civic 2Dr 31.5556          CVT
## 836              Honda                           Civic 2Dr 33.3494          CVT
## 837             Toyota                                C-HR 28.8126          CVT
## 838         Alfa Romeo                              Giulia 26.9548    Automatic
## 839         Alfa Romeo                          Giulia AWD 25.5433    Automatic
## 840         Alfa Romeo                              Giulia 20.3144    Automatic
## 841              Honda                           Civic 4Dr 33.2383          CVT
## 842              Honda                           Civic 4Dr 35.6082          CVT
## 843              Honda                           Civic 4Dr 29.4217       Manual
## 844              Honda                           Civic 4Dr 32.0019          CVT
## 845              Honda                           Civic 4Dr 33.0274          CVT
## 846                Ram                            1500 2WD 26.0473    Automatic
## 847                Ram                            1500 4WD 24.2793    Automatic
## 848             Nissan                     NV200 Cargo Van 25.1814          CVT
## 849         Alfa Romeo                             Stelvio 25.3266    Automatic
## 850         Alfa Romeo                         Stelvio AWD 24.4128    Automatic
## 851         Volkswagen           Atlas Cross Sport 4motion 20.0891    Automatic
## 852        Lamborghini                     Aventador Coupe 10.9357       Manual
## 853        Lamborghini                  Aventador Roadster 10.7137       Manual
## 854           Infiniti                                 Q60 22.4496    Automatic
## 855           Infiniti                       Q60 Red Sport 22.0000    Automatic
## 856           Infiniti                   Q60 Red Sport AWD 21.0000    Automatic
## 857           Infiniti                             Q60 AWD 21.7714    Automatic
## 858              Acura                                 ILX 27.9249       Manual
## 859               Ford                 F150 Pickup 2WD FFV 21.7173    Automatic
## 860               Ford                 F150 Pickup 2WD FFV 19.0230    Automatic
## 861               Ford        F150 2WD FFV BASE PAYLOAD LT 18.0000    Automatic
## 862               Ford     F150 5.0L 2WD FFV GVWR>7599 LBS 17.0000    Automatic
## 863               Ford                     F150 Pickup 2WD 19.0102    Automatic
## 864               Ford             F150 Pickup 2WD Limited 19.0111    Automatic
## 865               Ford                     F150 Pickup 2WD 22.1016    Automatic
## 866               Ford                 F150 Pickup 4WD FFV 20.2236    Automatic
## 867               Ford                 F150 Pickup 4WD FFV 17.5403    Automatic
## 868               Ford                     F150 Pickup 4WD 18.1783    Automatic
## 869               Ford             F150 Pickup 4WD Limited 18.8698    Automatic
## 870               Ford                     F150 RAPTOR 4WD 16.4075    Automatic
## 871               Ford                     F150 Pickup 4WD 20.3813    Automatic
## 872      Mercedes-Benz                              GLB250 25.5879       Manual
## 873             Toyota                                RAV4 29.8882    Automatic
## 874             Toyota                                RAV4 30.3957    Automatic
## 875         Volkswagen                               Atlas 21.6896    Automatic
## 876      Mercedes-Benz                       GLB250 4matic 26.0968       Manual
## 877             Toyota                            RAV4 AWD 28.0938    Automatic
## 878             Toyota                            RAV4 AWD 29.2767    Automatic
## 879             Toyota                     RAV4 Hybrid AWD 40.1750          CVT
## 880             Toyota                RAV4 AWD TRD OFFROAD 27.4302    Automatic
## 881             Toyota                         RAV4 AWD LE 29.8215    Automatic
## 882      Mercedes-Benz                             AMG G63 13.8444    Automatic
## 883              Acura                          NSX Hybrid 21.0000       Manual
## 884      Mercedes-Benz                    S560 Convertible 20.1785    Automatic
## 885      Mercedes-Benz     AMG S63 4matic Plus Convertible 17.9865    Automatic
## 886      Mercedes-Benz                   S560 4matic Coupe 20.1091    Automatic
## 887      Mercedes-Benz           AMG S63 4matic Plus Coupe 20.2762    Automatic
## 888      Mercedes-Benz                                S560 20.5095    Automatic
## 889      Mercedes-Benz                 AMG S63 4matic Plus 20.0251    Automatic
## 890      Mercedes-Benz                         S560 4matic 20.7551    Automatic
## 891      Mercedes-Benz                 S560 4matic Maybach 19.4129    Automatic
## 892              Honda                            HR-V FWD 30.2500          CVT
## 893              Honda                            HR-V FWD 30.2500          CVT
## 894              Honda                            HR-V AWD 28.5595          CVT
## 895              Honda                            HR-V AWD 28.2825          CVT
## 896               Fiat                            500X AWD 25.9757    Automatic
## 897      Mercedes-Benz               AMG GLC63 4matic Plus 18.1759    Automatic
## 898      Mercedes-Benz         AMG GLC63 4matic Plus Coupe 18.1716    Automatic
## 899      Mercedes-Benz       AMG GLC63 S 4matic Plus Coupe 18.1716    Automatic
## 900            Bugatti                              Chiron 10.5921       Manual
## 901      Mercedes-Benz                           AMG SLC43 23.1054    Automatic
## 902               Audi                                  S5 22.5073    Automatic
## 903               Audi                        S5 Cabriolet 21.8571    Automatic
## 904      Mercedes-Benz               AMG C63 S Convertible 19.6981    Automatic
## 905      Mercedes-Benz                 AMG C63 Convertible 19.6981    Automatic
## 906      Mercedes-Benz                     AMG C63 S Coupe 20.0502    Automatic
## 907      Mercedes-Benz                       AMG C63 Coupe 20.0502    Automatic
## 908      Mercedes-Benz                AMG C43 4matic Coupe 21.7146    Automatic
## 909      Mercedes-Benz          AMG C43 4matic Convertible 21.0704    Automatic
## 910               Audi                                  A4 29.9608       Manual
## 911               Audi                                  S4 22.5073    Automatic
## 912              Lexus                             UX 250h 42.0612          CVT
## 913              Lexus                         UX 250h AWD 39.0000          CVT
## 914      Mercedes-Benz                             AMG C63 20.9747    Automatic
## 915      Mercedes-Benz                           AMG C63 S 20.9747    Automatic
## 916      Mercedes-Benz                      AMG C43 4matic 21.9176    Automatic
## 917         Mitsubishi                              Mirage 36.3980       Manual
## 918         Mitsubishi                              Mirage 38.6074          CVT
## 919         Mitsubishi                           Mirage G4 35.0000       Manual
## 920         Mitsubishi                           Mirage G4 37.3939          CVT
## 921         Volkswagen                                 GTI 27.0113       Manual
## 922               Audi                        S5 Sportback 22.5073    Automatic
## 923               Audi                                  S6 21.5635    Automatic
## 924               Audi                                  S7 21.5635    Automatic
## 925               Audi                                  S8 16.2313    Automatic
## 926                BMW           M8 Competition Gran Coupe 17.0858    Automatic
## 927           Cadillac                                 CT5 26.1491    Automatic
## 928           Cadillac                             CT5 AWD 24.7046    Automatic
## 929              Lexus                              UX 200 32.5226          CVT
## 930      Mercedes-Benz               AMG E63 S 4matic Plus 17.5069    Automatic
## 931               Audi                                A8 L 17.8756    Automatic
## 932            Hyundai                               Ioniq 54.5885       Manual
## 933            Hyundai                          Ioniq Blue 57.7824       Manual
## 934            Hyundai                              Sonata 30.5776    Automatic
## 935            Hyundai                              Sonata 31.7911    Automatic
## 936            Hyundai                              Sonata 30.5860    Automatic
## 937            Lincoln                    Continental  AWD 19.6658    Automatic
## 938            Lincoln                    Continental  FWD 20.9822    Automatic
## 939            Lincoln                    Continental  AWD 19.0863    Automatic
## 940            Lincoln                    Continental  AWD 19.2516    Automatic
## 941            Lincoln                    Continental  FWD 20.1608    Automatic
## 942      Mercedes-Benz       AMG E63 S 4matic Plus (wagon) 18.5852    Automatic
## 943               Ford                      Escape FWD HEV 40.5365          CVT
## 944              Honda                        Passport FWD 21.8669    Automatic
## 945               Audi                                 SQ5 19.9013    Automatic
## 946               Ford                          Escape AWD 25.9423    Automatic
## 947               Ford                          Escape AWD 28.0000    Automatic
## 948               Ford                      Escape AWD HEV 39.7753          CVT
## 949              Honda                        Passport AWD 20.8521    Automatic
## 950         Land Rover                         Evoque MHEV 22.8791    Automatic
## 951         Land Rover                Discovery Sport MHEV 21.0989    Automatic
## 952            Lincoln                         Corsair AWD 24.1114    Automatic
## 953      Mercedes-Benz                    AMG GLC43 4matic 20.5741    Automatic
## 954      Mercedes-Benz              AMG GLC43 4matic Coupe 20.5323    Automatic
## 955               Ford                      Expedition 2WD 19.0000    Automatic
## 956               Ford                  Expedition MAX 2WD 19.0000    Automatic
## 957            Lincoln                       Navigator 2WD 18.5915    Automatic
## 958            Lincoln                     Navigator L 2WD 17.7425    Automatic
## 959               Ford                      Expedition 4WD 18.7942    Automatic
## 960               Ford                  Expedition MAX 4WD 18.0000    Automatic
## 961            Lincoln                       Navigator 4WD 17.5802    Automatic
## 962      Mercedes-Benz                                G550 14.4689    Automatic
## 963            Hyundai                Ioniq Plug-in Hybrid 52.3231       Manual
## 964              Karma          Revero GT (21-inch wheels) 25.8000    Automatic
## 965             Subaru                Crosstrek Hybrid AWD 35.1000          CVT
## 966               Audi                          A6 quattro 24.2460       Manual
## 967               Audi                          A7 quattro 24.2460       Manual
## 968               Audi                          A6 Allroad 22.2239       Manual
## 969         Volkswagen                              Passat 26.9474    Automatic
## 970              Honda                            CR-V FWD 30.0000          CVT
## 971              Honda                            CR-V AWD 29.0000          CVT
## 972           Infiniti                            QX80 2WD 16.1339    Automatic
## 973           Infiniti                            QX80 4WD 15.3391    Automatic
## 974               Audi                                 RS3 22.5088       Manual
## 975               Audi                               TT RS 22.8005       Manual
## 976              Mazda                                   6 29.3866    Automatic
## 977              Mazda                                   6 26.4147    Automatic
## 978               Audi                                A8 L 20.5422    Automatic
## 979                Kia                                Niro 48.6987       Manual
## 980                Kia                             Niro FE 50.4101       Manual
## 981                Kia                        Niro Touring 43.1750       Manual
## 982               Audi                                  Q7 18.4822    Automatic
## 983               Audi                                  Q8 18.4822    Automatic
## 984        Lamborghini                         Huracan 2WD 15.0057       Manual
## 985        Lamborghini                  Huracan Spyder 2WD 15.0057       Manual
## 986              Lexus                          RC 300 AWD 21.0000    Automatic
## 987              Lexus                              RC 350 22.8206    Automatic
## 988              Lexus                          RC 350 AWD 21.0000    Automatic
## 989              Lexus                              RC 300 24.2954    Automatic
## 990  Roush Performance                     Stage 3 Mustang 15.2813       Manual
## 991              Dodge                      Challenger SRT 16.2222    Automatic
## 992              Dodge                      Challenger SRT 15.5775       Manual
## 993              Dodge             Challenger SRT Widebody 15.3673    Automatic
## 994              Dodge             Challenger SRT Widebody 15.5775       Manual
## 995              Dodge                          Challenger 22.7798    Automatic
## 996              Dodge                          Challenger 17.7608    Automatic
## 997              Dodge                          Challenger 16.9149       Manual
## 998              Dodge                 Challenger Widebody 17.7608    Automatic
## 999              Dodge                 Challenger Widebody 16.9149       Manual
## 1000             Dodge                      Challenger AWD 21.4055    Automatic
## 1001          Chrysler                                 300 22.7798    Automatic
## 1002          Chrysler                             300 AWD 21.4055    Automatic
## 1003             Dodge                Charger SRT Widebody 15.0483    Automatic
## 1004             Dodge                             Charger 22.7798    Automatic
## 1005             Dodge                             Charger 17.7608    Automatic
## 1006             Dodge                    Charger Widebody 17.7608    Automatic
## 1007             Dodge                         Charger AWD 21.4055    Automatic
## 1008            Nissan                         Rogue Sport 28.0000          CVT
## 1009            Nissan                     Rogue Sport AWD 27.0000          CVT
## 1010              Ford                          Ranger 2WD 23.3249    Automatic
## 1011              Ford                          Ranger 4WD 21.8642    Automatic
## 1012              Ford          Transit T150 Wagon 2WD FFV 16.6229    Automatic
## 1013              Ford          Transit T150 Wagon 4WD FFV 15.8294    Automatic
## 1014              Ford               Ranger 2WD Incomplete 18.3123    Automatic
## 1015         Chevrolet                          Blazer FWD 24.0681    Automatic
## 1016             Mazda                            CX-5 2WD 27.5485    Automatic
## 1017             Mazda                            CX-9 2WD 24.3926    Automatic
## 1018         Chevrolet                          Blazer AWD 23.2617    Automatic
## 1019             Mazda                            CX-5 4WD 26.2462    Automatic
## 1020             Mazda                            CX-5 4WD 23.8943    Automatic
## 1021             Mazda                            CX-9 4WD 22.6002    Automatic
## 1022               GMC                          Acadia FWD 24.0675    Automatic
## 1023               GMC                          Acadia AWD 23.2617    Automatic
## 1024     Mercedes-Benz                       GLE580 4matic 18.9016    Automatic
## 1025     Mercedes-Benz                       GLS580 4matic 18.1463    Automatic
## 1026               Kia                 Niro Plug-in Hybrid 46.4127       Manual
## 1027           Porsche                 Panamera 4 e-Hybrid 22.5030       Manual
## 1028           Porsche       Panamera 4 e-Hybrid Executive 22.5030       Manual
## 1029           Porsche              Panamera 4 e-Hybrid ST 22.5030       Manual
## 1030              Audi                          A5 quattro 26.8097       Manual
## 1031              Audi                A5 Cabriolet quattro 26.2623       Manual
## 1032              Audi                          A4 quattro 26.8097       Manual
## 1033             Mazda                            CX-3 2WD 30.6953    Automatic
## 1034             Mazda                            CX-3 4WD 29.1658    Automatic
## 1035              Audi                A5 Sportback quattro 26.8097       Manual
## 1036              Audi                  A4 allroad quattro 26.2623       Manual
## 1037          Cadillac                             XT5 FWD 24.2204    Automatic
## 1038             Mazda                           CX-30 2WD 28.3285    Automatic
## 1039             Mazda                           CX-30 2WD 27.8919    Automatic
## 1040          Cadillac                             XT5 AWD 23.1817    Automatic
## 1041               BMW                            I8 Coupe 27.1257    Automatic
## 1042               BMW                         I8 Roadster 27.1257    Automatic
## 1043              MINI           Cooper SE Countryman All4 29.4601    Automatic
## 1044        Land Rover                    Range Rover PHEV 19.2332    Automatic
## 1045        Land Rover              Range Rover Sport PHEV 19.2332    Automatic
## 1046           Lincoln                    Aviator PHEV AWD 22.8000    Automatic
## 1047          Cadillac                             CT6 AWD 17.0723    Automatic
## 1048            Toyota                          Highlander 22.8678    Automatic
## 1049            Toyota                          Highlander 23.7222    Automatic
## 1050            Toyota                      Highlander AWD 22.8128    Automatic
## 1051           Bentley                            Bentayga 13.6613    Automatic
## 1052              Ford                                  GT 14.1252       Manual
## 1053     Mercedes-Benz                              SLC300 26.6861    Automatic
## 1054 Roush Performance                     Stage 3 Mustang 14.2443    Automatic
## 1055             Dodge                          Challenger 19.0620    Automatic
## 1056             Dodge                          Challenger 17.9612       Manual
## 1057            Nissan                               Kicks 33.1184          CVT
## 1058          Chrysler                                 300 19.0620    Automatic
## 1059             Dodge                             Charger 19.0620    Automatic
## 1060     Mercedes-Benz                        Maybach S650 15.5665    Automatic
## 1061     Mercedes-Benz                             AMG S65 15.5264    Automatic
## 1062             Honda                                 Fit 36.4670          CVT
## 1063             Honda                                 Fit 33.0000          CVT
## 1064             Honda                                 Fit 31.0000       Manual
## 1065             Honda                       Ridgeline FWD 21.8873    Automatic
## 1066               Ram                    1500 Classic 2WD 19.7182    Automatic
## 1067               Ram                    1500 Classic 2WD 17.3529    Automatic
## 1068             Honda                       Ridgeline AWD 20.5738    Automatic
## 1069               Ram                    1500 Classic 4WD 18.7200    Automatic
## 1070               Ram                    1500 Classic 4WD 16.8732    Automatic
## 1071             Buick                       Encore GX FWD 30.7314          CVT
## 1072             Buick                       Encore GX AWD 27.5574    Automatic
## 1073              Jeep              Wrangler Unlimited 4WD 24.7211    Automatic
## 1074           Bentley          Continental GT Convertible 14.6616       Manual
## 1075           Porsche                         911 Carrera 20.0000       Manual
## 1076           Porsche               911 Carrera Cabriolet 19.6342       Manual
## 1077           Porsche                       911 Carrera S 20.4606       Manual
## 1078           Porsche             911 Carrera S Cabriolet 20.0000       Manual
## 1079           Porsche                       911 Carrera 4 20.0000       Manual
## 1080           Porsche             911 Carrera 4 Cabriolet 20.0000       Manual
## 1081           Porsche                      911 Carrera 4S 20.0000       Manual
## 1082           Porsche            911 Carrera 4S Cabriolet 20.1774       Manual
## 1083           Bentley                      Continental GT 14.9951       Manual
## 1084     Mercedes-Benz                    AMG CLA35 4matic 25.0870       Manual
## 1085           Bentley                         Flying Spur 14.6616       Manual
## 1086        Volkswagen                              Arteon 25.0272    Automatic
## 1087        Volkswagen                      Arteon 4motion 23.4414    Automatic
## 1088 Roush Performance                     F150 Pickup 2WD 12.6756    Automatic
## 1089 Roush Performance                     F150 Pickup 4WD 12.6756    Automatic
## 1090             Dodge                       Grand Caravan 19.9421    Automatic
## 1091             Mazda                           CX-30 4WD 26.4924    Automatic
## 1092             Mazda                           CX-30 4WD 27.4937    Automatic
## 1093        Land Rover              Range Rover Sport MHEV 20.8528    Automatic
## 1094        Volkswagen                               Jetta 34.0525    Automatic
## 1095        Volkswagen                               Jetta 34.0383       Manual
## 1096        Volkswagen                                Golf 31.5797    Automatic
## 1097        Volkswagen                                Golf 30.9111       Manual
## 1098           Lincoln                   Continental Coach 18.5922    Automatic
## 1099         Chevrolet             Silverado 4WD TrailBoss 17.2535    Automatic
## 1100         Chevrolet             Silverado 4WD TrailBoss 15.4336    Automatic
## 1101             Mazda                                MX-5 29.0373       Manual
## 1102             Mazda                                MX-5 29.7239    Automatic
## 1103               Kia                             Cadenza 23.3243    Automatic
## 1104             Buick                       Encore GX FWD 27.9466          CVT
## 1105             Dodge                             Journey 21.1431    Automatic
## 1106               BMW                                X5 M 15.0983    Automatic
## 1107               BMW                    X5 M Competition 15.0983    Automatic
## 1108               BMW                                X6 M 15.0983    Automatic
## 1109               BMW                    X6 M Competition 15.0983    Automatic
## 1110              Audi                                A8 L 23.0643    Automatic
## 1111              Audi                                  Q5 26.5403       Manual
## 1112             Mazda                                   2 34.0784       Manual
## 1113             Mazda                                   2 34.9351    Automatic
## 1114            Nissan                           Titan 2WD 18.1693    Automatic
## 1115            Nissan                           Titan 4WD 17.7971    Automatic
## 1116            Nissan                     Titan 4WD PRO4X 17.0000    Automatic
## 1117        Volkswagen                               Atlas 19.2110    Automatic
## 1118        Volkswagen                   Atlas Cross Sport 19.2110    Automatic
## 1119              Jeep                        Wrangler 4WD 21.4822    Automatic
## 1120        Volkswagen           Atlas Cross Sport 4motion 18.5818    Automatic
## 1121        Volkswagen                       Atlas 4motion 18.5818    Automatic
## 1122         Chevrolet                            Corvette 18.9269    Automatic
## 1123     Mercedes-Benz                      AMG A35 4matic 26.5615       Manual
## 1124               BMW              228i xDrive Gran Coupe 26.6667    Automatic
## 1125               BMW                        M235i xDrive 26.1835    Automatic
## 1126          Cadillac                                 CT4 23.6864    Automatic
## 1127          Cadillac                             CT4 AWD 23.1948    Automatic
## 1128          Cadillac                           CT4 V AWD 23.1948    Automatic
## 1129          Cadillac                             CT4 AWD 25.9339    Automatic
## 1130          Cadillac                                 CT4 27.1593    Automatic
## 1131          Cadillac                             CT5 AWD 20.5169    Automatic
## 1132          Cadillac                           CT5 V AWD 20.0000    Automatic
## 1133          Cadillac                                 CT5 21.4176    Automatic
## 1134          Cadillac                               CT5 V 21.4176    Automatic
## 1135            Nissan                              Sentra 33.1338          CVT
## 1136            Nissan                           Sentra SR 31.8639          CVT
## 1137           Hyundai                       Sonata Hybrid 47.2269       Manual
## 1138           Hyundai                  Sonata Hybrid Blue 51.8655       Manual
## 1139            Toyota                   Highlander Hybrid 35.8587          CVT
## 1140            Toyota               Highlander Hybrid AWD 34.9824          CVT
## 1141            Toyota      Highlander Hybrid AWD LTD/PLAT 35.0205          CVT
## 1142             Honda              Clarity Plug-in Hybrid 42.1053          CVT
## 1143          Cadillac                               CT4 V 23.0000    Automatic
## 1144           Porsche                       Cayenne Coupe 20.2475    Automatic
## 1145           Porsche                     Cayenne S Coupe 19.1192    Automatic
## 1146           Porsche                 Cayenne Turbo Coupe 16.5714    Automatic
## 1147             Honda                           Civic 5Dr 24.5785       Manual
## 1148             Honda                            CR-V AWD 38.1714          CVT
## 1149           Porsche                           Macan GTS 19.2975       Manual
## 1150           Porsche                         Macan Turbo 19.1612       Manual
## 1151              Audi                               RS Q8 15.2575    Automatic
## 1152            Toyota                     Camry AWD LE/SE 28.5883    Automatic
## 1153            Toyota                   Camry AWD XLE/XSE 28.0000    Automatic
## 1154              Ford                     F150 Pickup 2WD 24.2231    Automatic
## 1155              Ford       F150 2WD BASE PAYLOAD LT TIRE 24.2231    Automatic
## 1156              Ford              F150 Pickup 4WD XL/XLT 23.6588    Automatic
## 1157              Ford                     F150 Pickup 4WD 21.8271    Automatic
## 1158               BMW                        X3 xDrive30e 23.7073    Automatic
## 1159     Mercedes-Benz                               S560e 22.8263    Automatic
## 1160               BMW                         M2 CS Coupe 19.5165       Manual
## 1161               BMW                         M2 CS Coupe 18.7179       Manual
## 1162              Audi                                 SQ7 17.1357    Automatic
## 1163              Audi                                 SQ8 17.0474    Automatic
## 1164           Bentley                            Bentayga 18.7319    Automatic
##      gears  drive displ cylinders            class lv2 lv4 sidi  aspiration
## 1       10    FWD   2.0         4          Compact   0  13    Y     Natural
## 2        1    FWD   1.8         4          Compact   0  13    N     Natural
## 3        6    FWD   2.0         4          Compact   0  13    Y     Natural
## 4       10    FWD   2.0         4          Compact   0  13    Y     Natural
## 5        1    FWD   1.8         4          Compact   0  13    N     Natural
## 6        6    FWD   1.8         4          Compact   0  13    N     Natural
## 7        1    FWD   1.8         4          Compact   0  13    N     Natural
## 8        1    FWD   2.0         4      Sm St Wagon   0  24    N     Natural
## 9        1    FWD   2.0         4      Sm St Wagon   0  24    N     Natural
## 10       6    FWD   2.0         4      Sm St Wagon   0  24    N     Natural
## 11       7    FWD   1.6         4      Sm St Wagon   0  24    Y       Turbo
## 12       6    FWD   2.4         4           Sm SUV   0   0    Y     Natural
## 13       6    FWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 14       8    FWD   3.8         6           Sm SUV   0  21    Y     Natural
## 15       6    AWD   2.4         4           Sm SUV   0   0    Y     Natural
## 16       6    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 17       8    AWD   3.8         6           Sm SUV   0  21    Y     Natural
## 18       9    4WD   2.0         4           Sm SUV   0   0    Y       Turbo
## 19       9    4WD   3.0         6          Std SUV   0   0    Y       Turbo
## 20       8    RWD   2.0         4       Two Seater   0   0    Y       Turbo
## 21       8    RWD   2.0         4       Two Seater   0   0    Y       Turbo
## 22       8    RWD   2.0         4          Compact   0  10    Y       Turbo
## 23       8    RWD   2.0         4          Compact   0  10    Y       Turbo
## 24       8    AWD   2.0         4          Compact   0  10    Y       Turbo
## 25       8    AWD   2.0         4          Compact   0  10    Y       Turbo
## 26       8    RWD   2.0         4          Midsize   0  14    Y       Turbo
## 27       8    RWD   2.0         4          Midsize   0  14    Y       Turbo
## 28       8    AWD   2.0         4          Midsize   0  14    Y       Turbo
## 29       8    AWD   2.0         4          Midsize   0  14    Y       Turbo
## 30       8    AWD   2.0         4     Mid St Wagon   0  34    Y       Turbo
## 31       9    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 32       9    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 33       8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 34       8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 35       8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 36       8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 37       8    RWD   3.0         6       Two Seater   0   0    Y       Super
## 38       8    RWD   3.0         6       Two Seater   0   0    Y       Super
## 39       8    RWD   3.0         6       Two Seater   0   0    Y       Super
## 40       8    RWD   3.0         6       Two Seater   0   0    Y       Super
## 41       8    AWD   3.0         6       Two Seater   0   0    Y       Super
## 42       8    AWD   3.0         6       Two Seater   0   0    Y       Super
## 43       8    AWD   3.0         6          Midsize   0  14    Y       Super
## 44       8    AWD   3.0         6     Mid St Wagon   0  34    Y       Super
## 45       8    AWD   3.0         6           Sm SUV   0   0    Y       Super
## 46       8    AWD   5.0         8           Sm SUV   0   0    Y       Super
## 47       9    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 48       8    AWD   3.8         6          Std SUV   0   0    Y     Natural
## 49       8    RWD   2.0         4       Subcompact  10   0    Y       Turbo
## 50       1    FWD   2.0         4          Midsize   0  14    N     Natural
## 51       1    FWD   2.0         4          Midsize   0  14    N     Natural
## 52       8    RWD   3.0         6       Two Seater   0   0    Y       Turbo
## 53       6    RWD   2.0         4       Subcompact  10   0    Y       Turbo
## 54       8    AWD   2.0         4       Subcompact  10   0    Y       Turbo
## 55       8    RWD   2.0         4       Subcompact  10   0    Y       Turbo
## 56       8    AWD   2.0         4       Subcompact  10   0    Y       Turbo
## 57       8    RWD   3.0         6       Subcompact  10   0    Y       Turbo
## 58       6    RWD   3.0         6       Subcompact  10   0    Y       Turbo
## 59       8    AWD   3.0         6       Subcompact  10   0    Y       Turbo
## 60       8    RWD   3.0         6       Subcompact  10   0    Y       Turbo
## 61       6    RWD   3.0         6       Subcompact  10   0    Y       Turbo
## 62       8    AWD   3.0         6       Subcompact  10   0    Y       Turbo
## 63       8    RWD   3.0         6       Subcompact  10   0    Y       Turbo
## 64       6    RWD   3.0         6       Subcompact  10   0    Y       Turbo
## 65       8    AWD   3.0         6       Subcompact  10   0    Y       Turbo
## 66       6    RWD   3.0         6       Subcompact  10   0    Y       Turbo
## 67       7    RWD   3.0         6       Subcompact  10   0    Y       Turbo
## 68       8    RWD   2.0         4       Subcompact   9   0    Y       Turbo
## 69       8    AWD   2.0         4       Subcompact   9   0    Y       Turbo
## 70       8    RWD   3.0         6       Subcompact   9   0    Y       Turbo
## 71       8    AWD   3.0         6       Subcompact   9   0    Y       Turbo
## 72       6    RWD   3.0         6       Subcompact   9   0    Y       Turbo
## 73       7    RWD   3.0         6       Subcompact   9   0    Y       Turbo
## 74       6    RWD   3.0         6       Subcompact   9   0    Y       Turbo
## 75       7    RWD   3.0         6       Subcompact   9   0    Y       Turbo
## 76       8    FWD   2.4         4          Compact   0  14    Y     Natural
## 77       8    FWD   2.4         4          Compact   0  14    Y     Natural
## 78       9    FWD   3.5         6          Compact   0  14    Y     Natural
## 79       9    FWD   3.5         6          Compact   0  14    Y     Natural
## 80       9    AWD   3.5         6          Compact   0  14    Y     Natural
## 81       9    AWD   3.5         6          Compact   0  14    Y     Natural
## 82       8    RWD   2.0         4          Compact   0  13    Y       Turbo
## 83       8    AWD   3.0         6          Compact   0  13    Y       Turbo
## 84       8    RWD   3.0         6          Compact   0  13    Y       Turbo
## 85       8    RWD   2.0         4          Compact  11   0    Y       Turbo
## 86       6    RWD   2.0         4          Compact  11   0    Y       Turbo
## 87       8    AWD   2.0         4          Compact  11   0    Y       Turbo
## 88       8    RWD   2.0         4          Compact   0  12    Y       Turbo
## 89       8    AWD   2.0         4          Compact   0  12    Y       Turbo
## 90       8    RWD   3.0         6          Compact  11   0    Y       Turbo
## 91       6    RWD   3.0         6          Compact  11   0    Y       Turbo
## 92       8    AWD   3.0         6          Compact  11   0    Y       Turbo
## 93       6    AWD   3.0         6          Compact  11   0    Y       Turbo
## 94       8    RWD   3.0         6          Compact   0  12    Y       Turbo
## 95       8    AWD   3.0         6          Compact   0  12    Y       Turbo
## 96       6    RWD   3.0         6          Compact  11   0    Y       Turbo
## 97       7    RWD   3.0         6          Compact  11   0    Y       Turbo
## 98       7    RWD   3.0         6          Compact  11   0    Y       Turbo
## 99       6    RWD   3.0         6          Compact  11   0    Y       Turbo
## 100      7    RWD   3.0         6          Compact  11   0    Y       Turbo
## 101      8    RWD   3.0         6            Large   0  14    Y       Turbo
## 102      8    AWD   3.0         6            Large   0  14    Y       Turbo
## 103      8    AWD   4.4         8            Large   0  14    Y       Turbo
## 104      8    AWD   6.6        12            Large   0  14    Y       Turbo
## 105      8    4WD   3.6         6 Std Pickup Truck   0   0    N     Natural
## 106      6    4WD   3.6         6 Std Pickup Truck   0   0    N     Natural
## 107      8    AWD   5.0         8           Sm SUV   0   0    Y       Super
## 108      8    AWD   3.0         6            Large   0  10    Y       Turbo
## 109      8    RWD   3.0         6       Two Seater   0   0    Y       Turbo
## 110      7    RWD   3.7         6       Two Seater   0   0    N     Natural
## 111      6    RWD   3.7         6       Two Seater   0   0    N     Natural
## 112      7    RWD   3.7         6       Two Seater   0   0    N     Natural
## 113      8    AWD   2.0         4          Compact   0  10    Y       Turbo
## 114      8    RWD   2.0         4          Compact   0  10    Y       Turbo
## 115      6    RWD   2.0         4          Compact   0  10    Y       Turbo
## 116      6    FWD   2.0         4          Compact   0  20    N     Natural
## 117      6    FWD   2.0         4          Compact   0  20    N     Natural
## 118      7    FWD   1.6         4          Compact   0  20    Y       Turbo
## 119      6    FWD   1.6         4          Compact   0  20    Y       Turbo
## 120      6    FWD   2.0         4          Compact   0  20    Y       Turbo
## 121      8    FWD   3.5         6          Minivan   0   0    Y     Natural
## 122      8    AWD   3.5         6          Minivan   0   0    Y     Natural
## 123      8    FWD   3.8         6          Std SUV   0   0    Y     Natural
## 124      8    RWD   4.0         8       Two Seater   0   0    N       Turbo
## 125      7    AWD   5.2        10       Two Seater   0   0    Y     Natural
## 126      7    AWD   5.2        10       Two Seater   0   0    Y     Natural
## 127      7    AWD   5.2        10       Two Seater   0   0    Y     Natural
## 128      8    RWD   5.2        12      Minicompact   9   0    N       Turbo
## 129      8    RWD   4.0         8      Minicompact   9   0    N       Turbo
## 130      8    AWD   3.3         6          Compact   0  10    Y       Turbo
## 131      8    RWD   3.3         6          Compact   0  10    Y       Turbo
## 132      1    FWD   1.6         4          Compact   0  14    N     Natural
## 133      6    FWD   1.6         4          Compact   0  14    N     Natural
## 134      7    FWD   1.4         4          Midsize   0  14    Y       Turbo
## 135      8    FWD   3.3         6          Minivan   0   0    Y     Natural
## 136     10    FWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 137     10    FWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 138     10    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 139     10    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 140      7    AWD   1.6         4           Sm SUV   0   0    Y       Turbo
## 141      7    FWD   1.6         4           Sm SUV   0   0    Y       Turbo
## 142      6    FWD   2.0         4           Sm SUV   0   0    Y     Natural
## 143      6    AWD   2.0         4           Sm SUV   0   0    Y     Natural
## 144      9    FWD   2.0         4          Midsize   0  13    Y       Turbo
## 145      8    AWD   3.8         6            Large   0  15    Y     Natural
## 146      8    RWD   3.8         6            Large   0  15    Y     Natural
## 147      9    FWD   3.6         6           Sm SUV   0   0    Y     Natural
## 148      9    AWD   3.6         6           Sm SUV   0   0    Y     Natural
## 149      8    AWD   2.4         4          Std SUV   0   0    Y       Turbo
## 150      8    AWD   2.4         4          Std SUV   0   0    Y       Turbo
## 151      6    FWD   2.0         4          Midsize   0  14    N     Natural
## 152      6    FWD   2.0         4          Midsize   0  13    Y     Natural
## 153      6    FWD   2.5         4           Sm SUV   0  26    Y     Natural
## 154      6    FWD   2.0         4           Sm SUV   0   0    Y     Natural
## 155      6    FWD   2.4         4           Sm SUV   0   0    Y     Natural
## 156      6    AWD   2.5         4           Sm SUV   0   0    Y     Natural
## 157      9    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 158      6    AWD   2.0         4           Sm SUV   0   0    Y     Natural
## 159      6    AWD   2.4         4           Sm SUV   0   0    Y     Natural
## 160      8    AWD   3.0         6           Sm SUV   0   0    Y       Super
## 161      8    AWD   3.0         6           Sm SUV   0   0    Y       Super
## 162      8    AWD   5.0         8       Two Seater   0   0    Y       Super
## 163      8    AWD   5.0         8       Two Seater   0   0    Y       Super
## 164      1    FWD   1.4         4       Subcompact   0  11    N     Natural
## 165      5    FWD   1.4         4       Subcompact   0  11    N     Natural
## 166      5    FWD   1.4         4       Subcompact   0  11    N     Natural
## 167      1    FWD   1.4         4       Subcompact   0  11    N     Natural
## 168      8    RWD   5.0         8       Subcompact  10   0    Y     Natural
## 169      7    FWD   1.6         4          Midsize   0  14    Y       Turbo
## 170      7    FWD   1.6         4          Midsize   0  15    Y       Turbo
## 171      6    FWD   1.6         4          Midsize   0  15    Y       Turbo
## 172      1    FWD   2.0         4          Midsize   0  15    N     Natural
## 173      1    FWD   2.0         4          Midsize   0  15    N     Natural
## 174      6    FWD   2.0         4          Midsize   0  15    N     Natural
## 175      8    AWD   3.3         6            Large   0  15    Y       Turbo
## 176      8    RWD   3.3         6            Large   0  15    Y       Turbo
## 177      8    AWD   5.0         8            Large   0  15    Y     Natural
## 178      8    RWD   5.0         8            Large   0  15    Y     Natural
## 179      7    FWD   1.6         4            Large   0  16    Y       Turbo
## 180      6    FWD   2.0         4            Large   0  16    Y       Turbo
## 181      6    FWD   2.4         4            Large   0  16    Y     Natural
## 182      6    FWD   2.4         4            Large   0  16    Y     Natural
## 183      8    FWD   2.0         4              SPV   0   0    Y     Natural
## 184      8    FWD   2.0         4              SPV   0   0    Y     Natural
## 185      8    FWD   2.0         4              SPV   0   0    Y     Natural
## 186      8    FWD   2.0         4              SPV   0   0    Y     Natural
## 187      6    FWD   2.5         4              SPV   0   0    N     Natural
## 188      6    FWD   2.5         4              SPV   0   0    N     Natural
## 189      6    FWD   2.5         4              SPV   0   0    N     Natural
## 190      8    FWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 191      8    FWD   2.4         4           Sm SUV   0   0    Y     Natural
## 192      6    FWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 193      6    FWD   2.4         4           Sm SUV   0   0    N     Natural
## 194      8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 195      8    AWD   2.4         4           Sm SUV   0   0    Y     Natural
## 196      6    AWD   2.5         4           Sm SUV   0   0    N     Natural
## 197      6    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 198      6    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 199      6    4WD   2.4         4           Sm SUV   0   0    N     Natural
## 200     10    RWD   2.3         4          Std SUV   0   0    Y       Turbo
## 201     10    RWD   3.0         6          Std SUV   0   0    Y       Turbo
## 202     10 PT 4WD   2.3         4          Std SUV   0   0    Y       Turbo
## 203     10 PT 4WD   3.0         6          Std SUV   0   0    Y       Turbo
## 204      1    FWD   1.8         4          Midsize   0   0    N     Natural
## 205      8    FWD   2.0         4          Compact   0  12    Y       Turbo
## 206      8    AWD   2.0         4          Compact   0  12    Y Super+Turbo
## 207      1    FWD   1.5         4          Midsize   0  13    Y       Turbo
## 208      8    AWD   2.0         4          Midsize   0  14    Y Super+Turbo
## 209      8    FWD   2.0         4      Sm St Wagon   0  29    Y       Turbo
## 210      8    AWD   2.0         4      Sm St Wagon   0  29    Y       Turbo
## 211      8    FWD   2.0         4     Mid St Wagon   0  34    Y       Turbo
## 212      8    AWD   2.0         4     Mid St Wagon   0  34    Y Super+Turbo
## 213      8    AWD   2.0         4     Mid St Wagon   0  34    Y Super+Turbo
## 214      6    FWD   1.5         4           Sm SUV   0  30    Y       Turbo
## 215      9    FWD   2.0         4           Sm SUV   0  30    Y       Turbo
## 216      9    FWD   1.5         4           Sm SUV   0  13    Y       Turbo
## 217      8    FWD   2.0         4           Sm SUV   0  30    Y       Turbo
## 218      8    FWD   2.0         4           Sm SUV   0  25    Y       Turbo
## 219      6    AWD   1.5         4           Sm SUV   0  30    Y       Turbo
## 220      9    AWD   2.0         4           Sm SUV   0  30    Y       Turbo
## 221      9    AWD   1.5         4           Sm SUV   0  13    Y       Turbo
## 222      9    AWD   2.0         4           Sm SUV   0  13    Y       Turbo
## 223      8    AWD   2.0         4           Sm SUV   0  30    Y Super+Turbo
## 224      8    AWD   2.0         4           Sm SUV   0  25    Y       Turbo
## 225      8    FWD   2.0         4          Std SUV   0  47    Y       Turbo
## 226     10 PT 4WD   3.0         6          Std SUV   0   0    Y       Turbo
## 227     10 PT 4WD   3.3         6          Std SUV   0   0    Y     Natural
## 228      7    RWD   4.0         8       Two Seater   0   0    Y       Turbo
## 229      7    RWD   4.0         8       Two Seater   0   0    Y       Turbo
## 230      6    FWD   1.4         4           Sm SUV   0   0    N       Turbo
## 231      6    FWD   1.4         4           Sm SUV   0   0    N       Turbo
## 232      6    AWD   1.4         4           Sm SUV   0   0    N       Turbo
## 233      6    AWD   1.4         4           Sm SUV   0   0    N       Turbo
## 234      8    AWD   2.0         4           Sm SUV   0  30    Y       Turbo
## 235      9    FWD   3.6         6          Std SUV   0   0    Y     Natural
## 236      9    FWD   3.6         6          Std SUV   0   0    Y     Natural
## 237      9    AWD   3.6         6          Std SUV   0   0    Y     Natural
## 238      9    AWD   3.6         6          Std SUV   0   0    Y     Natural
## 239      8    AWD   2.0         4          Std SUV   0  47    Y       Turbo
## 240      6    FWD   2.0         4          Midsize   0  10    Y     Natural
## 241      6    RWD   1.8         4       Two Seater   0   0    Y       Turbo
## 242      6    FWD   1.4         4          Compact   0  12    N       Turbo
## 243      1    FWD   2.5         4          Midsize   0  15    Y     Natural
## 244      1    FWD   2.5         4          Midsize   0  15    Y     Natural
## 245      1    AWD   2.5         4          Midsize   0  15    Y     Natural
## 246      1    AWD   2.5         4          Midsize   0  15    Y     Natural
## 247      1    FWD   2.0         4          Midsize   0  15    Y       Turbo
## 248      8    AWD   3.3         6            Large   0  15    Y       Turbo
## 249      6    FWD   1.4         4      Sm St Wagon   0  19    N       Turbo
## 250      9    FWD   2.0         4           Sm SUV   0  22    Y       Turbo
## 251      7    FWD   3.5         6           Sm SUV   0   0    Y     Natural
## 252      9    AWD   2.0         4           Sm SUV   0  22    Y       Turbo
## 253      7    AWD   3.5         6           Sm SUV   0   0    Y     Natural
## 254      6    RWD   2.0         4      Minicompact   7   0    Y     Natural
## 255      6    RWD   2.0         4      Minicompact   7   0    Y     Natural
## 256      6    RWD   2.0         4      Minicompact   7   0    Y     Natural
## 257     10    RWD   6.2         8       Subcompact   9   0    Y       Super
## 258      6    RWD   6.2         8       Subcompact   9   0    Y       Super
## 259      6    RWD   2.0         4       Subcompact   9   0    Y       Turbo
## 260      8    RWD   2.0         4       Subcompact   9   0    Y       Turbo
## 261      6    RWD   3.6         6       Subcompact   9   0    Y     Natural
## 262     10    RWD   5.0         8       Subcompact   5   0    Y     Natural
## 263     10    RWD   3.5         6       Subcompact   5   0    Y     Natural
## 264      1    FWD   1.6         4          Compact   0  14    N     Natural
## 265      6    AWD   2.0         4          Compact   0  12    Y       Turbo
## 266      8    AWD   2.0         4          Compact   0  12    Y       Turbo
## 267      6    AWD   2.5         4          Compact   0  12    N       Turbo
## 268      9    FWD   2.0         4          Midsize   0   0    Y       Turbo
## 269      8    AWD   2.0         4          Midsize   0   0    Y       Turbo
## 270      6    FWD   2.0         4            Large   0   0    Y     Natural
## 271      7    FWD   1.6         4            Large   0   0    Y       Turbo
## 272      6    FWD   1.6         4            Large   0   0    Y       Turbo
## 273      8    AWD   2.5         4            Large   0  15    Y     Natural
## 274      8    AWD   2.4         4            Large   0  15    Y       Turbo
## 275      8    AWD   2.0         4      Sm St Wagon   0  19    Y       Turbo
## 276      8    FWD   1.5         4           Sm SUV   0   0    Y       Turbo
## 277      8    FWD   1.5         4           Sm SUV   0   0    Y       Turbo
## 278      8    4WD   1.5         4           Sm SUV   0   0    Y       Turbo
## 279      7    AWD   2.5         4           Sm SUV   0   0    Y     Natural
## 280      8    AWD   2.5         4           Sm SUV   0   0    Y     Natural
## 281      8    AWD   2.4         4           Sm SUV   0   0    Y       Turbo
## 282     10    RWD   6.2         8          Std SUV   0   0    Y     Natural
## 283      6    RWD   5.3         8          Std SUV   0   0    Y     Natural
## 284      6    RWD   5.3         8          Std SUV   0   0    Y     Natural
## 285      6    RWD   5.3         8          Std SUV   0   0    Y     Natural
## 286      6    RWD   5.3         8          Std SUV   0   0    Y     Natural
## 287     10    RWD   6.2         8          Std SUV   0   0    Y     Natural
## 288      6    RWD   5.3         8          Std SUV   0   0    Y     Natural
## 289      6    RWD   5.3         8          Std SUV   0   0    Y     Natural
## 290      6    RWD   5.3         8          Std SUV   0   0    Y     Natural
## 291      6    RWD   5.3         8          Std SUV   0   0    Y     Natural
## 292     10    RWD   6.2         8          Std SUV   0   0    Y     Natural
## 293     10    RWD   6.2         8          Std SUV   0   0    Y     Natural
## 294     10    4WD   6.2         8          Std SUV   0   0    Y     Natural
## 295      6    4WD   5.3         8          Std SUV   0   0    Y     Natural
## 296      6    4WD   5.3         8          Std SUV   0   0    Y     Natural
## 297      6    4WD   5.3         8          Std SUV   0   0    Y     Natural
## 298     10    4WD   6.2         8          Std SUV   0   0    Y     Natural
## 299      6    4WD   5.3         8          Std SUV   0   0    Y     Natural
## 300      6    4WD   5.3         8          Std SUV   0   0    Y     Natural
## 301     10    4WD   6.2         8          Std SUV   0   0    Y     Natural
## 302     10    4WD   6.2         8          Std SUV   0   0    Y     Natural
## 303      7    AWD   5.2        10       Two Seater   0   0    Y     Natural
## 304      7    RWD   4.0         8       Two Seater   0   0    Y       Turbo
## 305      8    AWD   2.0         4          Midsize   0  23    Y       Turbo
## 306      8    RWD   2.0         4          Midsize   0  23    Y       Turbo
## 307      8    AWD   3.3         6          Midsize   0  23    Y       Turbo
## 308      8    RWD   3.3         6          Midsize   0  23    Y       Turbo
## 309      6    FWD   2.4         4           Sm SUV   0   0    Y     Natural
## 310      8    FWD   3.3         6           Sm SUV   0   0    Y     Natural
## 311      6    AWD   2.4         4           Sm SUV   0   0    Y     Natural
## 312      8    AWD   3.3         6           Sm SUV   0   0    Y     Natural
## 313     10    RWD   6.2         8          Std SUV   0   0    Y     Natural
## 314     10    4WD   6.2         8          Std SUV   0   0    Y     Natural
## 315      8    AWD   4.0         8          Std SUV   0   0    Y       Turbo
## 316      6    RWD   1.4         4       Two Seater   0   0    N       Turbo
## 317      6    RWD   1.4         4       Two Seater   0   0    N       Turbo
## 318      6    AWD   3.8         6       Subcompact   9   0    N       Turbo
## 319      8    AWD   3.3         6            Large   0  15    Y       Turbo
## 320      8    RWD   3.3         6            Large   0  15    Y       Turbo
## 321      8    AWD   5.0         8            Large   0  15    Y     Natural
## 322      8    RWD   5.0         8            Large   0  15    Y     Natural
## 323      6    RWD   2.8         4  Sm Pickup Truck   0   0    N       Turbo
## 324      8    RWD   3.6         6  Sm Pickup Truck   0   0    Y     Natural
## 325      6    RWD   2.5         4  Sm Pickup Truck   0   0    Y     Natural
## 326      6    RWD   2.8         4  Sm Pickup Truck   0   0    N       Turbo
## 327      8    RWD   3.6         6  Sm Pickup Truck   0   0    Y     Natural
## 328      6    RWD   2.5         4  Sm Pickup Truck   0   0    Y     Natural
## 329      6    4WD   2.8         4  Sm Pickup Truck   0   0    N       Turbo
## 330      6    4WD   2.8         4  Sm Pickup Truck   0   0    N       Turbo
## 331      8    4WD   3.6         6  Sm Pickup Truck   0   0    Y     Natural
## 332      8    4WD   3.6         6  Sm Pickup Truck   0   0    Y     Natural
## 333      6    4WD   2.5         4  Sm Pickup Truck   0   0    Y     Natural
## 334      6    4WD   2.8         4  Sm Pickup Truck   0   0    N       Turbo
## 335      8    4WD   3.6         6  Sm Pickup Truck   0   0    Y     Natural
## 336      6    4WD   2.5         4  Sm Pickup Truck   0   0    Y     Natural
## 337      6    FWD   1.0         3           Sm SUV   0   0    Y       Turbo
## 338      6    AWD   2.0         4           Sm SUV   0   0    Y     Natural
## 339      6    4WD   5.3         8          Std SUV   0   0    Y     Natural
## 340      6    4WD   5.3         8          Std SUV   0   0    Y     Natural
## 341      6    4WD   5.3         8          Std SUV   0   0    Y     Natural
## 342      8    4WD   3.0         6          Std SUV   0   0    N       Turbo
## 343      8    4WD   3.0         6          Std SUV   0   0    N       Turbo
## 344      8    4WD   5.0         8          Std SUV   0   0    Y       Super
## 345      8    4WD   5.0         8          Std SUV   0   0    Y       Super
## 346      8    4WD   5.0         8          Std SUV   0   0    Y       Super
## 347      8    4WD   5.0         8          Std SUV   0   0    Y       Super
## 348      8    4WD   5.0         8          Std SUV   0   0    Y       Super
## 349      8    4WD   5.0         8          Std SUV   0   0    Y       Super
## 350      8    RWD   2.0         4       Two Seater   0   0    Y       Turbo
## 351      8    AWD   5.0         8       Two Seater   0   0    Y       Super
## 352      8    AWD   5.0         8       Two Seater   0   0    Y       Super
## 353     10    RWD   3.6         6       Subcompact   9   0    Y     Natural
## 354     10    RWD   6.2         8       Subcompact   9   0    Y     Natural
## 355      9    AWD   3.6         6          Midsize   0   0    Y     Natural
## 356      1    FWD   1.5         4          Midsize   0  15    N     Natural
## 357      1    FWD   1.5         4          Midsize   0  15    N     Natural
## 358      6    FWD   1.4         4      Sm St Wagon   0  22    N       Turbo
## 359      8    RWD   3.6         6 Std Pickup Truck   0   0    N     Natural
## 360      8    RWD   5.7         8 Std Pickup Truck   0   0    N     Natural
## 361      8    RWD   5.7         8 Std Pickup Truck   0   0    N     Natural
## 362      8    RWD   3.6         6 Std Pickup Truck   0   0    N     Natural
## 363      8    4WD   3.6         6 Std Pickup Truck   0   0    N     Natural
## 364      8    4WD   5.7         8 Std Pickup Truck   0   0    N     Natural
## 365      8    4WD   5.7         8 Std Pickup Truck   0   0    N     Natural
## 366      9    FWD   3.5         6           Sm SUV   0   0    Y     Natural
## 367      9    FWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 368      9    FWD   2.4         4           Sm SUV   0   0    N     Natural
## 369      9    FWD   3.2         6           Sm SUV   0   0    N     Natural
## 370      1    FWD   2.5         4           Sm SUV   0   0    N     Natural
## 371      9    AWD   3.5         6           Sm SUV   0   0    Y     Natural
## 372      9    AWD   3.5         6           Sm SUV   0   0    Y     Natural
## 373      7    AWD   3.0         6           Sm SUV   0   0    N     Natural
## 374      8    AWD   3.0         6           Sm SUV   0   0    Y       Turbo
## 375      8    AWD   3.0         6           Sm SUV   0   0    Y       Turbo
## 376      8    AWD   3.0         6           Sm SUV   0   0    Y       Turbo
## 377      8    AWD   3.0         6           Sm SUV   0   0    Y       Turbo
## 378      9    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 379      9    AWD   2.4         4           Sm SUV   0   0    N     Natural
## 380      9    AWD   3.2         6           Sm SUV   0   0    N     Natural
## 381      9    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 382      9    AWD   3.2         6           Sm SUV   0   0    N     Natural
## 383      9    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 384      9    AWD   3.2         6           Sm SUV   0   0    N     Natural
## 385      8    4WD   3.6         6           Sm SUV   0   0    N     Natural
## 386      8    4WD   3.6         6           Sm SUV   0   0    N     Natural
## 387      8    4WD   3.6         6           Sm SUV   0   0    N     Natural
## 388      9    RWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 389      1    AWD   2.5         4           Sm SUV   0   0    N     Natural
## 390      7    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 391      7    AWD   3.0         6           Sm SUV   0   0    Y       Turbo
## 392      8    AWD   3.0         6          Std SUV   0   0    Y       Turbo
## 393      8    AWD   2.9         6          Std SUV   0   0    Y       Turbo
## 394      7    RWD   3.9         8       Two Seater   0   0    Y       Turbo
## 395      7    RWD   3.9         8       Two Seater   0   0    Y       Turbo
## 396      7    RWD   6.5        12       Two Seater   0   0    Y     Natural
## 397      7 PT 4WD   6.3        12      Minicompact   0   0    Y     Natural
## 398      7    RWD   3.9         8      Minicompact   0   0    Y       Turbo
## 399      7    RWD   3.9         8      Minicompact   5   0    Y       Turbo
## 400      7    FWD   1.5         3      Minicompact   5   0    Y       Turbo
## 401      7    FWD   2.0         4      Minicompact   5   0    Y       Turbo
## 402      8    FWD   2.0         4      Minicompact   5   0    Y       Turbo
## 403      8    RWD   3.0         6       Subcompact  15   0    Y       Turbo
## 404      8    AWD   3.0         6       Subcompact  15   0    Y       Turbo
## 405      8    RWD   3.0         6       Subcompact  12   0    Y       Turbo
## 406      8    AWD   3.0         6       Subcompact  12   0    Y       Turbo
## 407      8    AWD   4.4         8       Subcompact  15   0    Y       Turbo
## 408      8    AWD   4.4         8       Subcompact  12   0    Y       Turbo
## 409      9    4WD   3.0         6       Subcompact  10   0    Y     Natural
## 410      9    4WD   3.0         6       Subcompact   9   0    Y     Natural
## 411      7    FWD   1.5         3       Subcompact   0   0    Y       Turbo
## 412      7    FWD   1.5         3       Subcompact   0   0    Y       Turbo
## 413      7    FWD   2.0         4       Subcompact   0   0    Y       Turbo
## 414      7    FWD   2.0         4       Subcompact   0   0    Y       Turbo
## 415      8    FWD   2.0         4       Subcompact   0   0    Y       Turbo
## 416      8    AWD   2.0         4          Compact   0  13    Y       Turbo
## 417      6    4WD   2.5         4          Compact   0  13    Y     Natural
## 418      9    4WD   3.0         6          Compact   0  12    Y     Natural
## 419      9    RWD   3.0         6          Compact   0  12    Y       Turbo
## 420      9    4WD   3.0         6          Compact   0  12    Y       Turbo
## 421      9    4WD   3.0         6          Compact   0  13    Y     Natural
## 422      1    FWD   1.5         4          Compact   0   0    N     Natural
## 423     10    FWD   3.5         6          Midsize   0  15    Y     Natural
## 424      7    AWD   3.5         6          Midsize   0  15    Y     Natural
## 425      8    AWD   2.0         4          Midsize   0   0    Y       Turbo
## 426      8    FWD   2.0         4          Midsize   0   0    Y       Turbo
## 427      8    AWD   2.0         4          Midsize   0   0    Y       Turbo
## 428      8    RWD   2.0         4          Midsize   0  14    Y       Turbo
## 429      8    AWD   2.0         4          Midsize   0  14    Y       Turbo
## 430      8    AWD   3.0         6          Midsize   0  14    Y       Turbo
## 431      8    AWD   4.4         8          Midsize   0  14    Y       Turbo
## 432      8    RWD   3.0         6          Midsize  16   0    Y       Turbo
## 433      8    AWD   3.0         6          Midsize  16   0    Y       Turbo
## 434      8    AWD   4.4         8          Midsize  16   0    Y       Turbo
## 435      8    AWD   3.0         6          Midsize   0  10    Y       Turbo
## 436      8    RWD   3.0         6          Midsize   0  10    Y       Turbo
## 437      8    RWD   3.0         6          Midsize   0  10    Y       Turbo
## 438      6    FWD   2.5         4          Midsize   0   0    Y     Natural
## 439      6    FWD   2.5         4          Midsize   0   0    Y     Natural
## 440      6    4WD   2.5         4          Midsize   0   0    Y     Natural
## 441      9    4WD   3.0         6          Midsize   0  13    Y     Natural
## 442      7    FWD   1.5         3          Midsize   0   0    Y       Turbo
## 443      8    AWD   1.5         3          Midsize   0   0    Y       Turbo
## 444      7    FWD   2.0         4          Midsize   0   0    Y       Turbo
## 445      8    AWD   2.0         4          Midsize   0   0    Y       Turbo
## 446      8    AWD   2.0         4          Midsize   0   0    Y       Turbo
## 447      8    AWD   2.0         4          Midsize   0   0    Y       Turbo
## 448      8    AWD   2.0         4            Large   0   0    Y       Turbo
## 449      8    AWD   4.4         8            Large   0  14    Y       Turbo
## 450     10    AWD   3.6         6            Large   0  15    Y     Natural
## 451      6    FWD   3.6         6            Large   0  19    Y     Natural
## 452      6    FWD   3.6         6            Large   0  19    Y     Natural
## 453      6    FWD   1.5         4            Large   0   0    Y       Turbo
## 454      1    FWD   1.5         4            Large   0   0    Y       Turbo
## 455      7    FWD   1.5         4            Large   0   0    Y       Turbo
## 456      8    RWD   3.8         8            Large   0  19    Y       Turbo
## 457      8    AWD   3.0         6            Large   0  19    Y       Turbo
## 458      8    RWD   3.0         6            Large   0  19    Y       Turbo
## 459      9    FWD   2.0         4           Sm SUV   0  13    Y       Turbo
## 460      9    FWD   3.5         6           Sm SUV   0   0    Y     Natural
## 461      6    FWD   3.5         6           Sm SUV   0   0    Y     Natural
## 462      6    FWD   2.0         4           Sm SUV   0   0    N     Natural
## 463      8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 464      8    AWD   3.0         6           Sm SUV   0   0    Y       Turbo
## 465      8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 466      8    AWD   3.0         6           Sm SUV   0   0    Y       Turbo
## 467      9    AWD   3.5         6           Sm SUV   0   0    Y     Natural
## 468      6    AWD   3.5         6           Sm SUV   0   0    Y     Natural
## 469      8    4WD   2.0         4           Sm SUV   0   0    Y       Turbo
## 470      6    4WD   3.6         6           Sm SUV   0   0    N     Natural
## 471      8    4WD   2.0         4           Sm SUV   0   0    Y       Turbo
## 472      8    4WD   2.0         4           Sm SUV   0   0    Y       Turbo
## 473      6    4WD   3.6         6           Sm SUV   0   0    N     Natural
## 474      6    4WD   2.0         4           Sm SUV   0   0    N     Natural
## 475      8    4WD   1.5         4           Sm SUV   0   0    Y       Turbo
## 476      8    RWD   3.6         6          Std SUV   0   0    N     Natural
## 477      8    RWD   5.7         8          Std SUV   0   0    N     Natural
## 478      8    RWD   3.6         6          Std SUV   0   0    N     Natural
## 479      8    AWD   4.4         8          Std SUV   0   0    Y       Turbo
## 480      8    AWD   4.4         8          Std SUV   0   0    Y       Turbo
## 481      8    AWD   4.4         8          Std SUV   0   0    Y       Turbo
## 482      8    AWD   4.4         8          Std SUV   0   0    Y       Turbo
## 483      8    AWD   4.4         8          Std SUV   0   0    Y       Turbo
## 484      8    AWD   3.6         6          Std SUV   0   0    N     Natural
## 485      8    AWD   5.7         8          Std SUV   0   0    N     Natural
## 486      8    AWD   3.6         6          Std SUV   0   0    N     Natural
## 487      8    AWD   5.7         8          Std SUV   0   0    N     Natural
## 488      8    4WD   3.0         6          Std SUV   0   0    Y       Super
## 489      8    4WD   3.0         6          Std SUV   0   0    N       Turbo
## 490      8    AWD   3.0         6          Std SUV   0   0    Y       Turbo
## 491      8    AWD   3.0         6          Std SUV   0   0    Y       Turbo
## 492      8    AWD   3.8         8          Std SUV   0   0    Y       Turbo
## 493      8    AWD   3.8         8          Std SUV   0   0    Y       Turbo
## 494      9    4WD   3.0         6          Std SUV   0   0    Y       Turbo
## 495      9    RWD   4.7         8       Two Seater   0   0    Y       Turbo
## 496      9    RWD   3.0         6       Two Seater   0   0    Y       Turbo
## 497      6    RWD   6.2         8       Subcompact   9   0    Y     Natural
## 498      9    RWD   3.0         6       Subcompact  10   0    Y       Turbo
## 499      9    4WD   3.0         6       Subcompact  10   0    Y       Turbo
## 500      9    RWD   3.0         6       Subcompact   9   0    Y       Turbo
## 501      9    4WD   3.0         6       Subcompact   9   0    Y       Turbo
## 502      1    FWD   1.6         4          Compact   0  15    N     Natural
## 503      5    FWD   1.6         4          Compact   0  15    N     Natural
## 504      6    FWD   1.5         4          Midsize   0  16    Y       Turbo
## 505      6    AWD   2.0         4          Midsize   0  16    Y       Turbo
## 506      6    FWD   2.0         4          Midsize   0  16    Y       Turbo
## 507      6    FWD   2.5         4          Midsize   0  16    N     Natural
## 508      1    FWD   2.0         4          Midsize   0  12    N     Natural
## 509      1    FWD   2.0         4          Midsize   0  16    N     Natural
## 510      6    AWD   2.0         4          Midsize   0  16    Y       Turbo
## 511      6    FWD   2.0         4          Midsize   0  16    Y       Turbo
## 512      6    AWD   3.0         6          Midsize   0  16    Y       Turbo
## 513      6    FWD   3.0         6          Midsize   0  16    Y       Turbo
## 514      1    FWD   2.0         4          Midsize   0  12    N     Natural
## 515      9    4WD   3.0         6          Midsize   0  13    Y       Turbo
## 516      7    FWD   2.0         4          Midsize   0   0    Y       Turbo
## 517      8    FWD   3.5         6          Midsize   0  16    Y     Natural
## 518      8    FWD   3.5         6          Midsize   0  16    Y     Natural
## 519      6    FWD   2.5         4          Midsize   0  16    Y     Natural
## 520      6    FWD   2.5         4          Midsize   0  16    Y     Natural
## 521      9    RWD   3.0         6            Large   0  12    Y       Turbo
## 522      9    4WD   3.0         6            Large   0  12    Y       Turbo
## 523      9    4WD   3.0         6     Mid St Wagon   0  35    Y       Turbo
## 524      6    RWD   4.3         6 Std Pickup Truck   0   0    Y     Natural
## 525      8    RWD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 526      8    RWD   2.7         4 Std Pickup Truck   0   0    Y       Turbo
## 527     10    RWD   3.0         6 Std Pickup Truck   0   0    N       Turbo
## 528      6    RWD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 529      6    RWD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 530      8    RWD   2.7         4 Std Pickup Truck   0   0    Y       Turbo
## 531      6    RWD   4.3         6 Std Pickup Truck   0   0    Y     Natural
## 532      8    RWD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 533      8    RWD   2.7         4 Std Pickup Truck   0   0    Y       Turbo
## 534     10    RWD   3.0         6 Std Pickup Truck   0   0    N       Turbo
## 535      6    RWD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 536      6    RWD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 537      8    RWD   2.7         4 Std Pickup Truck   0   0    Y       Turbo
## 538      6    FWD   5.7         8 Std Pickup Truck   0   0    N     Natural
## 539      8    4WD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 540      8    4WD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 541     10    4WD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 542     10    4WD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 543      8    4WD   2.7         4 Std Pickup Truck   0   0    Y       Turbo
## 544     10    4WD   6.2         8 Std Pickup Truck   0   0    Y     Natural
## 545      6    4WD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 546      6    4WD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 547      6    4WD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 548     10    4WD   3.0         6 Std Pickup Truck   0   0    N       Turbo
## 549      8    4WD   2.7         4 Std Pickup Truck   0   0    Y       Turbo
## 550      6    4WD   4.3         6 Std Pickup Truck   0   0    Y     Natural
## 551      6    4WD   4.3         6 Std Pickup Truck   0   0    Y     Natural
## 552      8    4WD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 553      8    4WD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 554     10    4WD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 555     10    4WD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 556      8    4WD   2.7         4 Std Pickup Truck   0   0    Y       Turbo
## 557     10    4WD   6.2         8 Std Pickup Truck   0   0    Y     Natural
## 558     10    4WD   6.2         8 Std Pickup Truck   0   0    Y     Natural
## 559      6    4WD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 560      6    4WD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 561     10    4WD   3.0         6 Std Pickup Truck   0   0    N       Turbo
## 562     10    4WD   3.0         6 Std Pickup Truck   0   0    N       Turbo
## 563      8    4WD   2.7         4 Std Pickup Truck   0   0    Y       Turbo
## 564      6    4WD   4.3         6 Std Pickup Truck   0   0    Y     Natural
## 565      6 PT 4WD   5.7         8 Std Pickup Truck   0   0    N     Natural
## 566      6    RWD   4.3         6              SPV   0   0    Y     Natural
## 567      6    4WD   5.3         8              SPV   0   0    Y     Natural
## 568      6    RWD   5.3         8              SPV   0   0    Y     Natural
## 569      6    4WD   4.3         6              SPV   0   0    Y     Natural
## 570      6    RWD   4.3         6              SPV   0   0    Y     Natural
## 571      6    4WD   4.3         6              SPV   0   0    Y     Natural
## 572      6    4WD   5.3         8              SPV   0   0    Y     Natural
## 573      6    RWD   5.3         8              SPV   0   0    Y     Natural
## 574      9    FWD   3.6         6           Sm SUV   0   0    Y     Natural
## 575      9    FWD   2.5         4           Sm SUV   0   0    Y     Natural
## 576      6    FWD   2.4         4           Sm SUV   0   0    N     Natural
## 577      6    FWD   2.4         4           Sm SUV   0   0    N     Natural
## 578      9    RWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 579      8    RWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 580      9    AWD   3.6         6           Sm SUV   0   0    Y     Natural
## 581      9    AWD   2.4         4           Sm SUV   0   0    N     Natural
## 582      6    AWD   2.4         4           Sm SUV   0   0    N     Natural
## 583      9    4WD   2.0         4           Sm SUV   0   0    Y       Turbo
## 584      9    FWD   2.5         4          Std SUV   0   0    Y     Natural
## 585      9    FWD   3.6         6          Std SUV   0   0    Y     Natural
## 586      6    RWD   5.7         8          Std SUV   0   0    N     Natural
## 587      9    AWD   3.6         6          Std SUV   0   0    Y     Natural
## 588      6 PT 4WD   5.7         8          Std SUV   0   0    N     Natural
## 589      7    RWD   3.9         8       Two Seater   0   0    Y       Turbo
## 590      6    FWD   1.5         4          Compact  12   0    Y       Turbo
## 591      6    FWD   2.5         4          Compact   0  13    Y     Natural
## 592      6    FWD   2.5         4          Compact   0  13    Y     Natural
## 593      9    4WD   2.0         4          Compact   0  13    Y       Turbo
## 594      9    RWD   2.0         4          Compact   0  13    Y       Turbo
## 595      6    FWD   1.5         4          Midsize   0  15    Y       Turbo
## 596      8    AWD   2.0         4          Midsize   0   0    Y       Turbo
## 597      8    RWD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 598      8    RWD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 599      9    FWD   3.6         6          Minivan   0   0    N     Natural
## 600      9    FWD   3.6         6          Minivan   0   0    N     Natural
## 601     10    FWD   3.5         6          Minivan   0   0    Y     Natural
## 602      9    4WD   2.0         4           Sm SUV   0   0    Y       Turbo
## 603      8    AWD   2.0         4          Std SUV   0  47    Y Super+Turbo
## 604      1    FWD   2.0         4          Midsize   0   8    N     Natural
## 605      1    FWD   2.0         4          Midsize   0   8    N     Natural
## 606      8    RWD   5.2        12      Minicompact   9   0    N       Turbo
## 607      6    RWD   3.5         6      Minicompact   6   6    N       Super
## 608      6    RWD   3.5         6      Minicompact   6   6    N       Super
## 609      6    RWD   3.5         6      Minicompact   6   6    N       Super
## 610      6    RWD   3.5         6      Minicompact   6   6    N       Super
## 611      9    RWD   2.0         4       Subcompact  10   0    Y       Turbo
## 612      9    4WD   2.0         4       Subcompact  10   0    Y       Turbo
## 613      9    4WD   2.0         4       Subcompact   9   0    Y       Turbo
## 614      9    RWD   2.0         4       Subcompact   9   0    Y       Turbo
## 615      8    FWD   3.5         6          Midsize   0  13    Y     Natural
## 616      8    FWD   3.5         6          Midsize   0  13    Y     Natural
## 617      6    FWD   2.5         4          Midsize   0  14    Y     Natural
## 618      7    FWD   3.5         6          Midsize   0  14    N     Natural
## 619      1    FWD   1.8         4          Midsize   0   0    N     Natural
## 620      1 PT 4WD   1.8         4          Midsize   0   0    N     Natural
## 621      1    FWD   1.8         4          Midsize   0   0    N     Natural
## 622      8    RWD   3.0         6            Large   0  16    Y       Turbo
## 623      8    4WD   3.0         6            Large   0  16    Y       Turbo
## 624      8    4WD   3.0         6            Large   0  16    Y       Turbo
## 625      8    4WD   3.0         6            Large   0  16    Y       Turbo
## 626      8    4WD   2.9         6            Large   0  16    Y       Turbo
## 627      8    4WD   2.9         6            Large   0  16    Y       Turbo
## 628      8    4WD   2.9         6            Large   0  16    Y       Turbo
## 629      8    AWD   4.0         8            Large   0  16    Y       Turbo
## 630      8    AWD   4.0         8            Large   0  16    Y       Turbo
## 631      8    AWD   4.0         8            Large   0  16    Y       Turbo
## 632      8    AWD   4.0         8            Large   0  16    Y       Turbo
## 633      8    AWD   4.0         8            Large   0  16    Y       Turbo
## 634      7    RWD   2.0         4              SPV   0   0    Y       Turbo
## 635      7    RWD   2.0         4              SPV   0   0    Y       Turbo
## 636      7    RWD   2.0         4              SPV   0   0    Y       Turbo
## 637      7    RWD   2.0         4              SPV   0   0    Y       Turbo
## 638      7    RWD   2.0         4              SPV   0   0    Y       Turbo
## 639      7    RWD   2.0         4              SPV   0   0    Y       Turbo
## 640      9    FWD   2.4         4              SPV   0   0    N     Natural
## 641     10    RWD   3.3         6          Std SUV   0   0    Y     Natural
## 642      5    RWD   4.0         6          Std SUV   0   0    N     Natural
## 643     10 PT 4WD   3.3         6          Std SUV   0   0    Y     Natural
## 644      5 PT 4WD   4.0         6          Std SUV   0   0    N     Natural
## 645      8    4WD   5.7         8          Std SUV   0   0    N     Natural
## 646      5    AWD   4.0         6          Std SUV   0   0    N     Natural
## 647      1    FWD   3.6         6          Minivan   0   0    N     Natural
## 648      8    AWD   2.0         4          Compact   0  12    Y     Natural
## 649      8    AWD   2.0         4          Midsize   0  14    Y     Natural
## 650      8    AWD   2.0         4      Sm St Wagon   0  29    Y     Natural
## 651      8    AWD   2.0         4           Sm SUV   0  30    Y     Natural
## 652      8    AWD   2.0         4          Std SUV   0  47    Y     Natural
## 653      6    RWD   2.0         4      Minicompact   7   0    Y     Natural
## 654      6    RWD   2.0         4      Minicompact   7   0    Y     Natural
## 655      6    RWD   2.3         4       Subcompact  13   0    Y       Turbo
## 656     10    RWD   2.3         4       Subcompact  13   0    Y       Turbo
## 657      6    RWD   5.0         8       Subcompact  13   0    N     Natural
## 658      6    RWD   2.3         4       Subcompact  11   0    Y       Turbo
## 659     10    RWD   2.3         4       Subcompact  11   0    Y       Turbo
## 660     10    RWD   5.0         8       Subcompact  13   0    N     Natural
## 661     10    RWD   5.0         8       Subcompact  11   0    N     Natural
## 662     10    RWD   2.3         4       Subcompact  12   0    Y       Turbo
## 663     10    RWD   2.3         4       Subcompact  12   0    Y       Turbo
## 664      6    RWD   2.3         4       Subcompact  12   0    Y       Turbo
## 665      6    RWD   2.3         4       Subcompact  12   0    Y       Turbo
## 666      9    RWD   2.0         4          Midsize   0  13    Y       Turbo
## 667      6    4WD   5.3         8 Std Pickup Truck   0   0    Y     Natural
## 668      9    FWD   3.6         6           Sm SUV   0   0    Y     Natural
## 669      8    FWD   3.5         6           Sm SUV   0   0    Y     Natural
## 670      8    FWD   3.5         6           Sm SUV   0   0    Y     Natural
## 671      9    AWD   3.6         6           Sm SUV   0   0    Y     Natural
## 672      9    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 673      8    AWD   3.5         6           Sm SUV   0   0    Y     Natural
## 674      8    AWD   3.5         6           Sm SUV   0   0    Y     Natural
## 675      8    AWD   6.4         8          Std SUV   0   0    N     Natural
## 676      8    AWD   6.4         8          Std SUV   0   0    N     Natural
## 677      8    AWD   6.2         8          Std SUV   0   0    N       Super
## 678      6    AWD   3.5         6          Std SUV   0   0    Y     Natural
## 679      6    AWD   3.5         6          Std SUV   0   0    Y     Natural
## 680      8    4WD   5.7         8          Std SUV   0   0    N     Natural
## 681      7    AWD   2.0         4       Two Seater   0   0    Y       Turbo
## 682      8    AWD   4.0         8      Minicompact   6   0    Y       Turbo
## 683      8    RWD   3.0         6      Minicompact   5   0    Y       Turbo
## 684      8    RWD   3.0         6      Minicompact   5   0    Y       Turbo
## 685      8    AWD   3.0         6      Minicompact   5   0    Y       Turbo
## 686      7    AWD   2.0         4       Subcompact   0  10    Y       Turbo
## 687      7    AWD   2.0         4       Subcompact  10   0    Y       Turbo
## 688      7    AWD   2.0         4       Subcompact  13   0    Y       Turbo
## 689      7    AWD   2.0         4       Subcompact   0  10    Y       Turbo
## 690      7    AWD   2.0         4       Subcompact  13   0    Y       Turbo
## 691      8    AWD   4.0         8       Subcompact  11   0    Y       Turbo
## 692      8    AWD   4.4         8       Subcompact  15   0    Y       Turbo
## 693      8    AWD   4.4         8       Subcompact  15   0    Y       Turbo
## 694      8    AWD   4.4         8       Subcompact  12   0    Y       Turbo
## 695      8    AWD   4.4         8       Subcompact  12   0    Y       Turbo
## 696      6    RWD   5.2         8       Subcompact  12   0    N     Natural
## 697      6    RWD   5.0         8       Subcompact  14   0    N     Natural
## 698      7    4WD   2.0         4          Compact   0  12    Y       Turbo
## 699      7    FWD   2.0         4          Compact   0  12    Y       Turbo
## 700      8    RWD   6.6        12          Compact   9   0    Y       Turbo
## 701      6    FWD   1.5         4          Compact   0  13    Y     Natural
## 702      6    FWD   1.5         4          Compact   0  13    Y     Natural
## 703      7    AWD   2.0         4          Midsize   0  14    Y       Turbo
## 704      8    AWD   4.4         8          Midsize   0  14    Y       Turbo
## 705      8    AWD   4.4         8          Midsize   0  14    Y       Turbo
## 706      8    AWD   4.4         8          Midsize  16   0    Y       Turbo
## 707      9    4WD   2.0         4          Midsize   0  13    Y       Turbo
## 708      8    RWD   6.6        12          Midsize  13   0    Y       Turbo
## 709      5    AWD   2.0         4          Midsize   0  12    Y     Natural
## 710      7    AWD   2.0         4          Midsize   0  12    Y     Natural
## 711      5    AWD   2.0         4          Midsize   0  12    Y     Natural
## 712      7    AWD   2.0         4          Midsize   0  12    Y     Natural
## 713      8    FWD   3.5         6          Midsize   0  14    Y     Natural
## 714      8    FWD   3.5         6          Midsize   0  14    Y     Natural
## 715      8    FWD   2.5         4          Midsize   0  14    Y     Natural
## 716      8    FWD   2.5         4          Midsize   0  14    Y     Natural
## 717      8    FWD   2.5         4          Midsize   0  14    Y     Natural
## 718      6    FWD   2.5         4          Midsize   0  15    Y     Natural
## 719      6    FWD   2.5         4          Midsize   0  15    Y     Natural
## 720      8    FWD   3.5         6          Midsize   0  14    Y     Natural
## 721      8    FWD   2.0         4            Large   0   0    Y       Turbo
## 722      8    RWD   6.7        12            Large   0  15    Y       Turbo
## 723      8    RWD   6.7        12            Large   0  15    Y       Turbo
## 724      8    RWD   6.6        12            Large   0  14    Y       Turbo
## 725      8    RWD   6.6        12            Large   0  14    Y       Turbo
## 726      5    AWD   2.0         4      Sm St Wagon   0  21    Y     Natural
## 727      7    AWD   2.0         4      Sm St Wagon   0  21    Y     Natural
## 728      5    AWD   2.0         4      Sm St Wagon   0  21    Y     Natural
## 729      7    AWD   2.0         4      Sm St Wagon   0  21    Y     Natural
## 730      8    AWD   6.7        12     Mid St Wagon   0  21    Y       Turbo
## 731      8    AWD   6.7        12     Mid St Wagon   0  21    Y       Turbo
## 732      6    RWD   3.5         6  Sm Pickup Truck   0   0    Y     Natural
## 733      6    RWD   2.7         4  Sm Pickup Truck   0   0    N     Natural
## 734      6 PT 4WD   3.5         6  Sm Pickup Truck   0   0    Y     Natural
## 735      6 PT 4WD   3.5         6  Sm Pickup Truck   0   0    Y     Natural
## 736      6 PT 4WD   2.7         4  Sm Pickup Truck   0   0    N     Natural
## 737      6 PT 4WD   3.5         6  Sm Pickup Truck   0   0    Y     Natural
## 738      9    FWD   1.3         4           Sm SUV   0   0    Y       Turbo
## 739      9    FWD   2.4         4           Sm SUV   0   0    N     Natural
## 740      8    FWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 741      7    FWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 742      6    FWD   2.4         4           Sm SUV   0   0    N     Natural
## 743      8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 744      9    AWD   1.3         4           Sm SUV   0   0    Y       Turbo
## 745      9    AWD   2.4         4           Sm SUV   0   0    N     Natural
## 746      9    AWD   1.3         4           Sm SUV   0   0    Y       Turbo
## 747      7    4WD   2.0         4           Sm SUV   0   0    Y       Turbo
## 748      6    4WD   3.0         6           Sm SUV   0   0    N     Natural
## 749      6    4WD   2.4         4           Sm SUV   0   0    N     Natural
## 750      6    AWD   2.0         4           Sm SUV   0   0    Y     Natural
## 751      8    AWD   2.0         4           Sm SUV   0   0    Y     Natural
## 752      7    RWD   5.6         8          Std SUV   0   0    Y     Natural
## 753      8    AWD   4.0         8          Std SUV   0   0    Y       Turbo
## 754      8    AWD   3.0         6          Std SUV   0   0    Y       Turbo
## 755      8    AWD   3.0         6          Std SUV   0   0    Y       Turbo
## 756      8    AWD   3.0         6          Std SUV   0   0    Y       Turbo
## 757      8    AWD   4.0         8          Std SUV   0   0    Y       Turbo
## 758      7    4WD   5.6         8          Std SUV   0   0    Y     Natural
## 759      7    RWD   4.0         8       Two Seater   0   0    Y       Turbo
## 760      8    AWD   3.0         6      Minicompact   5   0    Y       Turbo
## 761     10    FWD   2.0         4          Midsize   0  17    Y       Turbo
## 762      7    RWD   3.0         6          Midsize   0  14    Y       Turbo
## 763      7    AWD   3.0         6          Midsize   0  14    Y       Turbo
## 764      7    RWD   3.0         6          Midsize   0  14    Y       Turbo
## 765      7    AWD   3.0         6          Midsize   0  14    Y       Turbo
## 766      6    FWD   1.5         4            Large   0  17    Y       Turbo
## 767      1    FWD   1.5         4            Large   0  17    Y       Turbo
## 768      7    FWD   1.5         4            Large   0  17    Y       Turbo
## 769      6    FWD   2.0         4            Large   0  17    Y       Turbo
## 770     10    FWD   2.0         4            Large   0  17    Y       Turbo
## 771      8    FWD   1.5         3           Sm SUV   0   0    Y       Turbo
## 772      8    FWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 773      8    FWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 774      1    FWD   3.5         6           Sm SUV   0   0    Y     Natural
## 775      8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 776      8    AWD   2.7         6           Sm SUV   0   0    Y       Turbo
## 777      8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 778      8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 779      8    AWD   2.7         6           Sm SUV   0   0    Y       Turbo
## 780      8    AWD   2.3         4           Sm SUV   0   0    Y       Turbo
## 781      1    4WD   3.5         6           Sm SUV   0   0    Y     Natural
## 782      1    4WD   3.5         6           Sm SUV   0   0    Y     Natural
## 783      8    RWD   3.0         6          Std SUV   0   0    Y       Turbo
## 784      8    RWD   3.0         6          Std SUV   0   0    Y       Turbo
## 785      7    RWD   4.0         8       Two Seater   0   0    Y       Turbo
## 786      7    RWD   4.0         8       Two Seater   0   0    Y       Turbo
## 787      6    FWD   2.0         4          Compact   0   0    Y     Natural
## 788     10    FWD   2.0         4          Compact   0   0    Y     Natural
## 789     10    FWD   2.0         4          Compact   0   0    Y     Natural
## 790      1    FWD   1.6         4          Midsize   0  19    N     Natural
## 791      6    FWD   1.6         4          Midsize   0  19    N     Natural
## 792      7    FWD   3.5         6     Mid St Wagon   0  28    N     Natural
## 793      7    AWD   3.5         6     Mid St Wagon   0  28    N     Natural
## 794      8    FWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 795      8    FWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 796      8    FWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 797      7    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 798      8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 799      8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 800      8    4WD   3.0         6          Std SUV   0   0    Y       Turbo
## 801      6    4WD   4.6         8          Std SUV   0   0    N     Natural
## 802      8    RWD   2.0         4          Compact   0  10    Y       Turbo
## 803      8    AWD   2.0         4          Compact   0  10    Y       Turbo
## 804      1    AWD   2.0         4           Sm SUV   0   0    N     Natural
## 805      8    AWD   4.0         8            Large   0  16    Y       Turbo
## 806      8    AWD   4.0         8            Large   0  16    Y       Turbo
## 807      8    AWD   4.0         8            Large   0  16    Y       Turbo
## 808      7    FWD   2.0         4       Subcompact   0  12    Y       Turbo
## 809      7    4WD   2.0         4       Subcompact   0   9    Y       Turbo
## 810      7    FWD   2.0         4       Subcompact   0   9    Y       Turbo
## 811      6    AWD   3.5         6          Compact   0  11    Y     Natural
## 812      8    RWD   3.5         6          Compact   0  11    Y     Natural
## 813      6    AWD   3.5         6          Compact   0  11    Y     Natural
## 814      8    RWD   5.0         8          Compact   0  14    Y     Natural
## 815      8    RWD   2.0         4          Compact   0  11    Y       Turbo
## 816      9    4WD   4.0         8          Compact   0  13    Y       Turbo
## 817      9    4WD   4.0         8          Compact   0  13    Y       Turbo
## 818      6    FWD   2.0         4          Compact   0   0    Y       Turbo
## 819      6    FWD   2.0         4          Compact   0  16    Y       Turbo
## 820      7    FWD   2.0         4          Compact   0  16    Y       Turbo
## 821      8    RWD   3.0         6          Midsize   0  14    Y       Turbo
## 822      8    RWD   3.5         6          Midsize   0  14    Y     Natural
## 823      6    AWD   3.5         6          Midsize   0  14    Y     Natural
## 824      8    RWD   3.5         6          Midsize   0  14    Y     Natural
## 825     10    RWD   3.5         6          Midsize  12   0    Y     Natural
## 826     10    AWD   3.5         6          Midsize  12   0    Y     Natural
## 827     10    RWD   3.4         6          Midsize  13   0    Y       Turbo
## 828     10    AWD   3.4         6          Midsize  13   0    Y       Turbo
## 829      1    FWD   2.0         4            Large   0  17    N     Natural
## 830      8    AWD   2.9         6           Sm SUV   0   0    Y       Turbo
## 831      7    RWD   5.2         8       Subcompact  12   0    N       Super
## 832      7    FWD   1.5         4          Compact  12   0    Y       Turbo
## 833      1    FWD   1.5         4          Compact  12   0    Y       Turbo
## 834      6    FWD   2.0         4          Compact  12   0    N     Natural
## 835      7    FWD   2.0         4          Compact  12   0    N     Natural
## 836      1    FWD   2.0         4          Compact  12   0    N     Natural
## 837      7    FWD   2.0         4          Compact   0   0    N     Natural
## 838      8    RWD   2.0         4          Midsize   0  12    Y       Turbo
## 839      8    AWD   2.0         4          Midsize   0  12    Y       Turbo
## 840      8    RWD   2.9         6          Midsize   0  12    Y       Turbo
## 841      7    FWD   1.5         4          Midsize   0  15    Y       Turbo
## 842      1    FWD   1.5         4          Midsize   0  15    Y       Turbo
## 843      6    FWD   2.0         4          Midsize   0  15    N     Natural
## 844      7    FWD   2.0         4          Midsize   0  15    N     Natural
## 845      1    FWD   2.0         4          Midsize   0  15    N     Natural
## 846      8    RWD   3.0         6 Std Pickup Truck   0   0    N       Turbo
## 847      8    4WD   3.0         6 Std Pickup Truck   0   0    N       Turbo
## 848      1    FWD   2.0         4              SPV   0   0    N     Natural
## 849      8    RWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 850      8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 851      8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 852      7    AWD   6.5        12       Two Seater   0   0    N     Natural
## 853      7    AWD   6.5        12       Two Seater   0   0    N     Natural
## 854      7    RWD   3.0         6       Subcompact   9   0    Y       Turbo
## 855      7    RWD   3.0         6       Subcompact   9   0    Y       Turbo
## 856      7    AWD   3.0         6       Subcompact   9   0    Y       Turbo
## 857      7    AWD   3.0         6       Subcompact   9   0    Y       Turbo
## 858      8    FWD   2.4         4          Compact   0  12    Y     Natural
## 859      6    RWD   3.3         6 Std Pickup Truck   0   0    N     Natural
## 860     10    RWD   5.0         8 Std Pickup Truck   0   0    Y     Natural
## 861     10    RWD   5.0         8 Std Pickup Truck   0   0    Y     Natural
## 862     10    RWD   5.0         8 Std Pickup Truck   0   0    Y     Natural
## 863     10    RWD   3.5         6 Std Pickup Truck   0   0    Y       Turbo
## 864     10    RWD   3.5         6 Std Pickup Truck   0   0    Y       Turbo
## 865     10    RWD   2.7         6 Std Pickup Truck   0   0    Y       Turbo
## 866      6 PT 4WD   3.3         6 Std Pickup Truck   0   0    N     Natural
## 867     10 PT 4WD   5.0         8 Std Pickup Truck   0   0    Y     Natural
## 868     10 PT 4WD   3.5         6 Std Pickup Truck   0   0    Y       Turbo
## 869     10 PT 4WD   3.5         6 Std Pickup Truck   0   0    Y       Turbo
## 870     10 PT 4WD   3.5         6 Std Pickup Truck   0   0    Y       Turbo
## 871     10 PT 4WD   2.7         6 Std Pickup Truck   0   0    Y       Turbo
## 872      8    FWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 873      8    FWD   2.5         4           Sm SUV   0   0    Y     Natural
## 874      8    FWD   2.5         4           Sm SUV   0   0    Y     Natural
## 875      8    FWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 876      8    4WD   2.0         4           Sm SUV   0   0    Y       Turbo
## 877      8    AWD   2.5         4           Sm SUV   0   0    Y     Natural
## 878      8    AWD   2.5         4           Sm SUV   0   0    Y     Natural
## 879      6    AWD   2.5         4           Sm SUV   0   0    Y     Natural
## 880      8    AWD   2.5         4           Sm SUV   0   0    Y     Natural
## 881      8    AWD   2.5         4           Sm SUV   0   0    Y     Natural
## 882      9    4WD   4.0         8          Std SUV   0   0    Y       Turbo
## 883      9    AWD   3.5         6       Two Seater   0   0    Y       Turbo
## 884      9    RWD   4.0         8       Subcompact   7   0    Y       Turbo
## 885      9    4WD   4.0         8       Subcompact   7   0    Y       Turbo
## 886      9    4WD   4.0         8          Compact  10   0    Y       Turbo
## 887      9    4WD   4.0         8          Compact  10   0    Y       Turbo
## 888      9    RWD   4.0         8            Large   0  12    Y       Turbo
## 889      9    4WD   4.0         8            Large   0  12    Y       Turbo
## 890      9    4WD   4.0         8            Large   0  12    Y       Turbo
## 891      9    4WD   4.0         8            Large   0  10    Y       Turbo
## 892      1    FWD   1.8         4      Sm St Wagon   0  24    N     Natural
## 893      7    FWD   1.8         4      Sm St Wagon   0  24    N     Natural
## 894      1    4WD   1.8         4      Sm St Wagon   0  23    N     Natural
## 895      7    4WD   1.8         4      Sm St Wagon   0  23    N     Natural
## 896      9    AWD   1.3         4           Sm SUV   0   0    Y       Turbo
## 897      9    4WD   4.0         8           Sm SUV   0   0    Y       Turbo
## 898      9    4WD   4.0         8           Sm SUV   0   0    Y       Turbo
## 899      9    4WD   4.0         8           Sm SUV   0   0    Y       Turbo
## 900      7    AWD   8.0        16       Two Seater   0   0    N       Turbo
## 901      9    RWD   3.0         6       Two Seater   0   0    Y       Turbo
## 902      8    AWD   3.0         6       Subcompact  13   0    Y       Turbo
## 903      8    AWD   3.0         6       Subcompact  10   0    Y       Turbo
## 904      9    RWD   4.0         8       Subcompact   9   0    Y       Turbo
## 905      9    RWD   4.0         8       Subcompact   9   0    Y       Turbo
## 906      9    RWD   4.0         8       Subcompact  11   0    Y       Turbo
## 907      9    RWD   4.0         8       Subcompact  11   0    Y       Turbo
## 908      9    4WD   3.0         6       Subcompact  10   0    Y       Turbo
## 909      9    4WD   3.0         6       Subcompact   9   0    Y       Turbo
## 910      7    FWD   2.0         4          Compact   0  13    Y       Turbo
## 911      8    AWD   3.0         6          Compact   0  13    Y       Turbo
## 912      6    FWD   2.0         4          Compact   0   0    Y     Natural
## 913      6    AWD   2.0         4          Compact   0   0    Y     Natural
## 914      9    RWD   4.0         8          Compact   0  13    Y       Turbo
## 915      9    RWD   4.0         8          Compact   0  13    Y       Turbo
## 916      9    4WD   3.0         6          Compact   0  13    Y       Turbo
## 917      5    FWD   1.2         3          Compact   0  17    N     Natural
## 918      1    FWD   1.2         3          Compact   0  17    N     Natural
## 919      5    FWD   1.2         3          Compact   0  12    N     Natural
## 920      1    FWD   1.2         3          Compact   0  12    N     Natural
## 921      7    FWD   2.0         4          Compact   0   0    Y       Turbo
## 922      8    AWD   3.0         6          Midsize  22   0    Y       Turbo
## 923      8    AWD   2.9         6          Midsize   0  16    Y       Turbo
## 924      8    AWD   2.9         6          Midsize   0   0    Y       Turbo
## 925      8    AWD   4.0         8            Large   0  13    Y       Turbo
## 926      8    AWD   4.4         8          Midsize  16   0    Y       Turbo
## 927     10    RWD   2.0         4          Midsize   0  13    Y       Turbo
## 928     10    AWD   2.0         4          Midsize   0  13    Y       Turbo
## 929     10    FWD   2.0         4          Midsize   0   0    Y     Natural
## 930      9    4WD   4.0         8          Midsize   0  13    Y       Turbo
## 931      8    AWD   4.0         8            Large   0  13    Y       Turbo
## 932      6    FWD   1.6         4            Large   0   0    Y     Natural
## 933      6    FWD   1.6         4            Large   0   0    Y     Natural
## 934      8    FWD   2.5         4            Large   0  16    Y     Natural
## 935      8    FWD   2.5         4            Large   0  16    Y     Natural
## 936      8    FWD   1.6         4            Large   0  16    Y       Turbo
## 937      6    AWD   2.7         6            Large   0  16    Y       Turbo
## 938      6    FWD   2.7         6            Large   0  16    Y       Turbo
## 939      6    AWD   3.0         6            Large   0  16    Y       Turbo
## 940      6    AWD   3.7         6            Large   0  16    N     Natural
## 941      6    FWD   3.7         6            Large   0  16    N     Natural
## 942      9    4WD   4.0         8     Mid St Wagon   0  35    Y       Turbo
## 943      1    FWD   2.5         4           Sm SUV   0   0    N     Natural
## 944      9    FWD   3.5         6           Sm SUV   0   0    Y     Natural
## 945      8    AWD   3.0         6           Sm SUV   0   0    Y       Turbo
## 946      8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 947      8    AWD   1.5         3           Sm SUV   0   0    Y       Turbo
## 948      1 PT 4WD   2.5         4           Sm SUV   0   0    N     Natural
## 949      9    AWD   3.5         6           Sm SUV   0   0    Y     Natural
## 950      9    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 951      9    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 952      8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 953      9    4WD   3.0         6           Sm SUV   0   0    Y       Turbo
## 954      9    4WD   3.0         6           Sm SUV   0   0    Y       Turbo
## 955     10    RWD   3.5         6          Std SUV   0   0    Y       Turbo
## 956     10    RWD   3.5         6          Std SUV   0   0    Y       Turbo
## 957     10    RWD   3.5         6          Std SUV   0   0    Y       Turbo
## 958     10    RWD   3.5         6          Std SUV   0   0    Y       Turbo
## 959     10 PT 4WD   3.5         6          Std SUV   0   0    Y       Turbo
## 960     10 PT 4WD   3.5         6          Std SUV   0   0    Y       Turbo
## 961     10 PT 4WD   3.5         6          Std SUV   0   0    Y       Turbo
## 962      9    4WD   4.0         8          Std SUV   0   0    Y       Turbo
## 963      6    FWD   1.6         4          Midsize   0   0    Y     Natural
## 964      1    RWD   1.5         3       Subcompact   0   5    Y       Turbo
## 965      1    AWD   2.0         4           Sm SUV   0   0    Y     Natural
## 966      7    AWD   3.0         6          Midsize   0  14    Y       Turbo
## 967      7    AWD   3.0         6          Midsize   0   0    Y       Turbo
## 968      7    AWD   3.0         6          Midsize   0  14    Y       Turbo
## 969      6    FWD   2.0         4          Midsize   0  16    Y       Turbo
## 970      1    FWD   1.5         4           Sm SUV   0   0    Y       Turbo
## 971      1    AWD   1.5         4           Sm SUV   0   0    Y       Turbo
## 972      7    RWD   5.6         8          Std SUV   0   0    Y     Natural
## 973      7    4WD   5.6         8          Std SUV   0   0    Y     Natural
## 974      7    AWD   2.5         5       Subcompact   0  10    Y       Turbo
## 975      7    AWD   2.5         5       Subcompact  13   0    Y       Turbo
## 976      6    FWD   2.5         4          Midsize   0  14    Y     Natural
## 977      6    FWD   2.5         4          Midsize   0  14    Y       Turbo
## 978      8    AWD   3.0         6            Large   0  13    Y       Turbo
## 979      6    FWD   1.6         4      Sm St Wagon   0  22    Y     Natural
## 980      6    FWD   1.6         4      Sm St Wagon   0  22    Y     Natural
## 981      6    FWD   1.6         4      Sm St Wagon   0  22    Y     Natural
## 982      8    AWD   3.0         6          Std SUV   0   0    Y       Turbo
## 983      8    AWD   3.0         6          Std SUV   0   0    Y       Turbo
## 984      7    RWD   5.2        10       Two Seater   0   0    Y     Natural
## 985      7    RWD   5.2        10       Two Seater   0   0    Y     Natural
## 986      6    AWD   3.5         6       Subcompact  10   0    Y     Natural
## 987      8    RWD   3.5         6       Subcompact  10   0    Y     Natural
## 988      6    AWD   3.5         6       Subcompact  10   0    Y     Natural
## 989      8    RWD   2.0         4       Subcompact  10   0    Y       Turbo
## 990      6    RWD   5.0         8       Subcompact  12   0    Y       Super
## 991      8    RWD   6.2         8          Midsize  16   0    N       Super
## 992      6    RWD   6.2         8          Midsize  16   0    N       Super
## 993      8    RWD   6.2         8          Midsize  16   0    N       Super
## 994      6    RWD   6.2         8          Midsize  16   0    N       Super
## 995      8    RWD   3.6         6          Midsize  16   0    N     Natural
## 996      8    RWD   6.4         8          Midsize  16   0    N     Natural
## 997      6    RWD   6.4         8          Midsize  16   0    N     Natural
## 998      8    RWD   6.4         8          Midsize  16   0    N     Natural
## 999      6    RWD   6.4         8          Midsize  16   0    N     Natural
## 1000     8    AWD   3.6         6          Midsize  16   0    N     Natural
## 1001     8    RWD   3.6         6            Large   0  16    N     Natural
## 1002     8    AWD   3.6         6            Large   0  16    N     Natural
## 1003     8    RWD   6.2         8            Large   0  17    N       Super
## 1004     8    RWD   3.6         6            Large   0  16    N     Natural
## 1005     8    RWD   6.4         8            Large   0  16    N     Natural
## 1006     8    RWD   6.4         8            Large   0  17    N     Natural
## 1007     8    AWD   3.6         6            Large   0  16    N     Natural
## 1008     8    FWD   2.0         4      Sm St Wagon   0  20    Y     Natural
## 1009     8    AWD   2.0         4      Sm St Wagon   0  20    Y     Natural
## 1010    10    RWD   2.3         4 Std Pickup Truck   0   0    Y       Turbo
## 1011    10 PT 4WD   2.3         4 Std Pickup Truck   0   0    Y       Turbo
## 1012    10    RWD   3.5         6    Passenger Van   0   0    Y     Natural
## 1013    10    AWD   3.5         6    Passenger Van   0   0    Y     Natural
## 1014    10    RWD   2.3         4              SPV   0   0    Y       Turbo
## 1015     9    FWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 1016     6    FWD   2.5         4           Sm SUV   0   0    Y     Natural
## 1017     6    FWD   2.5         4           Sm SUV   0   0    Y       Turbo
## 1018     9    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 1019     6    4WD   2.5         4           Sm SUV   0   0    Y     Natural
## 1020     6    4WD   2.5         4           Sm SUV   0   0    Y       Turbo
## 1021     6    4WD   2.5         4           Sm SUV   0   0    Y       Turbo
## 1022     9    FWD   2.0         4          Std SUV   0   0    Y       Turbo
## 1023     9    AWD   2.0         4          Std SUV   0   0    Y       Turbo
## 1024     9    4WD   4.0         8          Std SUV   0   0    Y       Turbo
## 1025     9    4WD   4.0         8          Std SUV   0   0    Y       Turbo
## 1026     6    FWD   1.6         4      Sm St Wagon   0  19    Y     Natural
## 1027     8    AWD   2.9         6            Large   0  16    Y       Turbo
## 1028     8    AWD   2.9         6            Large   0  16    Y       Turbo
## 1029     8    AWD   2.9         6            Large   0  16    Y       Turbo
## 1030     7    AWD   2.0         4       Subcompact  12   0    Y       Turbo
## 1031     7    AWD   2.0         4       Subcompact  10   0    Y       Turbo
## 1032     7    AWD   2.0         4          Compact   0  13    Y       Turbo
## 1033     6    FWD   2.0         4          Compact   0   0    Y     Natural
## 1034     6    4WD   2.0         4          Compact   0   0    Y     Natural
## 1035     7    AWD   2.0         4          Midsize   0   0    Y       Turbo
## 1036     7    AWD   2.0         4      Sm St Wagon   0  28    Y       Turbo
## 1037     9    FWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 1038     6    FWD   2.5         4           Sm SUV   0   0    Y     Natural
## 1039     6    FWD   2.5         4           Sm SUV   0   0    Y     Natural
## 1040     9    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 1041     6    AWD   1.5         3       Subcompact   5   0    Y       Turbo
## 1042     6    AWD   1.5         3       Two Seater   5   0    Y       Turbo
## 1043     6    AWD   1.5         3          Midsize   0   0    Y       Turbo
## 1044     8    4WD   2.0         4          Std SUV   0   0    Y       Turbo
## 1045     8    4WD   2.0         4          Std SUV   0   0    Y       Turbo
## 1046    10 PT 4WD   3.0         6          Std SUV   0   0    Y       Turbo
## 1047    10    AWD   4.2         8            Large   0  15    Y       Turbo
## 1048     8    FWD   3.5         6           Sm SUV   0   0    Y     Natural
## 1049     8    FWD   3.5         6           Sm SUV   0   0    Y     Natural
## 1050     8    AWD   3.5         6           Sm SUV   0   0    Y     Natural
## 1051     8    AWD   6.0        12          Std SUV   0   0    Y       Turbo
## 1052     7    RWD   3.5         6       Two Seater   0   0    Y       Turbo
## 1053     9    RWD   2.0         4       Two Seater   0   0    Y       Turbo
## 1054    10    RWD   5.0         8       Subcompact  12   0    Y       Super
## 1055     8    RWD   5.7         8          Midsize  16   0    N     Natural
## 1056     6    RWD   5.7         8          Midsize  16   0    N     Natural
## 1057     1    FWD   1.6         4          Midsize   0   0    N     Natural
## 1058     8    RWD   5.7         8            Large   0  16    N     Natural
## 1059     8    RWD   5.7         8            Large   0  16    N     Natural
## 1060     7    RWD   6.0        12            Large   0  10    N       Turbo
## 1061     7    RWD   6.0        12            Large   0  12    N       Turbo
## 1062     1    FWD   1.5         4      Sm St Wagon   0  17    Y     Natural
## 1063     7    FWD   1.5         4      Sm St Wagon   0  17    Y     Natural
## 1064     6    FWD   1.5         4      Sm St Wagon   0  17    Y     Natural
## 1065     9    FWD   3.5         6  Sm Pickup Truck   0   0    Y     Natural
## 1066     8    RWD   3.6         6 Std Pickup Truck   0   0    N     Natural
## 1067     8    RWD   5.7         8 Std Pickup Truck   0   0    N     Natural
## 1068     9    AWD   3.5         6 Std Pickup Truck   0   0    Y     Natural
## 1069     8    4WD   3.6         6 Std Pickup Truck   0   0    N     Natural
## 1070     8    4WD   5.7         8 Std Pickup Truck   0   0    N     Natural
## 1071     1    FWD   1.3         3           Sm SUV   0   0    Y       Turbo
## 1072     9    AWD   1.3         3           Sm SUV   0   0    Y       Turbo
## 1073     8    4WD   3.0         6           Sm SUV   0   0    N       Turbo
## 1074     8    AWD   6.0        12      Minicompact   6   0    Y       Turbo
## 1075     8    RWD   3.0         6      Minicompact   5   0    Y       Turbo
## 1076     8    RWD   3.0         6      Minicompact   5   0    Y       Turbo
## 1077     7    RWD   3.0         6      Minicompact   5   0    Y       Turbo
## 1078     7    RWD   3.0         6      Minicompact   5   0    Y       Turbo
## 1079     8    AWD   3.0         6      Minicompact   5   0    Y       Turbo
## 1080     8    AWD   3.0         6      Minicompact   5   0    Y       Turbo
## 1081     7    AWD   3.0         6      Minicompact   5   0    Y       Turbo
## 1082     7    AWD   3.0         6      Minicompact   5   0    Y       Turbo
## 1083     8    AWD   6.0        12       Subcompact  11   0    Y       Turbo
## 1084     7    4WD   2.0         4          Compact   0  12    Y       Turbo
## 1085     8    AWD   6.0        12          Midsize  13   0    Y       Turbo
## 1086     8    FWD   2.0         4      Sm St Wagon   0  27    Y       Turbo
## 1087     8    AWD   2.0         4      Sm St Wagon   0  27    Y       Turbo
## 1088    10    RWD   5.0         8 Std Pickup Truck   0   0    N       Super
## 1089    10    4WD   5.0         8 Std Pickup Truck   0   0    N       Super
## 1090     6    FWD   3.6         6          Minivan   0   0    N     Natural
## 1091     6    4WD   2.5         4           Sm SUV   0   0    Y     Natural
## 1092     6    4WD   2.5         4           Sm SUV   0   0    Y     Natural
## 1093     8    4WD   3.0         6          Std SUV   0   0    Y       Turbo
## 1094     8    FWD   1.4         4          Compact   0  16    Y       Turbo
## 1095     6    FWD   1.4         4          Compact   0  16    Y       Turbo
## 1096     8    FWD   1.4         4          Compact   0   0    Y       Turbo
## 1097     6    FWD   1.4         4          Compact   0   0    Y       Turbo
## 1098     6    AWD   3.0         6            Large   0  16    Y       Turbo
## 1099    10    4WD   6.2         8 Std Pickup Truck   0   0    Y     Natural
## 1100    10    4WD   6.2         8 Std Pickup Truck   0   0    Y     Natural
## 1101     6    RWD   2.0         4       Two Seater   0   0    Y     Natural
## 1102     6    RWD   2.0         4       Two Seater   0   0    Y     Natural
## 1103     8    FWD   3.3         6            Large   0  16    Y     Natural
## 1104     1    FWD   1.2         3           Sm SUV   0   0    Y       Turbo
## 1105     4    FWD   2.4         4           Sm SUV   0   0    N     Natural
## 1106     8    AWD   4.4         8          Std SUV   0   0    Y       Turbo
## 1107     8    AWD   4.4         8          Std SUV   0   0    Y       Turbo
## 1108     8    AWD   4.4         8          Std SUV   0   0    Y       Turbo
## 1109     8    AWD   4.4         8          Std SUV   0   0    Y       Turbo
## 1110     8    AWD   3.0         6            Large   0  13    Y       Turbo
## 1111     7    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 1112     6    FWD   1.5         4          Compact   0   0    Y     Natural
## 1113     6    FWD   1.5         4          Compact   0   0    Y     Natural
## 1114     9    RWD   5.6         8 Std Pickup Truck   0   0    Y     Natural
## 1115     9 PT 4WD   5.6         8 Std Pickup Truck   0   0    Y     Natural
## 1116     9 PT 4WD   5.6         8 Std Pickup Truck   0   0    Y     Natural
## 1117     8    FWD   3.6         6           Sm SUV   0   0    Y     Natural
## 1118     8    FWD   3.6         6           Sm SUV   0   0    Y     Natural
## 1119     8    4WD   2.0         4           Sm SUV   0   0    Y       Turbo
## 1120     8    AWD   3.6         6           Sm SUV   0   0    Y     Natural
## 1121     8    AWD   3.6         6          Std SUV   0   0    Y     Natural
## 1122     8    RWD   6.2         8       Two Seater   0   0    Y     Natural
## 1123     7    4WD   2.0         4       Subcompact   0   9    Y       Turbo
## 1124     8    AWD   2.0         4          Compact   0  12    Y       Turbo
## 1125     8    AWD   2.0         4          Compact   0  12    Y       Turbo
## 1126    10    RWD   2.7         4          Compact   0  13    Y       Turbo
## 1127    10    AWD   2.7         4          Compact   0  13    Y       Turbo
## 1128    10    AWD   2.7         4          Compact   0  13    Y       Turbo
## 1129     8    AWD   2.0         4          Compact   0  13    Y       Turbo
## 1130     8    RWD   2.0         4          Compact   0  13    Y       Turbo
## 1131    10    AWD   3.0         6          Midsize   0  13    Y       Turbo
## 1132    10    AWD   3.0         6          Midsize   0  13    Y       Turbo
## 1133    10    RWD   3.0         6          Midsize   0  13    Y       Turbo
## 1134    10    RWD   3.0         6          Midsize   0  13    Y       Turbo
## 1135     1    FWD   2.0         4          Midsize   0  14    Y     Natural
## 1136     1    FWD   2.0         4          Midsize   0  14    Y     Natural
## 1137     6    FWD   2.0         4            Large   0  16    Y     Natural
## 1138     6    FWD   2.0         4            Large   0  16    Y     Natural
## 1139     6    FWD   2.5         4           Sm SUV   0   0    Y     Natural
## 1140     6    AWD   2.5         4          Std SUV   0   0    Y     Natural
## 1141     6    AWD   2.5         4          Std SUV   0   0    Y     Natural
## 1142     1    FWD   1.5         4          Midsize   0  16    N     Natural
## 1143    10    RWD   2.7         4          Compact  13   0    Y       Turbo
## 1144     8    AWD   3.0         6          Std SUV   0   0    Y       Turbo
## 1145     8    AWD   2.9         6          Std SUV   0   0    Y       Turbo
## 1146     8    AWD   4.0         8          Std SUV   0   0    Y       Turbo
## 1147     6    FWD   2.0         4            Large   0   0    Y       Turbo
## 1148     1    AWD   2.0         4           Sm SUV   0   0    N     Natural
## 1149     7    AWD   2.9         6           Sm SUV   0   0    Y       Turbo
## 1150     7    AWD   2.9         6           Sm SUV   0   0    Y       Turbo
## 1151     8    AWD   4.0         8          Std SUV   0   0    Y       Turbo
## 1152     8    AWD   2.5         4          Midsize   0  15    Y     Natural
## 1153     8    AWD   2.5         4          Midsize   0  15    Y     Natural
## 1154    10    RWD   3.0         6 Std Pickup Truck   0   0    N       Turbo
## 1155    10    RWD   3.0         6 Std Pickup Truck   0   0    N       Turbo
## 1156    10 PT 4WD   3.0         6 Std Pickup Truck   0   0    N       Turbo
## 1157    10 PT 4WD   3.0         6 Std Pickup Truck   0   0    N       Turbo
## 1158     8    AWD   2.0         4           Sm SUV   0   0    Y       Turbo
## 1159     9    RWD   3.0         6            Large   0   9    Y       Turbo
## 1160     6    RWD   3.0         6       Subcompact  10   0    Y       Turbo
## 1161     7    RWD   3.0         6       Subcompact  10   0    Y       Turbo
## 1162     8    AWD   4.0         8          Std SUV   0   0    Y       Turbo
## 1163     8    AWD   4.0         8          Std SUV   0   0    Y       Turbo
## 1164     8    AWD   3.0         6          Std SUV   0   0    Y       Turbo
##              fuelType1        atvType startStop
## 1     Regular Gasoline           None         N
## 2     Regular Gasoline         Hybrid         Y
## 3     Regular Gasoline           None         N
## 4     Regular Gasoline           None         N
## 5     Regular Gasoline           None         N
## 6     Regular Gasoline           None         N
## 7     Regular Gasoline           None         N
## 8     Regular Gasoline           None         N
## 9     Regular Gasoline           None         N
## 10    Regular Gasoline           None         N
## 11    Regular Gasoline           None         N
## 12    Regular Gasoline           None         N
## 13    Regular Gasoline           None         N
## 14    Regular Gasoline           None         N
## 15    Regular Gasoline           None         N
## 16    Regular Gasoline           None         N
## 17    Regular Gasoline           None         N
## 18    Premium Gasoline           None         Y
## 19    Premium Gasoline         Hybrid         Y
## 20    Premium Gasoline           None         Y
## 21    Premium Gasoline           None         Y
## 22    Premium Gasoline           None         Y
## 23    Premium Gasoline           None         Y
## 24    Premium Gasoline           None         Y
## 25    Premium Gasoline           None         Y
## 26    Premium Gasoline           None         Y
## 27    Premium Gasoline           None         Y
## 28    Premium Gasoline           None         Y
## 29    Premium Gasoline           None         Y
## 30    Premium Gasoline           None         Y
## 31    Premium Gasoline           None         Y
## 32    Premium Gasoline           None         Y
## 33    Premium Gasoline           None         Y
## 34    Premium Gasoline           None         Y
## 35    Premium Gasoline           None         Y
## 36    Premium Gasoline           None         Y
## 37    Premium Gasoline           None         Y
## 38    Premium Gasoline           None         Y
## 39    Premium Gasoline           None         Y
## 40    Premium Gasoline           None         Y
## 41    Premium Gasoline           None         Y
## 42    Premium Gasoline           None         Y
## 43    Premium Gasoline           None         Y
## 44    Premium Gasoline           None         Y
## 45    Premium Gasoline           None         Y
## 46    Premium Gasoline           None         Y
## 47    Premium Gasoline           None         Y
## 48    Regular Gasoline           None         N
## 49    Premium Gasoline           None         Y
## 50    Regular Gasoline           None         N
## 51    Regular Gasoline           None         N
## 52    Premium Gasoline           None         Y
## 53    Premium Gasoline           None         Y
## 54    Premium Gasoline           None         Y
## 55    Premium Gasoline           None         Y
## 56    Premium Gasoline           None         Y
## 57    Premium Gasoline           None         Y
## 58    Premium Gasoline           None         Y
## 59    Premium Gasoline           None         Y
## 60    Premium Gasoline           None         Y
## 61    Premium Gasoline           None         Y
## 62    Premium Gasoline           None         Y
## 63    Premium Gasoline           None         Y
## 64    Premium Gasoline           None         Y
## 65    Premium Gasoline           None         Y
## 66    Premium Gasoline           None         Y
## 67    Premium Gasoline           None         Y
## 68    Premium Gasoline           None         Y
## 69    Premium Gasoline           None         Y
## 70    Premium Gasoline           None         Y
## 71    Premium Gasoline           None         Y
## 72    Premium Gasoline           None         Y
## 73    Premium Gasoline           None         Y
## 74    Premium Gasoline           None         Y
## 75    Premium Gasoline           None         Y
## 76    Premium Gasoline           None         N
## 77    Premium Gasoline           None         N
## 78    Premium Gasoline           None         N
## 79    Premium Gasoline           None         N
## 80    Premium Gasoline           None         Y
## 81    Premium Gasoline           None         Y
## 82    Premium Gasoline           None         Y
## 83    Premium Gasoline           None         Y
## 84    Premium Gasoline           None         Y
## 85    Premium Gasoline           None         Y
## 86    Premium Gasoline           None         Y
## 87    Premium Gasoline           None         Y
## 88    Premium Gasoline           None         Y
## 89    Premium Gasoline           None         Y
## 90    Premium Gasoline           None         Y
## 91    Premium Gasoline           None         Y
## 92    Premium Gasoline           None         Y
## 93    Premium Gasoline           None         Y
## 94    Premium Gasoline           None         Y
## 95    Premium Gasoline           None         Y
## 96    Premium Gasoline           None         Y
## 97    Premium Gasoline           None         Y
## 98    Premium Gasoline           None         Y
## 99    Premium Gasoline           None         Y
## 100   Premium Gasoline           None         Y
## 101   Premium Gasoline           None         Y
## 102   Premium Gasoline           None         Y
## 103   Premium Gasoline           None         Y
## 104   Premium Gasoline           None         Y
## 105   Regular Gasoline           None         Y
## 106   Regular Gasoline           None         Y
## 107   Premium Gasoline           None         Y
## 108   Premium Gasoline Plug-in Hybrid         Y
## 109   Premium Gasoline           None         Y
## 110   Premium Gasoline           None         N
## 111   Premium Gasoline           None         N
## 112   Premium Gasoline           None         N
## 113   Premium Gasoline           None         Y
## 114   Premium Gasoline           None         Y
## 115   Premium Gasoline           None         N
## 116   Regular Gasoline           None         N
## 117   Regular Gasoline           None         N
## 118   Regular Gasoline           None         N
## 119   Regular Gasoline           None         N
## 120   Regular Gasoline           None         N
## 121   Regular Gasoline           None         N
## 122   Regular Gasoline           None         N
## 123   Regular Gasoline           None         N
## 124   Premium Gasoline           None         N
## 125   Premium Gasoline           None         N
## 126   Premium Gasoline           None         N
## 127   Premium Gasoline           None         N
## 128   Premium Gasoline           None         N
## 129   Premium Gasoline           None         N
## 130   Premium Gasoline           None         N
## 131   Premium Gasoline           None         N
## 132   Regular Gasoline           None         N
## 133   Regular Gasoline           None         N
## 134   Regular Gasoline           None         N
## 135   Regular Gasoline           None         N
## 136   Premium Gasoline           None         Y
## 137   Premium Gasoline           None         Y
## 138   Premium Gasoline           None         Y
## 139   Premium Gasoline           None         Y
## 140   Regular Gasoline           None         N
## 141   Regular Gasoline           None         N
## 142   Regular Gasoline           None         N
## 143   Regular Gasoline           None         N
## 144   Premium Gasoline           None         N
## 145   Regular Gasoline           None         N
## 146   Regular Gasoline           None         N
## 147   Regular Gasoline           None         Y
## 148   Regular Gasoline           None         Y
## 149   Regular Gasoline           None         N
## 150   Regular Gasoline           None         N
## 151   Regular Gasoline           None         N
## 152   Regular Gasoline         Hybrid         Y
## 153   Regular Gasoline           None         Y
## 154   Regular Gasoline           None         N
## 155   Regular Gasoline           None         N
## 156   Regular Gasoline           None         Y
## 157   Premium Gasoline           None         Y
## 158   Regular Gasoline           None         N
## 159   Regular Gasoline           None         N
## 160   Premium Gasoline           None         Y
## 161   Premium Gasoline           None         Y
## 162   Premium Gasoline           None         Y
## 163   Premium Gasoline           None         Y
## 164   Regular Gasoline           None         N
## 165   Regular Gasoline           None         N
## 166   Regular Gasoline           None         N
## 167   Regular Gasoline           None         N
## 168   Premium Gasoline           None         N
## 169   Regular Gasoline           None         N
## 170   Regular Gasoline           None         N
## 171   Regular Gasoline           None         N
## 172   Regular Gasoline           None         N
## 173   Regular Gasoline           None         N
## 174   Regular Gasoline           None         N
## 175   Premium Gasoline           None         N
## 176   Premium Gasoline           None         N
## 177   Premium Gasoline           None         N
## 178   Premium Gasoline           None         N
## 179   Regular Gasoline           None         N
## 180   Regular Gasoline           None         N
## 181   Regular Gasoline           None         N
## 182   Regular Gasoline           None         N
## 183   Regular Gasoline           None         Y
## 184   Regular Gasoline            FFV         Y
## 185   Regular Gasoline           None         Y
## 186   Regular Gasoline            FFV         Y
## 187   Regular Gasoline           None         N
## 188   Regular Gasoline           None         N
## 189   Regular Gasoline           None         N
## 190   Regular Gasoline           None         Y
## 191   Regular Gasoline           None         Y
## 192   Premium Gasoline           None         N
## 193   Regular Gasoline           None         N
## 194   Regular Gasoline           None         Y
## 195   Regular Gasoline           None         Y
## 196   Regular Gasoline         Hybrid         Y
## 197   Premium Gasoline           None         N
## 198   Premium Gasoline           None         N
## 199   Regular Gasoline           None         N
## 200   Regular Gasoline           None         Y
## 201   Regular Gasoline           None         Y
## 202   Regular Gasoline           None         Y
## 203   Regular Gasoline           None         Y
## 204   Regular Gasoline Plug-in Hybrid         Y
## 205   Premium Gasoline           None         Y
## 206   Premium Gasoline           None         Y
## 207   Regular Gasoline           None         Y
## 208   Premium Gasoline           None         Y
## 209   Premium Gasoline           None         Y
## 210   Premium Gasoline           None         Y
## 211   Premium Gasoline           None         Y
## 212   Premium Gasoline           None         Y
## 213   Premium Gasoline           None         Y
## 214   Regular Gasoline           None         Y
## 215   Premium Gasoline           None         Y
## 216   Regular Gasoline           None         Y
## 217   Premium Gasoline           None         Y
## 218   Regular Gasoline           None         Y
## 219   Regular Gasoline           None         Y
## 220   Premium Gasoline           None         Y
## 221   Regular Gasoline           None         Y
## 222   Premium Gasoline           None         Y
## 223   Premium Gasoline           None         Y
## 224   Premium Gasoline           None         Y
## 225   Premium Gasoline           None         Y
## 226   Regular Gasoline           None         Y
## 227   Regular Gasoline            FFV         N
## 228   Premium Gasoline           None         Y
## 229   Premium Gasoline           None         Y
## 230   Regular Gasoline           None         N
## 231   Regular Gasoline           None         N
## 232   Regular Gasoline           None         N
## 233   Regular Gasoline           None         N
## 234   Premium Gasoline           None         Y
## 235   Regular Gasoline           None         Y
## 236   Regular Gasoline           None         Y
## 237   Regular Gasoline           None         Y
## 238   Regular Gasoline           None         Y
## 239   Premium Gasoline           None         Y
## 240   Regular Gasoline Plug-in Hybrid         Y
## 241   Premium Gasoline           None         N
## 242   Regular Gasoline           None         N
## 243   Regular Gasoline           None         N
## 244   Regular Gasoline           None         N
## 245   Regular Gasoline           None         N
## 246   Regular Gasoline           None         N
## 247   Regular Gasoline           None         N
## 248   Premium Gasoline           None         Y
## 249   Regular Gasoline           None         N
## 250   Premium Gasoline           None         Y
## 251   Premium Gasoline           None         N
## 252   Premium Gasoline           None         Y
## 253   Premium Gasoline           None         N
## 254   Premium Gasoline           None         N
## 255   Premium Gasoline           None         N
## 256   Premium Gasoline           None         N
## 257   Premium Gasoline           None         N
## 258   Premium Gasoline           None         N
## 259   Premium Gasoline           None         N
## 260   Premium Gasoline           None         N
## 261   Regular Gasoline           None         N
## 262   Premium Gasoline           None         N
## 263   Premium Gasoline         Hybrid         Y
## 264   Regular Gasoline           None         N
## 265   Premium Gasoline           None         N
## 266   Premium Gasoline           None         N
## 267   Premium Gasoline           None         N
## 268   Premium Gasoline           None         N
## 269   Premium Gasoline           None         N
## 270   Regular Gasoline           None         N
## 271   Regular Gasoline           None         N
## 272   Regular Gasoline           None         N
## 273   Regular Gasoline           None         N
## 274   Regular Gasoline           None         Y
## 275   Premium Gasoline           None         N
## 276   Regular Gasoline           None         N
## 277   Regular Gasoline           None         N
## 278   Regular Gasoline           None         N
## 279   Regular Gasoline           None         Y
## 280   Regular Gasoline           None         Y
## 281   Regular Gasoline           None         Y
## 282   Premium Gasoline           None         N
## 283   Regular Gasoline            FFV         N
## 284   Regular Gasoline            FFV         N
## 285   Regular Gasoline           None         N
## 286   Regular Gasoline           None         N
## 287   Premium Gasoline           None         N
## 288   Regular Gasoline            FFV         N
## 289   Regular Gasoline            FFV         N
## 290   Regular Gasoline           None         N
## 291   Regular Gasoline           None         N
## 292   Premium Gasoline           None         N
## 293   Premium Gasoline           None         N
## 294   Premium Gasoline           None         N
## 295   Regular Gasoline           None         N
## 296   Regular Gasoline            FFV         N
## 297   Regular Gasoline            FFV         N
## 298   Premium Gasoline           None         N
## 299   Regular Gasoline            FFV         N
## 300   Regular Gasoline            FFV         N
## 301   Premium Gasoline           None         N
## 302   Premium Gasoline           None         N
## 303   Premium Gasoline           None         N
## 304   Premium Gasoline           None         Y
## 305   Premium Gasoline           None         Y
## 306   Premium Gasoline           None         Y
## 307   Premium Gasoline           None         Y
## 308   Premium Gasoline           None         Y
## 309   Regular Gasoline           None         N
## 310   Regular Gasoline           None         N
## 311   Regular Gasoline           None         N
## 312   Regular Gasoline           None         N
## 313   Premium Gasoline           None         N
## 314   Premium Gasoline           None         N
## 315   Premium Gasoline           None         Y
## 316   Premium Gasoline           None         N
## 317   Premium Gasoline           None         N
## 318   Premium Gasoline           None         N
## 319   Premium Gasoline           None         N
## 320   Premium Gasoline           None         N
## 321   Premium Gasoline           None         N
## 322   Premium Gasoline           None         N
## 323             Diesel         Diesel         Y
## 324   Regular Gasoline           None         N
## 325   Regular Gasoline           None         N
## 326             Diesel         Diesel         Y
## 327   Regular Gasoline           None         N
## 328   Regular Gasoline           None         N
## 329             Diesel         Diesel         Y
## 330             Diesel         Diesel         Y
## 331   Regular Gasoline           None         N
## 332   Regular Gasoline           None         N
## 333   Regular Gasoline           None         N
## 334             Diesel         Diesel         Y
## 335   Regular Gasoline           None         N
## 336   Regular Gasoline           None         N
## 337   Regular Gasoline           None         Y
## 338   Regular Gasoline           None         Y
## 339   Regular Gasoline           None         N
## 340   Regular Gasoline           None         N
## 341   Regular Gasoline           None         N
## 342             Diesel         Diesel         Y
## 343             Diesel         Diesel         Y
## 344   Premium Gasoline           None         Y
## 345   Premium Gasoline           None         Y
## 346   Premium Gasoline           None         Y
## 347   Premium Gasoline           None         Y
## 348   Premium Gasoline           None         Y
## 349   Premium Gasoline           None         Y
## 350   Premium Gasoline           None         Y
## 351   Premium Gasoline           None         Y
## 352   Premium Gasoline           None         Y
## 353   Regular Gasoline           None         N
## 354   Premium Gasoline           None         N
## 355   Regular Gasoline           None         Y
## 356   Regular Gasoline         Hybrid         Y
## 357   Regular Gasoline         Hybrid         Y
## 358   Premium Gasoline           None         N
## 359   Regular Gasoline         Hybrid         Y
## 360  Midgrade Gasoline         Hybrid         Y
## 361  Midgrade Gasoline           None         Y
## 362   Regular Gasoline         Hybrid         Y
## 363   Regular Gasoline         Hybrid         Y
## 364  Midgrade Gasoline         Hybrid         Y
## 365  Midgrade Gasoline           None         N
## 366   Premium Gasoline           None         N
## 367   Regular Gasoline           None         Y
## 368   Regular Gasoline           None         Y
## 369   Regular Gasoline           None         Y
## 370   Regular Gasoline           None         N
## 371   Premium Gasoline           None         N
## 372   Premium Gasoline           None         N
## 373   Premium Gasoline         Hybrid         Y
## 374   Premium Gasoline           None         Y
## 375   Premium Gasoline           None         Y
## 376   Premium Gasoline           None         Y
## 377   Premium Gasoline           None         Y
## 378   Regular Gasoline           None         Y
## 379   Regular Gasoline           None         Y
## 380   Regular Gasoline           None         Y
## 381   Regular Gasoline           None         Y
## 382   Regular Gasoline           None         Y
## 383   Regular Gasoline           None         Y
## 384   Regular Gasoline           None         Y
## 385   Regular Gasoline           None         Y
## 386   Regular Gasoline         Hybrid         Y
## 387   Regular Gasoline           None         Y
## 388   Premium Gasoline           None         Y
## 389   Regular Gasoline           None         N
## 390   Premium Gasoline           None         Y
## 391   Premium Gasoline           None         Y
## 392   Premium Gasoline           None         Y
## 393   Premium Gasoline           None         Y
## 394   Premium Gasoline           None         Y
## 395   Premium Gasoline           None         Y
## 396   Premium Gasoline           None         Y
## 397   Premium Gasoline           None         Y
## 398   Premium Gasoline           None         Y
## 399   Premium Gasoline           None         Y
## 400   Premium Gasoline           None         Y
## 401   Premium Gasoline           None         N
## 402   Premium Gasoline           None         Y
## 403   Premium Gasoline           None         Y
## 404   Premium Gasoline           None         Y
## 405   Premium Gasoline           None         Y
## 406   Premium Gasoline           None         Y
## 407   Premium Gasoline           None         Y
## 408   Premium Gasoline           None         Y
## 409   Premium Gasoline         Hybrid         Y
## 410   Premium Gasoline         Hybrid         Y
## 411   Premium Gasoline           None         Y
## 412   Premium Gasoline           None         Y
## 413   Premium Gasoline           None         N
## 414   Premium Gasoline           None         N
## 415   Premium Gasoline           None         N
## 416   Premium Gasoline           None         Y
## 417   Regular Gasoline           None         N
## 418   Premium Gasoline         Hybrid         Y
## 419   Premium Gasoline         Hybrid         Y
## 420   Premium Gasoline         Hybrid         Y
## 421   Premium Gasoline         Hybrid         Y
## 422   Regular Gasoline         Hybrid         Y
## 423   Premium Gasoline           None         N
## 424   Premium Gasoline         Hybrid         Y
## 425   Premium Gasoline           None         Y
## 426   Premium Gasoline           None         Y
## 427   Premium Gasoline           None         Y
## 428   Premium Gasoline           None         Y
## 429   Premium Gasoline           None         Y
## 430   Premium Gasoline           None         Y
## 431   Premium Gasoline           None         Y
## 432   Premium Gasoline           None         Y
## 433   Premium Gasoline           None         Y
## 434   Premium Gasoline           None         Y
## 435   Premium Gasoline           None         Y
## 436   Premium Gasoline           None         Y
## 437   Premium Gasoline           None         Y
## 438   Regular Gasoline           None         N
## 439   Regular Gasoline           None         N
## 440   Regular Gasoline           None         N
## 441   Premium Gasoline         Hybrid         Y
## 442   Premium Gasoline           None         Y
## 443   Premium Gasoline           None         N
## 444   Premium Gasoline           None         N
## 445   Premium Gasoline           None         Y
## 446   Premium Gasoline           None         Y
## 447   Premium Gasoline           None         Y
## 448   Premium Gasoline           None         Y
## 449   Premium Gasoline           None         Y
## 450   Regular Gasoline           None         Y
## 451   Regular Gasoline           None         N
## 452   Regular Gasoline            FFV         N
## 453   Premium Gasoline           None         N
## 454   Regular Gasoline           None         N
## 455   Premium Gasoline           None         N
## 456   Premium Gasoline           None         Y
## 457   Premium Gasoline           None         Y
## 458   Premium Gasoline           None         Y
## 459   Premium Gasoline           None         Y
## 460   Regular Gasoline           None         N
## 461   Regular Gasoline           None         N
## 462   Regular Gasoline           None         N
## 463   Premium Gasoline           None         Y
## 464   Premium Gasoline           None         Y
## 465   Premium Gasoline           None         Y
## 466   Premium Gasoline           None         Y
## 467   Regular Gasoline           None         N
## 468   Regular Gasoline           None         N
## 469   Regular Gasoline           None         Y
## 470   Regular Gasoline           None         Y
## 471   Regular Gasoline         Hybrid         Y
## 472   Regular Gasoline           None         Y
## 473   Regular Gasoline           None         Y
## 474   Regular Gasoline           None         N
## 475   Regular Gasoline           None         N
## 476   Regular Gasoline           None         Y
## 477  Midgrade Gasoline           None         N
## 478   Regular Gasoline           None         Y
## 479   Premium Gasoline           None         Y
## 480   Premium Gasoline           None         Y
## 481   Premium Gasoline           None         Y
## 482   Premium Gasoline           None         Y
## 483   Premium Gasoline           None         Y
## 484   Regular Gasoline           None         Y
## 485  Midgrade Gasoline           None         N
## 486   Regular Gasoline           None         Y
## 487  Midgrade Gasoline           None         N
## 488   Premium Gasoline           None         Y
## 489             Diesel         Diesel         Y
## 490   Premium Gasoline           None         Y
## 491   Premium Gasoline           None         Y
## 492   Premium Gasoline           None         Y
## 493   Premium Gasoline           None         Y
## 494   Premium Gasoline         Hybrid         Y
## 495   Premium Gasoline           None         Y
## 496   Premium Gasoline           None         Y
## 497   Premium Gasoline           None         N
## 498   Premium Gasoline           None         Y
## 499   Premium Gasoline           None         Y
## 500   Premium Gasoline           None         Y
## 501   Premium Gasoline           None         Y
## 502   Regular Gasoline           None         N
## 503   Regular Gasoline           None         N
## 504   Regular Gasoline           None         Y
## 505   Regular Gasoline           None         N
## 506   Regular Gasoline           None         Y
## 507   Regular Gasoline           None         N
## 508   Regular Gasoline         Hybrid         Y
## 509   Regular Gasoline         Hybrid         Y
## 510   Regular Gasoline           None         N
## 511   Regular Gasoline           None         N
## 512   Regular Gasoline           None         N
## 513   Regular Gasoline           None         N
## 514   Regular Gasoline         Hybrid         Y
## 515   Premium Gasoline           None         Y
## 516   Premium Gasoline           None         N
## 517   Regular Gasoline           None         N
## 518   Regular Gasoline           None         N
## 519   Regular Gasoline         Hybrid         Y
## 520   Regular Gasoline         Hybrid         Y
## 521   Premium Gasoline           None         Y
## 522   Premium Gasoline           None         Y
## 523   Premium Gasoline           None         Y
## 524   Regular Gasoline           None         N
## 525   Regular Gasoline           None         Y
## 526   Regular Gasoline           None         Y
## 527             Diesel         Diesel         Y
## 528   Regular Gasoline           None         N
## 529   Regular Gasoline            FFV         N
## 530   Regular Gasoline           None         Y
## 531   Regular Gasoline           None         N
## 532   Regular Gasoline           None         Y
## 533   Regular Gasoline           None         Y
## 534             Diesel         Diesel         Y
## 535   Regular Gasoline           None         N
## 536   Regular Gasoline            FFV         N
## 537   Regular Gasoline           None         Y
## 538   Regular Gasoline           None         N
## 539   Regular Gasoline           None         Y
## 540   Regular Gasoline           None         Y
## 541   Regular Gasoline           None         Y
## 542   Regular Gasoline           None         Y
## 543   Regular Gasoline           None         Y
## 544   Premium Gasoline           None         Y
## 545   Regular Gasoline           None         N
## 546   Regular Gasoline           None         N
## 547   Regular Gasoline            FFV         N
## 548             Diesel         Diesel         Y
## 549   Regular Gasoline           None         Y
## 550   Regular Gasoline           None         N
## 551   Regular Gasoline           None         N
## 552   Regular Gasoline           None         Y
## 553   Regular Gasoline           None         Y
## 554   Regular Gasoline           None         Y
## 555   Regular Gasoline           None         Y
## 556   Regular Gasoline           None         Y
## 557   Premium Gasoline           None         Y
## 558   Premium Gasoline           None         Y
## 559   Regular Gasoline           None         N
## 560   Regular Gasoline           None         N
## 561             Diesel         Diesel         Y
## 562             Diesel         Diesel         Y
## 563   Regular Gasoline           None         Y
## 564   Regular Gasoline           None         N
## 565   Regular Gasoline           None         N
## 566   Regular Gasoline           None         N
## 567   Regular Gasoline           None         N
## 568   Regular Gasoline           None         N
## 569   Regular Gasoline           None         N
## 570   Regular Gasoline           None         N
## 571   Regular Gasoline           None         N
## 572   Regular Gasoline           None         N
## 573   Regular Gasoline           None         N
## 574   Regular Gasoline           None         Y
## 575   Regular Gasoline           None         Y
## 576   Regular Gasoline           None         Y
## 577   Regular Gasoline           None         N
## 578   Premium Gasoline           None         Y
## 579   Premium Gasoline           None         Y
## 580   Regular Gasoline           None         Y
## 581   Regular Gasoline           None         Y
## 582   Regular Gasoline           None         N
## 583   Premium Gasoline           None         Y
## 584   Regular Gasoline           None         Y
## 585   Regular Gasoline           None         Y
## 586   Regular Gasoline           None         N
## 587   Regular Gasoline           None         Y
## 588   Regular Gasoline           None         N
## 589   Premium Gasoline           None         Y
## 590   Premium Gasoline           None         N
## 591   Regular Gasoline           None         N
## 592   Regular Gasoline           None         N
## 593   Premium Gasoline           None         Y
## 594   Premium Gasoline           None         Y
## 595   Premium Gasoline           None         N
## 596   Premium Gasoline           None         Y
## 597   Regular Gasoline           None         Y
## 598   Regular Gasoline           None         Y
## 599   Regular Gasoline           None         Y
## 600   Regular Gasoline           None         Y
## 601   Regular Gasoline           None         Y
## 602   Premium Gasoline           None         Y
## 603   Premium Gasoline           None         Y
## 604   Regular Gasoline Plug-in Hybrid         Y
## 605   Regular Gasoline Plug-in Hybrid         Y
## 606   Premium Gasoline           None         N
## 607   Premium Gasoline           None         N
## 608   Premium Gasoline           None         N
## 609   Premium Gasoline           None         N
## 610   Premium Gasoline           None         N
## 611   Premium Gasoline           None         Y
## 612   Premium Gasoline           None         Y
## 613   Premium Gasoline           None         Y
## 614   Premium Gasoline           None         Y
## 615   Regular Gasoline           None         N
## 616   Regular Gasoline           None         N
## 617   Regular Gasoline         Hybrid         Y
## 618   Premium Gasoline           None         N
## 619   Regular Gasoline         Hybrid         Y
## 620   Regular Gasoline         Hybrid         Y
## 621   Regular Gasoline         Hybrid         Y
## 622   Premium Gasoline           None         Y
## 623   Premium Gasoline           None         Y
## 624   Premium Gasoline           None         Y
## 625   Premium Gasoline           None         Y
## 626   Premium Gasoline           None         Y
## 627   Premium Gasoline           None         Y
## 628   Premium Gasoline           None         Y
## 629   Premium Gasoline           None         Y
## 630   Premium Gasoline           None         Y
## 631   Premium Gasoline           None         Y
## 632   Premium Gasoline           None         Y
## 633   Premium Gasoline           None         Y
## 634   Premium Gasoline           None         Y
## 635   Premium Gasoline           None         Y
## 636   Premium Gasoline           None         Y
## 637   Premium Gasoline           None         N
## 638   Premium Gasoline           None         N
## 639   Premium Gasoline           None         N
## 640   Regular Gasoline           None         N
## 641   Regular Gasoline         Hybrid         Y
## 642   Regular Gasoline           None         N
## 643   Regular Gasoline         Hybrid         Y
## 644   Regular Gasoline           None         N
## 645   Regular Gasoline           None         N
## 646   Regular Gasoline           None         N
## 647   Regular Gasoline Plug-in Hybrid         Y
## 648   Premium Gasoline Plug-in Hybrid         Y
## 649   Premium Gasoline Plug-in Hybrid         Y
## 650   Premium Gasoline Plug-in Hybrid         Y
## 651   Premium Gasoline Plug-in Hybrid         Y
## 652   Premium Gasoline Plug-in Hybrid         Y
## 653   Premium Gasoline           None         N
## 654   Premium Gasoline           None         N
## 655   Regular Gasoline           None         N
## 656   Regular Gasoline           None         N
## 657   Regular Gasoline           None         N
## 658   Regular Gasoline           None         N
## 659   Regular Gasoline           None         N
## 660   Regular Gasoline           None         N
## 661   Regular Gasoline           None         N
## 662   Regular Gasoline           None         N
## 663   Regular Gasoline           None         N
## 664   Regular Gasoline           None         N
## 665   Regular Gasoline           None         N
## 666   Premium Gasoline           None         Y
## 667   Regular Gasoline            FFV         N
## 668   Regular Gasoline           None         Y
## 669   Regular Gasoline           None         N
## 670   Regular Gasoline           None         N
## 671   Regular Gasoline           None         Y
## 672   Premium Gasoline           None         Y
## 673   Regular Gasoline           None         N
## 674   Regular Gasoline           None         N
## 675   Premium Gasoline           None         N
## 676   Premium Gasoline           None         N
## 677   Premium Gasoline           None         N
## 678   Premium Gasoline         Hybrid         Y
## 679   Premium Gasoline         Hybrid         Y
## 680   Premium Gasoline           None         N
## 681   Regular Gasoline           None         Y
## 682   Premium Gasoline           None         Y
## 683   Premium Gasoline           None         Y
## 684   Premium Gasoline           None         Y
## 685   Premium Gasoline           None         Y
## 686   Regular Gasoline           None         Y
## 687   Regular Gasoline           None         Y
## 688   Regular Gasoline           None         Y
## 689   Premium Gasoline           None         Y
## 690   Premium Gasoline           None         Y
## 691   Premium Gasoline           None         Y
## 692   Premium Gasoline           None         Y
## 693   Premium Gasoline           None         Y
## 694   Premium Gasoline           None         Y
## 695   Premium Gasoline           None         Y
## 696   Premium Gasoline           None         N
## 697   Regular Gasoline           None         N
## 698   Premium Gasoline           None         Y
## 699   Premium Gasoline           None         Y
## 700   Premium Gasoline           None         N
## 701   Regular Gasoline           None         N
## 702   Regular Gasoline           None         N
## 703   Premium Gasoline         Hybrid         Y
## 704   Premium Gasoline           None         Y
## 705   Premium Gasoline           None         Y
## 706   Premium Gasoline           None         Y
## 707   Premium Gasoline           None         Y
## 708   Premium Gasoline           None         N
## 709   Regular Gasoline           None         N
## 710   Regular Gasoline           None         N
## 711   Regular Gasoline           None         N
## 712   Regular Gasoline           None         N
## 713   Regular Gasoline           None         N
## 714   Regular Gasoline           None         N
## 715   Regular Gasoline           None         N
## 716   Regular Gasoline           None         N
## 717   Regular Gasoline           None         N
## 718   Regular Gasoline         Hybrid         Y
## 719   Regular Gasoline         Hybrid         Y
## 720   Regular Gasoline           None         N
## 721   Premium Gasoline           None         Y
## 722   Premium Gasoline           None         N
## 723   Premium Gasoline           None         N
## 724   Premium Gasoline           None         N
## 725   Premium Gasoline           None         N
## 726   Regular Gasoline           None         N
## 727   Regular Gasoline           None         N
## 728   Regular Gasoline           None         N
## 729   Regular Gasoline           None         N
## 730   Premium Gasoline           None         N
## 731   Premium Gasoline           None         N
## 732   Regular Gasoline           None         N
## 733   Regular Gasoline           None         N
## 734   Regular Gasoline           None         N
## 735   Regular Gasoline           None         N
## 736   Regular Gasoline           None         N
## 737   Regular Gasoline           None         N
## 738   Regular Gasoline           None         Y
## 739   Regular Gasoline           None         N
## 740   Regular Gasoline           None         Y
## 741   Premium Gasoline           None         Y
## 742   Regular Gasoline           None         N
## 743   Regular Gasoline           None         Y
## 744   Regular Gasoline           None         Y
## 745   Regular Gasoline           None         N
## 746   Regular Gasoline           None         Y
## 747   Premium Gasoline           None         Y
## 748   Premium Gasoline           None         N
## 749   Regular Gasoline           None         N
## 750   Regular Gasoline           None         N
## 751   Regular Gasoline           None         N
## 752   Regular Gasoline           None         N
## 753   Premium Gasoline           None         Y
## 754   Premium Gasoline           None         Y
## 755   Premium Gasoline           None         Y
## 756   Premium Gasoline           None         Y
## 757   Premium Gasoline           None         Y
## 758   Regular Gasoline           None         N
## 759   Premium Gasoline           None         Y
## 760   Premium Gasoline           None         Y
## 761   Regular Gasoline           None         N
## 762   Premium Gasoline           None         N
## 763   Premium Gasoline           None         N
## 764   Premium Gasoline           None         N
## 765   Premium Gasoline           None         N
## 766   Regular Gasoline           None         N
## 767   Regular Gasoline           None         N
## 768   Regular Gasoline           None         N
## 769   Regular Gasoline           None         N
## 770   Regular Gasoline           None         N
## 771   Regular Gasoline           None         Y
## 772   Regular Gasoline           None         Y
## 773   Regular Gasoline           None         Y
## 774   Regular Gasoline           None         N
## 775   Regular Gasoline           None         Y
## 776   Regular Gasoline           None         Y
## 777   Regular Gasoline           None         Y
## 778   Regular Gasoline           None         Y
## 779   Regular Gasoline           None         Y
## 780   Regular Gasoline           None         Y
## 781   Regular Gasoline           None         N
## 782   Regular Gasoline           None         N
## 783   Premium Gasoline           None         Y
## 784   Premium Gasoline           None         Y
## 785   Premium Gasoline           None         Y
## 786   Premium Gasoline           None         Y
## 787   Regular Gasoline           None         N
## 788   Regular Gasoline           None         N
## 789   Regular Gasoline           None         N
## 790   Regular Gasoline           None         N
## 791   Regular Gasoline           None         N
## 792   Regular Gasoline           None         N
## 793   Regular Gasoline           None         N
## 794   Premium Gasoline           None         N
## 795   Regular Gasoline           None         Y
## 796   Regular Gasoline           None         Y
## 797   Premium Gasoline           None         Y
## 798   Premium Gasoline           None         N
## 799   Regular Gasoline           None         Y
## 800   Premium Gasoline         Hybrid         Y
## 801   Premium Gasoline           None         N
## 802   Premium Gasoline Plug-in Hybrid         Y
## 803   Premium Gasoline Plug-in Hybrid         Y
## 804   Regular Gasoline Plug-in Hybrid         Y
## 805   Premium Gasoline Plug-in Hybrid         Y
## 806   Premium Gasoline Plug-in Hybrid         Y
## 807   Premium Gasoline Plug-in Hybrid         Y
## 808   Regular Gasoline           None         Y
## 809   Premium Gasoline           None         Y
## 810   Premium Gasoline           None         Y
## 811   Premium Gasoline           None         N
## 812   Premium Gasoline           None         N
## 813   Premium Gasoline           None         N
## 814   Premium Gasoline           None         N
## 815   Premium Gasoline           None         N
## 816   Premium Gasoline           None         Y
## 817   Premium Gasoline           None         Y
## 818   Regular Gasoline           None         N
## 819   Regular Gasoline           None         N
## 820   Regular Gasoline           None         Y
## 821   Premium Gasoline           None         Y
## 822   Premium Gasoline           None         N
## 823   Premium Gasoline           None         N
## 824   Premium Gasoline           None         N
## 825   Premium Gasoline         Hybrid         Y
## 826   Premium Gasoline         Hybrid         Y
## 827   Premium Gasoline           None         N
## 828   Premium Gasoline           None         N
## 829   Regular Gasoline         Hybrid         Y
## 830   Premium Gasoline           None         Y
## 831   Premium Gasoline           None         N
## 832   Regular Gasoline           None         N
## 833   Regular Gasoline           None         N
## 834   Regular Gasoline           None         N
## 835   Regular Gasoline           None         N
## 836   Regular Gasoline           None         N
## 837   Regular Gasoline           None         N
## 838   Premium Gasoline           None         Y
## 839   Premium Gasoline           None         Y
## 840   Premium Gasoline           None         Y
## 841   Regular Gasoline           None         N
## 842   Regular Gasoline           None         N
## 843   Regular Gasoline           None         N
## 844   Regular Gasoline           None         N
## 845   Regular Gasoline           None         N
## 846             Diesel         Diesel         N
## 847             Diesel         Diesel         N
## 848   Regular Gasoline           None         N
## 849   Premium Gasoline           None         Y
## 850   Premium Gasoline           None         Y
## 851   Regular Gasoline           None         Y
## 852   Premium Gasoline           None         Y
## 853   Premium Gasoline           None         Y
## 854   Premium Gasoline           None         N
## 855   Premium Gasoline           None         N
## 856   Premium Gasoline           None         N
## 857   Premium Gasoline           None         N
## 858   Premium Gasoline           None         N
## 859   Regular Gasoline            FFV         Y
## 860   Regular Gasoline            FFV         Y
## 861   Regular Gasoline            FFV         Y
## 862   Regular Gasoline            FFV         Y
## 863   Regular Gasoline           None         Y
## 864   Regular Gasoline           None         Y
## 865   Regular Gasoline           None         Y
## 866   Regular Gasoline            FFV         Y
## 867   Regular Gasoline            FFV         Y
## 868   Regular Gasoline           None         Y
## 869   Regular Gasoline           None         Y
## 870   Regular Gasoline           None         Y
## 871   Regular Gasoline           None         Y
## 872   Premium Gasoline           None         Y
## 873   Regular Gasoline           None         N
## 874   Regular Gasoline           None         Y
## 875   Regular Gasoline           None         Y
## 876   Premium Gasoline           None         Y
## 877   Regular Gasoline           None         N
## 878   Regular Gasoline           None         Y
## 879   Regular Gasoline         Hybrid         Y
## 880   Regular Gasoline           None         N
## 881   Regular Gasoline           None         Y
## 882   Premium Gasoline           None         Y
## 883   Premium Gasoline         Hybrid         Y
## 884   Premium Gasoline           None         Y
## 885   Premium Gasoline           None         Y
## 886   Premium Gasoline           None         Y
## 887   Premium Gasoline           None         Y
## 888   Premium Gasoline           None         Y
## 889   Premium Gasoline           None         Y
## 890   Premium Gasoline           None         Y
## 891   Premium Gasoline           None         Y
## 892   Regular Gasoline           None         N
## 893   Regular Gasoline           None         N
## 894   Regular Gasoline           None         N
## 895   Regular Gasoline           None         N
## 896   Regular Gasoline           None         Y
## 897   Premium Gasoline           None         Y
## 898   Premium Gasoline           None         Y
## 899   Premium Gasoline           None         Y
## 900   Premium Gasoline           None         N
## 901   Premium Gasoline           None         Y
## 902   Premium Gasoline           None         Y
## 903   Premium Gasoline           None         Y
## 904   Premium Gasoline           None         Y
## 905   Premium Gasoline           None         Y
## 906   Premium Gasoline           None         Y
## 907   Premium Gasoline           None         Y
## 908   Premium Gasoline           None         Y
## 909   Premium Gasoline           None         Y
## 910   Premium Gasoline           None         Y
## 911   Premium Gasoline           None         Y
## 912   Regular Gasoline         Hybrid         Y
## 913   Regular Gasoline         Hybrid         Y
## 914   Premium Gasoline           None         Y
## 915   Premium Gasoline           None         Y
## 916   Premium Gasoline           None         Y
## 917   Regular Gasoline           None         N
## 918   Regular Gasoline           None         N
## 919   Regular Gasoline           None         N
## 920   Regular Gasoline           None         N
## 921   Regular Gasoline           None         Y
## 922   Premium Gasoline           None         Y
## 923   Premium Gasoline         Hybrid         Y
## 924   Premium Gasoline         Hybrid         Y
## 925   Premium Gasoline         Hybrid         Y
## 926   Premium Gasoline           None         Y
## 927   Premium Gasoline           None         Y
## 928   Premium Gasoline           None         Y
## 929   Regular Gasoline           None         N
## 930   Premium Gasoline           None         Y
## 931   Premium Gasoline         Hybrid         Y
## 932   Regular Gasoline         Hybrid         Y
## 933   Regular Gasoline         Hybrid         Y
## 934   Regular Gasoline           None         N
## 935   Regular Gasoline           None         Y
## 936   Regular Gasoline           None         N
## 937   Regular Gasoline           None         N
## 938   Regular Gasoline           None         N
## 939   Regular Gasoline           None         N
## 940   Regular Gasoline           None         N
## 941   Regular Gasoline           None         N
## 942   Premium Gasoline           None         Y
## 943   Regular Gasoline         Hybrid         Y
## 944   Regular Gasoline           None         N
## 945   Premium Gasoline           None         Y
## 946   Regular Gasoline           None         Y
## 947   Regular Gasoline           None         Y
## 948   Regular Gasoline         Hybrid         Y
## 949   Regular Gasoline           None         N
## 950   Premium Gasoline         Hybrid         Y
## 951   Premium Gasoline         Hybrid         Y
## 952   Regular Gasoline           None         Y
## 953   Premium Gasoline           None         Y
## 954   Premium Gasoline           None         Y
## 955   Regular Gasoline           None         Y
## 956   Regular Gasoline           None         Y
## 957   Regular Gasoline           None         Y
## 958   Regular Gasoline           None         Y
## 959   Regular Gasoline           None         Y
## 960   Regular Gasoline           None         Y
## 961   Regular Gasoline           None         Y
## 962   Premium Gasoline           None         Y
## 963   Regular Gasoline Plug-in Hybrid         Y
## 964   Premium Gasoline Plug-in Hybrid         Y
## 965   Regular Gasoline Plug-in Hybrid         Y
## 966   Premium Gasoline         Hybrid         Y
## 967   Premium Gasoline         Hybrid         Y
## 968   Premium Gasoline         Hybrid         Y
## 969   Regular Gasoline           None         Y
## 970   Regular Gasoline           None         N
## 971   Regular Gasoline           None         N
## 972   Premium Gasoline           None         N
## 973   Premium Gasoline           None         N
## 974   Premium Gasoline           None         N
## 975   Premium Gasoline           None         N
## 976   Regular Gasoline           None         N
## 977   Regular Gasoline           None         N
## 978   Premium Gasoline         Hybrid         Y
## 979   Regular Gasoline         Hybrid         Y
## 980   Regular Gasoline         Hybrid         Y
## 981   Regular Gasoline         Hybrid         Y
## 982   Premium Gasoline         Hybrid         Y
## 983   Premium Gasoline         Hybrid         Y
## 984   Premium Gasoline           None         N
## 985   Premium Gasoline           None         N
## 986   Premium Gasoline           None         N
## 987   Premium Gasoline           None         N
## 988   Premium Gasoline           None         N
## 989   Premium Gasoline           None         N
## 990   Premium Gasoline           None         N
## 991   Premium Gasoline           None         N
## 992   Premium Gasoline           None         N
## 993   Premium Gasoline           None         N
## 994   Premium Gasoline           None         N
## 995   Regular Gasoline           None         N
## 996   Premium Gasoline           None         N
## 997   Premium Gasoline           None         N
## 998   Premium Gasoline           None         N
## 999   Premium Gasoline           None         N
## 1000  Regular Gasoline           None         N
## 1001  Regular Gasoline           None         N
## 1002  Regular Gasoline           None         N
## 1003  Premium Gasoline           None         N
## 1004  Regular Gasoline           None         N
## 1005  Premium Gasoline           None         N
## 1006  Premium Gasoline           None         N
## 1007  Regular Gasoline           None         N
## 1008  Regular Gasoline           None         N
## 1009  Regular Gasoline           None         N
## 1010  Regular Gasoline           None         Y
## 1011  Regular Gasoline           None         Y
## 1012  Regular Gasoline            FFV         N
## 1013  Regular Gasoline            FFV         N
## 1014  Regular Gasoline           None         Y
## 1015  Regular Gasoline           None         Y
## 1016  Regular Gasoline           None         N
## 1017  Regular Gasoline           None         N
## 1018  Regular Gasoline           None         Y
## 1019  Regular Gasoline           None         N
## 1020  Regular Gasoline           None         N
## 1021  Regular Gasoline           None         N
## 1022  Regular Gasoline           None         Y
## 1023  Regular Gasoline           None         Y
## 1024  Premium Gasoline         Hybrid         Y
## 1025  Premium Gasoline         Hybrid         Y
## 1026  Regular Gasoline Plug-in Hybrid         Y
## 1027  Premium Gasoline Plug-in Hybrid         Y
## 1028  Premium Gasoline Plug-in Hybrid         Y
## 1029  Premium Gasoline Plug-in Hybrid         Y
## 1030  Premium Gasoline           None         Y
## 1031  Premium Gasoline           None         Y
## 1032  Premium Gasoline           None         Y
## 1033  Regular Gasoline           None         N
## 1034  Regular Gasoline           None         N
## 1035  Premium Gasoline           None         Y
## 1036  Premium Gasoline           None         Y
## 1037  Premium Gasoline           None         Y
## 1038  Regular Gasoline           None         N
## 1039  Regular Gasoline           None         N
## 1040  Premium Gasoline           None         Y
## 1041  Premium Gasoline Plug-in Hybrid         Y
## 1042  Premium Gasoline Plug-in Hybrid         Y
## 1043  Premium Gasoline Plug-in Hybrid         Y
## 1044  Premium Gasoline Plug-in Hybrid         Y
## 1045  Premium Gasoline Plug-in Hybrid         Y
## 1046  Regular Gasoline Plug-in Hybrid         Y
## 1047  Premium Gasoline           None         Y
## 1048  Regular Gasoline           None         N
## 1049  Regular Gasoline           None         Y
## 1050  Regular Gasoline           None         Y
## 1051  Premium Gasoline           None         Y
## 1052  Premium Gasoline           None         N
## 1053  Premium Gasoline           None         Y
## 1054  Premium Gasoline           None         N
## 1055 Midgrade Gasoline           None         N
## 1056  Premium Gasoline           None         N
## 1057  Regular Gasoline           None         N
## 1058 Midgrade Gasoline           None         N
## 1059 Midgrade Gasoline           None         N
## 1060  Premium Gasoline           None         Y
## 1061  Premium Gasoline           None         Y
## 1062  Regular Gasoline           None         N
## 1063  Regular Gasoline           None         N
## 1064  Regular Gasoline           None         N
## 1065  Regular Gasoline           None         Y
## 1066  Regular Gasoline           None         N
## 1067 Midgrade Gasoline           None         N
## 1068  Regular Gasoline           None         N
## 1069  Regular Gasoline           None         N
## 1070 Midgrade Gasoline           None         N
## 1071  Regular Gasoline           None         Y
## 1072  Regular Gasoline           None         Y
## 1073            Diesel         Diesel         Y
## 1074  Premium Gasoline           None         Y
## 1075  Premium Gasoline           None         Y
## 1076  Premium Gasoline           None         Y
## 1077  Premium Gasoline           None         Y
## 1078  Premium Gasoline           None         Y
## 1079  Premium Gasoline           None         Y
## 1080  Premium Gasoline           None         Y
## 1081  Premium Gasoline           None         Y
## 1082  Premium Gasoline           None         Y
## 1083  Premium Gasoline           None         Y
## 1084  Premium Gasoline           None         Y
## 1085  Premium Gasoline           None         Y
## 1086  Premium Gasoline           None         Y
## 1087  Premium Gasoline           None         Y
## 1088  Premium Gasoline           None         N
## 1089  Premium Gasoline           None         N
## 1090  Regular Gasoline           None         N
## 1091  Regular Gasoline           None         N
## 1092  Regular Gasoline           None         N
## 1093  Premium Gasoline         Hybrid         Y
## 1094  Regular Gasoline           None         Y
## 1095  Regular Gasoline           None         N
## 1096  Regular Gasoline           None         Y
## 1097  Regular Gasoline           None         N
## 1098  Regular Gasoline           None         N
## 1099  Premium Gasoline           None         Y
## 1100  Premium Gasoline           None         Y
## 1101  Premium Gasoline           None         N
## 1102  Premium Gasoline           None         N
## 1103  Regular Gasoline           None         N
## 1104  Regular Gasoline           None         Y
## 1105  Regular Gasoline           None         N
## 1106  Premium Gasoline           None         Y
## 1107  Premium Gasoline           None         Y
## 1108  Premium Gasoline           None         Y
## 1109  Premium Gasoline           None         Y
## 1110  Premium Gasoline Plug-in Hybrid         Y
## 1111  Premium Gasoline Plug-in Hybrid         Y
## 1112  Regular Gasoline           None         N
## 1113  Regular Gasoline           None         N
## 1114  Premium Gasoline           None         N
## 1115  Premium Gasoline           None         N
## 1116  Premium Gasoline           None         N
## 1117  Regular Gasoline           None         Y
## 1118  Regular Gasoline           None         Y
## 1119  Regular Gasoline           None         Y
## 1120  Regular Gasoline           None         Y
## 1121  Regular Gasoline           None         Y
## 1122  Premium Gasoline           None         N
## 1123  Premium Gasoline           None         Y
## 1124  Premium Gasoline           None         Y
## 1125  Premium Gasoline           None         Y
## 1126  Premium Gasoline           None         Y
## 1127  Premium Gasoline           None         Y
## 1128  Premium Gasoline           None         Y
## 1129  Premium Gasoline           None         Y
## 1130  Premium Gasoline           None         Y
## 1131  Premium Gasoline           None         Y
## 1132  Premium Gasoline           None         Y
## 1133  Premium Gasoline           None         Y
## 1134  Premium Gasoline           None         Y
## 1135  Regular Gasoline           None         N
## 1136  Regular Gasoline           None         N
## 1137  Regular Gasoline         Hybrid         Y
## 1138  Regular Gasoline         Hybrid         Y
## 1139  Regular Gasoline         Hybrid         Y
## 1140  Regular Gasoline         Hybrid         Y
## 1141  Regular Gasoline         Hybrid         Y
## 1142  Regular Gasoline Plug-in Hybrid         Y
## 1143  Premium Gasoline           None         Y
## 1144  Premium Gasoline           None         Y
## 1145  Premium Gasoline           None         Y
## 1146  Premium Gasoline           None         Y
## 1147  Premium Gasoline           None         N
## 1148  Regular Gasoline         Hybrid         Y
## 1149  Premium Gasoline           None         Y
## 1150  Premium Gasoline           None         Y
## 1151  Premium Gasoline         Hybrid         Y
## 1152  Regular Gasoline           None         N
## 1153  Regular Gasoline           None         N
## 1154            Diesel         Diesel         Y
## 1155            Diesel         Diesel         Y
## 1156            Diesel         Diesel         Y
## 1157            Diesel         Diesel         Y
## 1158  Premium Gasoline Plug-in Hybrid         Y
## 1159  Premium Gasoline Plug-in Hybrid         Y
## 1160  Premium Gasoline           None         Y
## 1161  Premium Gasoline           None         Y
## 1162  Premium Gasoline           None         Y
## 1163  Premium Gasoline           None         Y
## 1164  Premium Gasoline Plug-in Hybrid         Y
```

```r
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


