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
#render("TravelProject.Rmd", html_document())
---

> 
# Client description

ABC Hotels & Resorts is a hotel chain based in Spain, specializing in 4 and 5 star hotels, owned by Grupo ABC.

The client has its own website, which allows for online booking and receives traffic from several countries.

# The Business Questions

Identify the main customer segments and define market strategies to increase revenues generated.

# The Process

The process we followed can be splitted in 2 different parts:

1. *Part 1*: We describe the dataset and the different attributes to find **key customer descriptors** using *dimensionality reduction* techniques 

2. *Part 2*: We will use the results of this analysis to make business decisions about positioning depending on key purchase drivers we find at the end of this process.

3. *Part 3*: Final Recommendation

# The Data

First we load the data to use:



The data refer to the users and their interaction with the website until that time.
Overall the dataset contains information on ```{r, eval=true, echo=FALSE, tidy=TRUE} nrow(ProjectDataFactor)``` customers described by `{r} ncol(ProjectDataFactor) ` number of variables

Variables description: <br>
* Type of User: Returning or New Visitor <br>
* Source: Organic vs Paid Promotions (cpc, cpm, referral) <br>
* Users: Number of User Visits <br>
* Sessions: Number of User Sessions <br>
* No of Pages Visited: Number of total pages visited <br>
* No of Transactions: Number of monetary transactions <br>
* Revenue: Revenue generated <br>
* Dummy Variable: We created dummy variables categorical data such as 'user type', 'source' and 'device type'  <br>
  
# Part 1: Key Customer Characteristics




```r
factor_attributes_used <- intersect(factor_attributes_used, 1:ncol(myData))
ProjectDataFactor <- myData[,factor_attributes_used]
ProjectDataFactor <- as.matrix(ProjectDataFactor)
```

## Steps 1-2: Check the Data 


Start by some basic visual exploration of, say, a few data:


```r
rownames(ProjectDataFactor) <- paste0("Obs.", sprintf("%02i", 1:nrow(ProjectDataFactor)))
print(t(head(ProjectDataFactor, max_data_report)))
```

```
##                     Obs.01              Obs.02             
## Type.of.User        "Returning Visitor" "Returning Visitor"
## Source              "google / organic"  "google / organic" 
## Device              "desktop"           "desktop"          
## Returning.Visitor   "1"                 "1"                
## New.Visitor         "0"                 "0"                
## Google.CPC          "0"                 "0"                
## TripAdvisor         "0"                 "0"                
## Direct              "0"                 "0"                
## DFA                 "0"                 "0"                
## Email               "0"                 "0"                
## Referral            "0"                 "0"                
## Desktop             "1"                 "1"                
## Mobile              "0"                 "0"                
## Tablet              "0"                 "0"                
## Users               " 7240"             "21442"            
## Sessions            "10776"             "35015"            
## No.of.Pages.Visited " 45328"            "173192"           
## No.of.Transactions  " 64"               "130"              
## Revenue             "456710.54"         "362368.88"        
##                     Obs.03             
## Type.of.User        "New Visitor"      
## Source              "(direct) / (none)"
## Device              "desktop"          
## Returning.Visitor   "0"                
## New.Visitor         "1"                
## Google.CPC          "0"                
## TripAdvisor         "0"                
## Direct              "1"                
## DFA                 "0"                
## Email               "0"                
## Referral            "0"                
## Desktop             "1"                
## Mobile              "0"                
## Tablet              "0"                
## Users               " 2989"            
## Sessions            " 2996"            
## No.of.Pages.Visited "  7989"           
## No.of.Transactions  " 13"              
## Revenue             "341553.90"
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
```

Now that we have only numeric data, we can proceed with Factor Analysis.


```r
Variance_Explained_Table_results<-PCA(scaled_data, graph=FALSE)
Variance_Explained_Table<-Variance_Explained_Table_results$eig
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
This can be easily explained looking at the variables. Some variables are binary values conveying the same meaning as the text variables. 
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

To analyze different scenarios and identify possible strategies, we can focus on different behaviors across devices.


```r
averageTransaction <- aggregate(myData$No.of.Transactions,list(myData$Device), FUN=sum)
colnames(averageTransaction) <- c("Device","No of Transactions")
iprint.df(averageTransaction)
```

<!--html_preserve--><div class="formattable_container"><table class="table table-condensed">
 <thead>
  <tr>
   <th style="text-align:right;"> Device </th>
   <th style="text-align:right;"> No of Transactions </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> <span>desktop</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 100.00%">3553</span> </td>
  </tr>
  <tr>
   <td style="text-align:right;"> <span>mobile </span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 10.00%">119</span> </td>
  </tr>
  <tr>
   <td style="text-align:right;"> <span>tablet </span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 10.03%">120</span> </td>
  </tr>
</tbody>
</table></div><!--/html_preserve-->

We can clearly see that desktop is the main channel used by customers to access the website, generating about 95% of total transactions.
We can now focus on revenues to check if we find the same trend or a different behavios depending on the device.


```r
averageRevenue <- aggregate(myData$Revenue,list(myData$Device), FUN=sum)
colnames(averageRevenue) <- c("Device","Revenues")
iprint.df(averageRevenue)
```

<!--html_preserve--><div class="formattable_container"><table class="table table-condensed">
 <thead>
  <tr>
   <th style="text-align:right;"> Device </th>
   <th style="text-align:right;"> Revenues </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> <span>desktop</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 100.00%">8459779.1</span> </td>
  </tr>
  <tr>
   <td style="text-align:right;"> <span>mobile </span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 10.00%">291844.2</span> </td>
  </tr>
  <tr>
   <td style="text-align:right;"> <span>tablet </span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 10.20%">310228.0</span> </td>
  </tr>
</tbody>
</table></div><!--/html_preserve-->
Even an analysis on revenues shows that desktop is the most used device, as expected from previous result.
We can then analyze revenues per transaction to check profitability for each device.


```r
RevTransaction <- averageRevenue
RevTransaction[,2] <- averageRevenue[,2]/averageTransaction[,2] 
colnames(RevTransaction) <- c("Device","Revenue per Transaction")
iprint.df(RevTransaction)
```

<!--html_preserve--><div class="formattable_container"><table class="table table-condensed">
 <thead>
  <tr>
   <th style="text-align:right;"> Device </th>
   <th style="text-align:right;"> Revenue per Transaction </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> <span>desktop</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 10.00%">2381.024</span> </td>
  </tr>
  <tr>
   <td style="text-align:right;"> <span>mobile </span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 41.49%">2452.472</span> </td>
  </tr>
  <tr>
   <td style="text-align:right;"> <span>tablet </span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 100.00%">2585.233</span> </td>
  </tr>
</tbody>
</table></div><!--/html_preserve-->
We can see that revenues per transaction are quite stable cross-device, even though tablet is more profitable if compared to desktop and mobile devices.

Moving from devices to sources, we can then compare organic sources to payed sources (e.g. "cpc") and see if there is a strong relationship between transaction and revenues and different sources.


```r
averageRevenueSource <- aggregate(myData$Revenue,list(myData$Source), FUN=sum)
colnames(averageRevenueSource) <- c("Source","Revenues")
averageRevenueSource <- averageRevenueSource[order(averageRevenueSource[,2],averageRevenueSource[,1],decreasing=TRUE),]
rownames(averageRevenueSource) <- c(1:nrow(averageRevenueSource))
iprint.df(averageRevenueSource[1:10,])
```

<!--html_preserve--><div class="formattable_container"><table class="table table-condensed">
 <thead>
  <tr>
   <th style="text-align:right;"> Source </th>
   <th style="text-align:right;"> Revenues </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> <span>(direct) / (none)              </span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 100.00%">2951215.28</span> </td>
  </tr>
  <tr>
   <td style="text-align:right;"> <span>google / organic               </span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 95.07%">2793075.46</span> </td>
  </tr>
  <tr>
   <td style="text-align:right;"> <span>google / cpc                   </span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 53.69%">1466490.42</span> </td>
  </tr>
  <tr>
   <td style="text-align:right;"> <span>dfa / cpm                      </span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 21.36%">430017.88</span> </td>
  </tr>
  <tr>
   <td style="text-align:right;"> <span>dfa / cpc                      </span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 15.01%">226526.63</span> </td>
  </tr>
  <tr>
   <td style="text-align:right;"> <span>bing / organic                 </span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 13.75%">186316.63</span> </td>
  </tr>
  <tr>
   <td style="text-align:right;"> <span>email / email                  </span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 11.35%">109285.60</span> </td>
  </tr>
  <tr>
   <td style="text-align:right;"> <span>yahoo / organic                </span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 10.82%">92137.69</span> </td>
  </tr>
  <tr>
   <td style="text-align:right;"> <span>olehotels.com / referral       </span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 10.12%">69812.64</span> </td>
  </tr>
  <tr>
   <td style="text-align:right;"> <span>adquiramexico.com.mx / referral</span> </td>
   <td style="text-align:right;"> <span style="display: inline-block; direction: rtl; border-radius: 4px; padding-right: 2px; background-color: #EEEEEE; width: 10.00%">65988.00</span> </td>
  </tr>
</tbody>
</table></div><!--/html_preserve-->

Focusing on top 10 sources, we can see that direct access and organic searches on google or bing provide more than 4 times the revenues of payed sources like "cpc" and "cpm".

<hr>\clearpage

# Part 3: Recommendation

From previous analysis, we can suggest the following strategy:
* reduce spending in payed sources;
* provide app/mobile portal to facilitate the access from tablet;
* .....

