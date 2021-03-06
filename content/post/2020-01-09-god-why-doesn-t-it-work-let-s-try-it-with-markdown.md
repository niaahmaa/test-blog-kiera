---
title: god why doesn't it work? Let's try it with markdown
author: David Lim
date: '2020-01-09'
slug: dX

categories:
  - cats
  - are
  - gay
tags:
  - tags
  - are
  - gay
  
images: []
---

#What would happen if I add a hastag here? 

##More?

###Moreee?

testing testing more and more words. 

The last theme failed because idk where to find my posts. Hopefully I can find it this time.

I am not sure how things works when writing I press enter

``` r
Moving into a RMD script

#Set sample size
T = 1000
#Create vector y to store simulated values
y <- c()
y[1] = 0
y[2] = 0
#Set parameter values
phi1 = 0.4
phi2 = 0.6
c = 0
#Simulate AR(2) process
set.seed(123)
for (i in 3:T) {
  y[i] = c + phi1*y[i-1]+ phi2*y[i-2]+rnorm(1, mean = 0, sd = 0.2)
}

AR2fit <- ar.ols(y, order.max = 2)
plot(AR2fit)
```
```{r setup, include = FALSE}
knitr::opts_chunk$set(echo = FALSE)

#loading libraries
library(fpp2)
library(urca)
library(dynlm)
library(portes)
library(vars)

```


```{r import data, include = FALSE}

#importing dataset
setwd("C:/Users/David/Desktop/Monash/2019 S1/ETW 3450/asg1")
getwd()

data <- read.csv("MsiaData.csv")
df <- ts(data [,-1], frequency = 4, start =1999)

#check imported dataset
str(df)
head(df)
tail(df)


```


```{r data preperation, include = FALSE}

#seperating the 3 variables and label
gdp <- df[,"GDP"]
hp <- df[,"HP"]
klse <- df[,"KLSE"]

#converting the variables into log
lgdp = log(gdp)
lklse = log(klse)
lhp = log(hp)

#combine all the log variables into a dataframe
df <- cbind(lgdp,lklse,lhp)

#check dataset
head(df)
tail(df)


```


#QUESTION 1

#Trend and Order of Integration (Informal method)

i)presence of trend

```{r lkse data, include = FALSE, fig.width= 8} 

#visualisation of the original series
autoplot(lklse, main = "Graph of Log KLSE", xlab = "year", ylab ="KLSE Index")

```


```{r lkse data 2, include = TRUE, fig.width = 8} 
#view Acf and Pacf
#slow decay in the acf and significant spike in the pacf is an indication of a trending series
ggtsdisplay(lklse,  main = "Time Series , ACF and PACF of lklse")

```

After taking a long transformation of the series KLSE , the series seems to have a trend. The presence of trend could be a deterministic trend, a stochastic trend or presence of both. Besides, the slow decay of autocorrelation in the SACF from an initial value close to one and a significant spike at lag one (close to 1) in the SPACF further evidence that LKLSE is a non-stationary process. By visualising the overall series, a deterministic trend could be present in the series as the series increases in a deterministic way over the years. Further analysis which is by detrending the series allows us to determine whether there is a presence of deterministic trend.  


```{r lkse data 3, include = FALSE, fig.width= 8}
ggAcf(lklse, main = "ACF of lklse")

#detrend the series
dt_lklse <- tslm(lklse ~ trend)


#extract residuals of the detrended series
resid_dt <- resid(dt_lklse)
```



```{r lkse data 4, include = TRUE, fig.width= 8} 
#Plot Acf, Pacf, Original series
#not mean reverting enough even after detrending, might be presence of stochastic trend 
ggtsdisplay(resid_dt, main = "Time Series , ACF and PACF of the detrended series (lklse)")


```

After detrending the series, there is a slow decay of autocorrelation in the SACF and significant spike in lag one and 2 respectively. Notably that the first significant spike in SPACF is relatively not so close to the value of one. However, if the series were to be partition into a few section for inspection, the series does not have a mean-reverting behaviour. This is an indication of a non-stationary series as the series LKLSE have violated the constant mean assumption in order for a series to be stationary. In addition the SACF and SPACF is not a white noise series which is an indication that the series is not a trend stationary process. From the above procedures that has been conducted, it is plausible to infer that the series is not a trend stationary series because detrending the series does not make LKLSE a stationary series.

 
\pagebreak

ii)order of integration

```{r first diff, include = FALSE, fig.width= 8}

#take a first difference of lklse
dlklse <- diff(lklse,1)

#plot the difference series
autoplot(dlklse, main = "Time series of first difference series", ylab ="LKLSE Index", xlab = "Years")

```


```{r first diff 2, include = TRUE, fig.width= 8}
#Plot Acf, Pacf, Differenced series
ggtsdisplay(dlklse, main = "Time Series , ACF and PACF of the differenced series (lklse)")


```

After taking a first differenced, the series is now stationary as it illustrates a mean reverting behaviour with a zero mean. The SACF shows a slight sinusoidal pattern but not slow decay of the autocorrelation and the SPACF has significant spikes at lag one and four but it does not shows a large and positive significant spike which is close to one at lag one. A stationary series allows for autocorrelation thus the differenced series need not be a white noise series after differencing. As the number of times taken to make the series stationary is once, it indicates that there is presence of one unit root and it also means that the order of integration is one. Thus, the tentative conclusion is, LKLSE is an I(1) process.


#Conclusion
A series is stationary after detrending is a trend stationary process while a series becomes stationary after differencing is a differenced stationary process. A differenced stationary process cannot be made stationary by detrending.The tentative conclusion is that LKLSE is an I(1) process.

\pagebreak

#Some comments

Kuala Lumpur Stock Exchange (KLSE) is a company limited that incorporated under the Companies Act 1966 and is guarantee without a share capital. It was established in March 1960 to provide a market place for raising new fund for the listed companies and investors. In addition, it is also a platform for buyers and sellers who determine share prices competitively to trade outstanding shares (Abdullah, M.S, 1996). In a stock market, the random shock caused by the volatility and fluctuating of the market should not be conflated with pure deterministic trend in long-term. The reason behind that the real value of the stock market drifts upwards overtime in a predictable way is due to the improvement of technological progress (Greenwald, D., Lettau, M., & Ludvigson, S. ,2015). The deterministic trend has drive output per capita and average standard of living upwards overtime. It is instead the shocks , the peak and trough around the trend that we have minimal knowledge in it. With the presence of random shocks, market can be persistently displace from its long-term trend for several decades (Greenwald, D., Lettau, M., & Ludvigson, S. ,2015).This might be part of the reason whereby we sometimes observe the presence of "deterministic trend" during the data visualisation process but whether or not there is a deterministic trend in the series, further analysis need to be conducted. In addition, possibilities that contributes to the departure from random walk might be due to the non-linear serial dependencies in the underlying DGP. However, Annuar et al. (1991) uses the unit root methodology to account for cyclicity in price series and holding thin trading effect constant to conduct a weak-form test on stocks which were continuously traded on KLSE. It is evidence that approximately 87% of the total sample possess a unit root  (Lim, K., Wong, H., & Liew, V, 1993). Prices series on KLSE that possess unit roots also indicates that the current prices are the best estimates for future prices (Nassir, Ariff & Mohamad, 1993).

\pagebreak


r

Now I am back