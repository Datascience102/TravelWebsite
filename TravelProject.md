---
title: "A Market Segmentation and Purchase Drivers Process"
author: "Alessandro Quintarelli, Marcelo Santiso, Oreste Gasparro"
output:
  html_document:
    css: AnalyticsStyles/default.css
    theme: paper
    toc: yes
    toc_float:
      collapsed: no
      smooth_scroll: yes
  pdf_document:
    includes:
      in_header: AnalyticsStyles/default.sty
always_allow_html: yes
---

> 
<<<<<<< HEAD
=======
# Client description

ABC Hotels & Resorts is a hotel chain based in Spain, specializing in 4 and 5 star hotels, owned by Grupo ABC.

The client has its own website, which allows for online booking and receives traffic from several countries.

>>>>>>> ebe986fedbc680e1b054f450e1888c3bdb913310
# The Business Questions

Identify the main customer segments and define market strategies to increase revenues generated.

# The Process

<<<<<<< HEAD
The process we followed can be splitted in 3 different parts:

1. *Part 1*: We analyse the different attributes to find **key customer descriptors** using *dimensionality reduction* techniques 

3. *Part 2*: We will use the results of this analysis to make business decisions about  positioning depending on key purchase drivers we find at the end of this process.

=======
The process we followed can be splitted in 2 different parts:

1. *Part 1*: We describe the dataset and the different attributes to find **key customer descriptors** using *dimensionality reduction* techniques 

2. *Part 2*: We will use the results of this analysis to make business decisions about positioning depending on key purchase drivers we find at the end of this process.
>>>>>>> ebe986fedbc680e1b054f450e1888c3bdb913310

# The Data

First we load the data to use:


```r
myData <- read.csv(file = "Data/Iberostar.csv", header = TRUE, sep = ",")
MIN_VALUE = 0.5
max_data_report = 10
```
<<<<<<< HEAD
=======

The data refer to the users and their interaction with the website until that time.

Data description:
Type of User: Returning or New Visitor
Source: Organic vs Paid Promotions (cpc, cpm, referral)
Users: Number of User Visits
Sessions: Number of User Sessions
No of Pages Visited: Number of total pages visited
No of Transactions: Number of monetary transactions
Revenue: Revenue generated
Dummy Variable: We created dummy variables categorical data such as 'user type', 'source' and 'device type'  
>>>>>>> ebe986fedbc680e1b054f450e1888c3bdb913310
  
# Part 1: Key Customer Characteristics


```r
factor_attributes_used = c(1:19)
factor_selectionciterion = minimum_variance_explained = 65
manual_numb_factors_used = 5
rotation_used = "varimax"
```


```r
factor_attributes_used <- intersect(factor_attributes_used, 1:ncol(myData))
ProjectDataFactor <- myData[,factor_attributes_used]
<<<<<<< HEAD
ProjectDataFactor <- myData <- data.matrix(ProjectDataFactor)
=======
ProjectDataFactor <- data.matrix(ProjectDataFactor)
>>>>>>> ebe986fedbc680e1b054f450e1888c3bdb913310
```

```
## Warning in data.matrix(ProjectDataFactor): si è prodotto un NA per
## coercizione

## Warning in data.matrix(ProjectDataFactor): si è prodotto un NA per
## coercizione

## Warning in data.matrix(ProjectDataFactor): si è prodotto un NA per
## coercizione
```

## Steps 1-2: Check the Data 

<<<<<<< HEAD
=======
Overall the dataset contains 

```r
nrow(ProjectDataFactor)
```

```
## [1] 667
```

>>>>>>> ebe986fedbc680e1b054f450e1888c3bdb913310
Start by some basic visual exploration of, say, a few data:


```r
rownames(ProjectDataFactor) <- paste0("Obs.", sprintf("%02i", 1:nrow(ProjectDataFactor)))
<<<<<<< HEAD
print(t(head(round(ProjectDataFactor, 2), max_data_report)))
```

```
##                       Obs.01   Obs.02   Obs.03   Obs.04 Obs.05 Obs.06
## Type.of.User              NA       NA       NA       NA     NA     NA
## Source                    NA       NA       NA       NA     NA     NA
## Device                    NA       NA       NA       NA     NA     NA
## Returning.Visitor        1.0      1.0      0.0      1.0      1      0
## New.Visitor              0.0      0.0      1.0      0.0      0      1
## Google.CPC               0.0      0.0      0.0      0.0      1      0
## TripAdvisor              0.0      0.0      0.0      0.0      0      0
## Direct                   0.0      0.0      1.0      0.0      0      1
## DFA                      0.0      0.0      0.0      0.0      0      0
## Email                    0.0      0.0      0.0      0.0      0      0
## Referral                 0.0      0.0      0.0      0.0      0      0
## Desktop                  1.0      1.0      1.0      1.0      1      1
## Mobile                   0.0      0.0      0.0      0.0      0      0
## Tablet                   0.0      0.0      0.0      0.0      0      0
## Users                 7240.0  21442.0   2989.0   2637.0   3538  25099
## Sessions             10776.0  35015.0   2996.0   4112.0   5179  25134
## No.of.Pages.Visited  45328.0 173192.0   7989.0  16936.0  21534  42127
## No.of.Transactions      64.0    130.0     13.0     15.0     35    142
## Revenue             456710.5 362368.9 341553.9 278226.1 216213 205256
##                       Obs.07 Obs.08 Obs.09 Obs.10
## Type.of.User              NA     NA     NA     NA
## Source                    NA     NA     NA     NA
## Device                    NA     NA     NA     NA
## Returning.Visitor        0.0      1      1      1
## New.Visitor              1.0      0      0      0
## Google.CPC               0.0      0      1      0
## TripAdvisor              0.0      0      0      0
## Direct                   0.0      1      0      0
## DFA                      0.0      0      0      0
## Email                    0.0      0      0      0
## Referral                 0.0      0      0      0
## Desktop                  1.0      1      1      1
## Mobile                   0.0      0      0      0
## Tablet                   0.0      0      0      0
## Users                23225.0   4826  11348   6487
## Sessions             23302.0   7648  17901  10222
## No.of.Pages.Visited  90779.0  32763  87182  39352
## No.of.Transactions      28.0     55     54     81
## Revenue             187332.8 185296 183433 182812
=======
print(head(round(ProjectDataFactor, 2), max_data_report))
```

```
##        Type.of.User Source Device Returning.Visitor New.Visitor Google.CPC
## Obs.01           NA     NA     NA                 1           0          0
## Obs.02           NA     NA     NA                 1           0          0
## Obs.03           NA     NA     NA                 0           1          0
## Obs.04           NA     NA     NA                 1           0          0
## Obs.05           NA     NA     NA                 1           0          1
## Obs.06           NA     NA     NA                 0           1          0
## Obs.07           NA     NA     NA                 0           1          0
## Obs.08           NA     NA     NA                 1           0          0
## Obs.09           NA     NA     NA                 1           0          1
## Obs.10           NA     NA     NA                 1           0          0
##        TripAdvisor Direct DFA Email Referral Desktop Mobile Tablet Users
## Obs.01           0      0   0     0        0       1      0      0  7240
## Obs.02           0      0   0     0        0       1      0      0 21442
## Obs.03           0      1   0     0        0       1      0      0  2989
## Obs.04           0      0   0     0        0       1      0      0  2637
## Obs.05           0      0   0     0        0       1      0      0  3538
## Obs.06           0      1   0     0        0       1      0      0 25099
## Obs.07           0      0   0     0        0       1      0      0 23225
## Obs.08           0      1   0     0        0       1      0      0  4826
## Obs.09           0      0   0     0        0       1      0      0 11348
## Obs.10           0      0   0     0        0       1      0      0  6487
##        Sessions No.of.Pages.Visited No.of.Transactions  Revenue
## Obs.01    10776               45328                 64 456710.5
## Obs.02    35015              173192                130 362368.9
## Obs.03     2996                7989                 13 341553.9
## Obs.04     4112               16936                 15 278226.1
## Obs.05     5179               21534                 35 216213.0
## Obs.06    25134               42127                142 205256.0
## Obs.07    23302               90779                 28 187332.8
## Obs.08     7648               32763                 55 185296.0
## Obs.09    17901               87182                 54 183433.0
## Obs.10    10222               39352                 81 182812.0
>>>>>>> ebe986fedbc680e1b054f450e1888c3bdb913310
```

The data we use here have the following descriptive statistics: 


```r
<<<<<<< HEAD
#print(round(my_summary(ProjectDataFactor), 2))  ERROR TO BE SOLVED
=======
print(summary(ProjectDataFactor))
```

```
##   Type.of.User     Source        Device    Returning.Visitor
##  Min.   : NA   Min.   : NA   Min.   : NA   Min.   :0.0000   
##  1st Qu.: NA   1st Qu.: NA   1st Qu.: NA   1st Qu.:0.0000   
##  Median : NA   Median : NA   Median : NA   Median :1.0000   
##  Mean   :NaN   Mean   :NaN   Mean   :NaN   Mean   :0.6552   
##  3rd Qu.: NA   3rd Qu.: NA   3rd Qu.: NA   3rd Qu.:1.0000   
##  Max.   : NA   Max.   : NA   Max.   : NA   Max.   :1.0000   
##  NA's   :667   NA's   :667   NA's   :667                    
##   New.Visitor       Google.CPC       TripAdvisor          Direct      
##  Min.   :0.0000   Min.   :0.00000   Min.   :0.00000   Min.   :0.0000  
##  1st Qu.:0.0000   1st Qu.:0.00000   1st Qu.:0.00000   1st Qu.:0.0000  
##  Median :0.0000   Median :0.00000   Median :0.00000   Median :0.0000  
##  Mean   :0.3448   Mean   :0.08396   Mean   :0.04798   Mean   :0.1589  
##  3rd Qu.:1.0000   3rd Qu.:0.00000   3rd Qu.:0.00000   3rd Qu.:0.0000  
##  Max.   :1.0000   Max.   :1.00000   Max.   :1.00000   Max.   :1.0000  
##                                                                       
##       DFA              Email            Referral         Desktop      
##  Min.   :0.00000   Min.   :0.00000   Min.   :0.0000   Min.   :0.0000  
##  1st Qu.:0.00000   1st Qu.:0.00000   1st Qu.:0.0000   1st Qu.:1.0000  
##  Median :0.00000   Median :0.00000   Median :0.0000   Median :1.0000  
##  Mean   :0.08096   Mean   :0.05397   Mean   :0.3583   Mean   :0.8081  
##  3rd Qu.:0.00000   3rd Qu.:0.00000   3rd Qu.:1.0000   3rd Qu.:1.0000  
##  Max.   :1.00000   Max.   :1.00000   Max.   :1.0000   Max.   :1.0000  
##                                                                       
##      Mobile            Tablet           Users          Sessions      
##  Min.   :0.00000   Min.   :0.0000   Min.   :    1   Min.   :    1.0  
##  1st Qu.:0.00000   1st Qu.:0.0000   1st Qu.:   10   1st Qu.:   17.0  
##  Median :0.00000   Median :0.0000   Median :  109   Median :  161.0  
##  Mean   :0.07796   Mean   :0.1139   Mean   : 1484   Mean   : 1794.0  
##  3rd Qu.:0.00000   3rd Qu.:0.0000   3rd Qu.:  607   3rd Qu.:  792.5  
##  Max.   :1.00000   Max.   :1.0000   Max.   :55675   Max.   :55917.0  
##                                                                      
##  No.of.Pages.Visited No.of.Transactions    Revenue        
##  Min.   :     1.0    Min.   :  1.000    Min.   :    46.8  
##  1st Qu.:    70.5    1st Qu.:  1.000    1st Qu.:  1017.7  
##  Median :   489.0    Median :  1.000    Median :  2412.0  
##  Mean   :  7060.3    Mean   :  5.685    Mean   : 13586.0  
##  3rd Qu.:  2810.5    3rd Qu.:  3.000    3rd Qu.:  6584.3  
##  Max.   :260886.0    Max.   :142.000    Max.   :456710.5  
## 
>>>>>>> ebe986fedbc680e1b054f450e1888c3bdb913310
```

## Step 3: Check Correlations

We can then analyze the data in terms of correlation between different variables.
In this way we have a first idea of possible cross-relationships between variables that could lead to a dimensionality reduction in our analysis.


```r
thecor = round(cor(myData[,4:19]),2)
print(round(thecor,2), scale=TRUE)
```

```
##                     Returning.Visitor New.Visitor Google.CPC TripAdvisor
## Returning.Visitor                1.00       -1.00      -0.03       -0.01
## New.Visitor                     -1.00        1.00       0.03        0.01
## Google.CPC                      -0.03        0.03       1.00       -0.07
## TripAdvisor                     -0.01        0.01      -0.07        1.00
## Direct                          -0.19        0.19      -0.13       -0.10
## DFA                              0.11       -0.11      -0.09       -0.07
## Email                            0.08       -0.08      -0.07       -0.05
## Referral                         0.09       -0.09      -0.23        0.10
## Desktop                          0.10       -0.10       0.13       -0.09
## Mobile                          -0.06        0.06      -0.09        0.01
## Tablet                          -0.07        0.07      -0.09        0.10
## Users                           -0.25        0.25       0.04       -0.06
## Sessions                        -0.18        0.18       0.05       -0.07
## No.of.Pages.Visited             -0.16        0.16       0.05       -0.06
## No.of.Transactions               0.05       -0.05       0.14       -0.06
## Revenue                          0.03       -0.03       0.10       -0.06
##                     Direct   DFA Email Referral Desktop Mobile Tablet
## Returning.Visitor    -0.19  0.11  0.08     0.09    0.10  -0.06  -0.07
## New.Visitor           0.19 -0.11 -0.08    -0.09   -0.10   0.06   0.07
## Google.CPC           -0.13 -0.09 -0.07    -0.23    0.13  -0.09  -0.09
## TripAdvisor          -0.10 -0.07 -0.05     0.10   -0.09   0.01   0.10
## Direct                1.00 -0.13 -0.10    -0.31   -0.13   0.12   0.06
## DFA                  -0.13  1.00 -0.07    -0.22    0.01  -0.02   0.01
## Email                -0.10 -0.07  1.00    -0.15    0.12  -0.07  -0.09
## Referral             -0.31 -0.22 -0.15     1.00   -0.05   0.04   0.03
## Desktop              -0.13  0.01  0.12    -0.05    1.00  -0.60  -0.74
## Mobile                0.12 -0.02 -0.07     0.04   -0.60   1.00  -0.10
## Tablet                0.06  0.01 -0.09     0.03   -0.74  -0.10   1.00
## Users                 0.15 -0.05 -0.07    -0.22   -0.07   0.12  -0.01
## Sessions              0.14 -0.03 -0.07    -0.24   -0.10   0.13   0.01
## No.of.Pages.Visited   0.10 -0.04 -0.07    -0.23   -0.13   0.18   0.00
## No.of.Transactions    0.11  0.01 -0.06    -0.19    0.13  -0.07  -0.10
## Revenue               0.16 -0.01 -0.06    -0.20    0.11  -0.06  -0.09
##                     Users Sessions No.of.Pages.Visited No.of.Transactions
## Returning.Visitor   -0.25    -0.18               -0.16               0.05
## New.Visitor          0.25     0.18                0.16              -0.05
## Google.CPC           0.04     0.05                0.05               0.14
## TripAdvisor         -0.06    -0.07               -0.06              -0.06
## Direct               0.15     0.14                0.10               0.11
## DFA                 -0.05    -0.03               -0.04               0.01
## Email               -0.07    -0.07               -0.07              -0.06
## Referral            -0.22    -0.24               -0.23              -0.19
## Desktop             -0.07    -0.10               -0.13               0.13
## Mobile               0.12     0.13                0.18              -0.07
## Tablet              -0.01     0.01                0.00              -0.10
## Users                1.00     0.98                0.92               0.46
## Sessions             0.98     1.00                0.95               0.53
## No.of.Pages.Visited  0.92     0.95                1.00               0.44
## No.of.Transactions   0.46     0.53                0.44               1.00
## Revenue              0.39     0.45                0.40               0.74
##                     Revenue
## Returning.Visitor      0.03
## New.Visitor           -0.03
## Google.CPC             0.10
## TripAdvisor           -0.06
## Direct                 0.16
## DFA                   -0.01
## Email                 -0.06
## Referral              -0.20
## Desktop                0.11
## Mobile                -0.06
## Tablet                -0.09
## Users                  0.39
## Sessions               0.45
## No.of.Pages.Visited    0.40
## No.of.Transactions     0.74
## Revenue                1.00
```

From correlation matrix we can see that there is a good positive correlation between number of Sessions, number of pages visited, number of transactions and revenues.
We can also see that Desktop device is the one that generates most of the traffic and revenues in absolute value.


## Step 4: Choose number of factors

To use Factor Analysis, we first need to adjust data to have only numeric data. 
We can then remove first columns and keep only binary values.


```r
num_data <- myData[,4:ncol(myData)]
scaled_data <- apply(num_data,2, function(r) {if (sd(r)!=0) res=(r-mean(r))/sd(r) else res=0*r; res})
#scaled_data = data.frame(cbind(colnames(scaled_data), round(cor(scaled_data),2)))
```

Now that we have only numeric data, we can proceed with Factor Analysis.


```r
Variance_Explained_Table_results<-PCA(scaled_data, graph=FALSE)
Variance_Explained_Table<-Variance_Explained_Table_results$eig
#Variance_Explained_Table_copy<-Variance_Explained_Table
#row=1:nrow(Variance_Explained_Table)
#name<-paste("Component No:",row,sep="")
#Variance_Explained_Table<-cbind(name,Variance_Explained_Table)
Variance_Explained_Table<-as.data.frame(Variance_Explained_Table)
colnames(Variance_Explained_Table)<-c("Eigenvalue", "Percentage_of_explained_variance", "Cumulative_percentage_of_explained_variance")

eigenvalues  <- Variance_Explained_Table[,2]
```

Let's look at the **variance explained** as well as the **eigenvalues**


```r
iprint.df(round(Variance_Explained_Table, 2))
```

<!--html_preserve--><div class="formattable_container"><table class="table table-condensed">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> Eigenvalue </th>
   <th style="text-align:right;"> Percentage_of_explained_variance </th>
   <th style="text-align:right;"> Cumulative_percentage_of_explained_variance </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> comp 1 </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 100.00%">3.84</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 100.00%">24.01</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 10.00%">24.01</span> </td>
  </tr>
  <tr>
   <td style="text-align:left;"> comp 2 </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 64.84%">2.34</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 64.76%">14.61</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 27.29%">38.61</span> </td>
  </tr>
  <tr>
   <td style="text-align:left;"> comp 3 </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 52.42%">1.81</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 52.32%">11.29</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 40.68%">49.91</span> </td>
  </tr>
  <tr>
   <td style="text-align:left;"> comp 4 </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 40.23%">1.29</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 40.25%">8.07</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 50.23%">57.98</span> </td>
  </tr>
  <tr>
   <td style="text-align:left;"> comp 5 </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 37.42%">1.17</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 37.48%">7.33</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 58.91%">65.31</span> </td>
  </tr>
  <tr>
   <td style="text-align:left;"> comp 6 </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 35.78%">1.10</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 35.79%">6.88</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 67.06%">72.19</span> </td>
  </tr>
  <tr>
   <td style="text-align:left;"> comp 7 </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 34.61%">1.05</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 34.55%">6.55</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 74.82%">78.74</span> </td>
  </tr>
  <tr>
   <td style="text-align:left;"> comp 8 </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 33.91%">1.02</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 33.95%">6.39</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 82.39%">85.13</span> </td>
  </tr>
  <tr>
   <td style="text-align:left;"> comp 9 </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 31.56%">0.92</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 31.52%">5.74</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 89.19%">90.87</span> </td>
  </tr>
  <tr>
   <td style="text-align:left;"> comp 10 </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 28.28%">0.78</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 28.33%">4.89</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 94.98%">95.76</span> </td>
  </tr>
  <tr>
   <td style="text-align:left;"> comp 11 </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 17.97%">0.34</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 18.06%">2.15</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 97.52%">97.91</span> </td>
  </tr>
  <tr>
   <td style="text-align:left;"> comp 12 </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 15.86%">0.25</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 15.85%">1.56</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 99.37%">99.47</span> </td>
  </tr>
  <tr>
   <td style="text-align:left;"> comp 13 </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 11.64%">0.07</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 11.69%">0.45</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 99.92%">99.93</span> </td>
  </tr>
  <tr>
   <td style="text-align:left;"> comp 14 </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 10.23%">0.01</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 10.26%">0.07</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 100.00%">100.00</span> </td>
  </tr>
  <tr>
   <td style="text-align:left;"> comp 15 </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 10.00%">0.00</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 10.00%">0.00</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 100.00%">100.00</span> </td>
  </tr>
  <tr>
   <td style="text-align:left;"> comp 16 </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 10.00%">0.00</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 10.00%">0.00</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 100.00%">100.00</span> </td>
  </tr>
</tbody>
</table></div><!--/html_preserve-->


```r
eigenvalues  <- Variance_Explained_Table[, "Eigenvalue"]
df           <- cbind(as.data.frame(eigenvalues), c(1:length(eigenvalues)), rep(1, length(eigenvalues)))
colnames(df) <- c("eigenvalues", "components", "abline")
iplot.df(melt(df, id="components"))
```

```
## Error in loadNamespace(name): there is no package called 'webshot'
```

## Step 5: Interpret the Components

We can see from the chart above that we could use only 8 components.
This can be easily explained looking at the variables. Some variables are binary values conveying the same meaning. 
We can identify the following groups:

* Device
* Returning/New Visitor
* Source
* Revenues
* Number of Sessions
* Number of pages visited
* Number of Users
* Number of Transactions

In this case we suggest to keep all the original variables since they are just a way to provide the same information in a binary form.

<hr>\clearpage

# Part 2: Marketing Strategy

To analyze different scenarios and identify possible strategies, we focus on different behaviors across devices.


```r
<<<<<<< HEAD
data <- as.data.frame(myData)
averageTransaction <- aggregate(data$No.of.Transactions,list(data$Device), FUN=mean)
averageRevenue <- aggregate(data$Revenue,list(data$Device), FUN=mean)
RevTransaction <- averageRevenue
RevTransaction[,2] <- averageRevenue[,2]/averageTransaction[,2] 
```

```
## Error in averageRevenue[, 2]/averageTransaction[, 2]: argomento non numerico trasformato in operatore binario
```

```r
=======
averageTransaction <- aggregate(myData$No.of.Transactions,list(myData$Device), FUN=mean)
averageRevenue <- aggregate(myData$Revenue,list(myData$Device), FUN=mean)
RevTransaction <- averageRevenue
RevTransaction[,2] <- averageRevenue[,2]/averageTransaction[,2] 
>>>>>>> ebe986fedbc680e1b054f450e1888c3bdb913310
colnames(RevTransaction) <- c("Device","Revenue per Transaction")
print(RevTransaction)
```

```
<<<<<<< HEAD
## [1] Device                  Revenue per Transaction
## <0 rows> (or 0-length row.names)
=======
##    Device Revenue per Transaction
## 1 desktop                2381.024
## 2  mobile                2452.472
## 3  tablet                2585.233
>>>>>>> ebe986fedbc680e1b054f450e1888c3bdb913310
```
We can see that revenues per transaction are quite stable cross-device, so there is no way to differentiate focusing on one device.


