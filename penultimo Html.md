---
title: "Motion Prediction"
author: "Gian Balsamo"
date: "November 17, 2015"
header-includes: \usepackage{graphicx}
---
## Introduction

It is well known that data from accelerometers enable us to gauge the frequency and intensity of certain physical activities. Less well known and less explored is the fact that the same sort of data may be used to gauge the quality of certain physical activities. This report is inspired by a 2013 study by Andreas Bulling et alia, cited in Reference, where it is shown that the quality of certain physical activities may be captured by a machine learning approach. Bulling and his colleagues elaborated their views based on data collected from accelerometers placed on the belt, forearm, arm, and dumbell of six participants who were asked to perform barbell lifts in one correct way (class A) and 4 incorrect ways (classes B, C, D, and E).  
The following image, taken from Figure 1 of Bulling et alia's study, gives a visual illustration of the logic inherent in this data collection.

![caption.](/Users/gianfrancobalsamo/Dropbox/BIG_DATA/practical\ machine\ learning/final_project/essential_image.png)

For the extraction of quantitative features, Bulling and his colleagues “used a sliding window approach with different lengths from 0.5 second to 2.5 seconds, with 0.5 second overlap. In each step of the sliding window approach [they] calculated features on the Euler angles (roll, pitch and yaw), as well as the raw accelerometer, gyroscope and magnetometer readings” [p.3]. From the resulting dataset, they computed classifiers designed to predict a subject’s performance quality.  
In this report I process their data through a supervised machine learning approach to compute and test the accuracy of my own classifier.  
Below is the code of my initial data assemblage and "cleaning." I have set a seed so as to make my results reproducible.





```r
set.seed(1234)
dati<-read.csv("pml-training.csv")
```

```
## Warning in file(file, "rt"): cannot open file 'pml-training.csv': No such
## file or directory
```

```
## Error in file(file, "rt"): cannot open the connection
```

```r
classColumn<-dati[,dim(dati)[2]]
```

```
## Error in eval(expr, envir, enclos): object 'dati' not found
```

```r
dati<-dati[,-c(1:7)]
```

```
## Error in eval(expr, envir, enclos): object 'dati' not found
```

```r
toDrop<-nearZeroVar(dati[,-dim(dati)[2]], saveMetrics=TRUE)
```

```
## Error in is.vector(x): object 'dati' not found
```

```r
good_columns<-names(dati[,-dim(dati)[2]][!toDrop[,dim(toDrop)[2]]])
```

```
## Error in eval(expr, envir, enclos): object 'dati' not found
```

```r
lista<-match(good_columns,names(dati[,-dim(dati)[2]]))
```

```
## Error in match(good_columns, names(dati[, -dim(dati)[2]])): object 'good_columns' not found
```

```r
lista<-as.vector(lista)
```

```
## Error in as.vector(lista): object 'lista' not found
```

```r
dati<-dati[,-dim(dati)[2]][,lista]
```

```
## Error in eval(expr, envir, enclos): object 'dati' not found
```

```r
datiFinal <- dati 
```

```
## Error in eval(expr, envir, enclos): object 'dati' not found
```

```r
for(i in 1:length(dati)) {
      if( sum( is.na(dati[, i] ) ) /nrow(dati) >= .8 ) {
            for(j in 1:length(datiFinal)) {
                  if( length(grep(names(dati[i]), names(datiFinal)[j]) ) ==1)  {
                        datiFinal<-datiFinal[ , -j]
                  }   
            } 
      }
}
```

```
## Error in eval(expr, envir, enclos): object 'dati' not found
```

```r
datiFinal<-cbind(datiFinal,classColumn)
```

```
## Error in cbind(datiFinal, classColumn): object 'datiFinal' not found
```

```r
datiFinal$classColumn<-as.factor(datiFinal$classColumn)
```

```
## Error in is.factor(x): object 'datiFinal' not found
```

```r
countZ<-0
for(i in 1:length(dati)) {
      if( sum( is.na(dati[, i] ) ) /nrow(dati) >= .8 ) { 
            countZ=countZ+1
            }
      }
```

```
## Error in eval(expr, envir, enclos): object 'dati' not found
```











