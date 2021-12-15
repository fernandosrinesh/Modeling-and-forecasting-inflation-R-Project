Modeling and forecasting inflation in Sri Lanka using ARIMA models

Zhaoshuai Wang / Srinesh Heshan Fernando 2021/6/4

***INTRODUCTION***

This study is based on a data set of annual rates of inflation in Sri Lanka (LK) ranging over the period 1960 – 2019. All the data was adapted from the World Bank data sources. We will use ARIMA model to forecast the future rates of inflation.

***Load the data***

**l i brary**( zoo)![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.001.png)

##![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.002.png)

\## At t achi ng package: ' zoo'

\## The f ol l owi ng obj ect s ar e mas ked f r om ' package: bas e' : ![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.003.png)##

\## as . Dat e, as . Dat e. numer i c

**l i brary**( f or ecas t )![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.004.png)

\## Regi s t er ed S3 met hod over wr i t t en by ' quant mod' : ![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.005.png)## met hod f r om

\## as . zoo. dat a. f r ame zoo

**l i brary**( ggpl ot 2)![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.006.png)

**l i brary**( t s er i es )

s et wd( "C: / Us er s / wangz/ Des kt op")

md <- r ead. cs v( "dat a. cs v", header = TRUE)

SL<- md[ md$Count r y. Name == ' Sr i Lanka' , ]

CN<- md[ md$Count r y. Name == ' Chi na' , ]

SL <- SL[ , ! names ( SL) %**i n**% c( "Count r y. Name", "Count r y. Code", "I ndi cat or . Name", "I ndi cat or . Code") ] SL <- t ( SL)

r ownames ( SL) <- c( 1960: 2020)

col names ( SL) <- c( "i nf l at i on r at es ")

***Converting data to time series format (TS)***

Sr i \_Lanka <- t s ( SL, s t ar t =1960, f r equency=1) ![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.007.png)s t r ( Sr i \_Lanka)

\## Ti me- Ser i es [ 1: 61, 1] f r om 1960 t o 2020: - 1. 54 1. 13 1. 5 2. 27 3. 2 . . . ![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.008.png)## - at t r ( \*, "di mnames ") =Li s t of 2

\## . . $ : NULL

\## . . $ : chr "i nf l at i on r at es "

***View trend chart***

pl ot . t s ( Sr i \_Lanka)![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.009.png)

![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.010.jpeg)

***Check if Stationary***

The augmented Dickey Fuller (ADF) test can be used to test whether a time series is stationary or not. The Ho is that the sequence is nonstationary.

ADF test:

adf . t es t ( Sr i \_Lanka, al t er nat i ve="s t at i onar y")![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.011.png)

##![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.012.png)

\## Augment ed Di ckey- Ful l er Tes t

\##

\## dat a: Sr i \_Lanka

\## Di ckey- Ful l er = - 2. 6328, Lag or der = 3, p- val ue = 0. 3189 ## al t er nat i ve hypot hes i s : s t at i onar y

As the p-value is higher than 0.05, so we can not reject the Ho, which means the sequence is nonstationary.

So we have to change it into a stationary sequence by differential processing. Here, the ndiffs() function is used to judge the difference degree: ***Judge the difference degree***

ndi f f s ( Sr i \_Lanka) ##![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.013.png) [ 1] 1![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.014.png)

The result of the function is the first-order difference, let’s do 1st difference of the data and check ACF/PACF: ***1st difference/ ACF and PACF***

Sr i \_Lankadi f f <- di f f ( Sr i \_Lanka)![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.015.png)

pl ot . t s ( Sr i \_Lankadi f f , mai n="1s t di f f er ence")

![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.016.jpeg)

As we could see the sequence become stationary. Then use 2 different way to check ACF and PACF:

t s di s pl ay( Sr i \_Lankadi f f , xl ab="year ", yl ab="i nf l at i on r at es i ndex")![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.017.png)

![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.018.jpeg)

acf ( Sr i \_Lankadi f f )![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.019.png)

![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.020.jpeg)

pacf ( Sr i \_Lankadi f f )![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.021.png)

![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.022.jpeg)

As we could see the result of ACF and PACF, between the two blue dashed lines can be regarded as 0. There is no outlier for both ACF and PACF. So we could select ARIMA(1,1,1), the difference order is 1, the autoregressive order is 1, and the smooth moving order is 1.

***Model fitting*** Then we do Model fitting:

pr e<- ar i ma( Sr i \_Lanka, or der =c( 1, 1, 1) ) ![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.023.png)pr e

##![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.024.png)

\## Cal l :

\## ar i ma( x = Sr i \_Lanka, or der = c( 1, 1, 1) )

\##

\## Coef f i ci ent s :

\## ar 1 ma1

\## 0. 2977 - 0. 8213

\## s . e. 0. 1639 0. 0941

\##

\## s i gma^2 es t i mat ed as 24. 5: l og l i kel i hood = - 181. 42, ai c = 368. 84

aut o. ar i ma( Sr i \_Lanka, s eas onal =F)![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.025.png)

\## Ser i es : Sr i \_Lanka![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.026.png)

\## ARI MA( 1, 1, 1)

\##

\## Coef f i ci ent s :

\## ar 1 ma1

\## 0. 2977 - 0. 8213

\## s . e. 0. 1639 0. 0941

\##

\## s i gma^2 es t i mat ed as 25. 34: l og l i kel i hood=- 181. 42 ## AI C=368. 84 AI Cc=369. 27 BI C=375. 12

From first result, we could get AIC=368.84, and from second on, we get AIC=368.84 AICc=369.27 BIC=375.12. (AIC value is the standard to judge whether the model fits well or not, the smaller the better)

Also ,we could use Auto.ARIMA () in forecast can directly select the best fitting model, it is same with our choice.

ar i ma1<- aut o. ar i ma( Sr i \_Lanka, t r ace=T)![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.027.png)

##![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.028.png)

\## ARI MA( 2, 1, 2) wi t h dr i f t : 374. 8459

\## ARI MA( 0, 1, 0) wi t h dr i f t : 382. 4603

\## ARI MA( 1, 1, 0) wi t h dr i f t : 378. 6245

\## ARI MA( 0, 1, 1) wi t h dr i f t : 372. 3281

\## ARI MA( 0, 1, 0) : 380. 3496

\## ARI MA( 1, 1, 1) wi t h dr i f t : 371. 3853

\## ARI MA( 2, 1, 1) wi t h dr i f t : 373. 6836

\## ARI MA( 1, 1, 2) wi t h dr i f t : 373. 7145

\## ARI MA( 0, 1, 2) wi t h dr i f t : 371. 5328

\## ARI MA( 2, 1, 0) wi t h dr i f t : 378. 1811

\## ARI MA( 1, 1, 1) : 369. 2703

\## ARI MA( 0, 1, 1) : 370. 223

\## ARI MA( 1, 1, 0) : 376. 4478

\## ARI MA( 2, 1, 1) : 371. 4792

\## ARI MA( 1, 1, 2) : 371. 5118

\## ARI MA( 0, 1, 2) : 369. 4031

\## ARI MA( 2, 1, 0) : 375. 9293

\## ARI MA( 2, 1, 2) : 372. 5652 ##

\## Bes t model : ARI MA( 1, 1, 1)

***Forecast according to the selected model***

Forecast the value in the next 6 years and set up 95% confidence interval:

i nf l at i on<- f or ecas t ( pr e, h=6, l evel =c( 99. 5) ) i![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.029.png) nf l at i on

\## Poi nt For ecas t Lo 99. 5 Hi 99. 5 ![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.030.png)## 2021 5. 810083 - 8. 083320 19. 70349 ## 2022 5. 707731 - 9. 681229 21. 09669 ## 2023 5. 677265 - 10. 342735 21. 69727 ## 2024 5. 668197 - 10. 798029 22. 13442 ## 2025 5. 665498 - 11. 192988 22. 52398 ## 2026 5. 664694 - 11. 565243 22. 89463

***Model evaluation***

1. Judge whether the prediction error is a normal distribution with zero mean and constant variance

Plot it first:

aut opl ot ( pr e$r es i dual s )![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.031.png)

![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.032.jpeg)

Then we make a function to Convert to normal distribution:

pl ot For ecas t Er r or s <- **f unct i on**( f or ecas t er r or s )![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.033.png)

{

- make a r ed hi s t ogr am of t he f or ecas t er r or s :

mys d <- s d( f or ecas t er r or s )

hi s t ( f or ecas t er r or s , col ="r ed", f r eq=FALSE)

- f r eq=FALSE ens ur es t he ar ea under t he hi s t ogr am = 1
- gener at e nor mal l y di s t r i but ed dat a wi t h mean 0 and s t andar d devi at i on mys d

mynor m <- r nor m( 10000, mean=0, s d=mys d)

myhi s t <- hi s t ( mynor m, pl ot =FALSE)

- pl ot t he nor mal cur ve as a bl ue l i ne on t op of t he hi s t ogr am of f or ecas t er r or s : poi nt s ( myhi s t $mi ds , myhi s t $dens i t y, t ype="l ", col ="bl ue", l wd=2)

}

pl ot For ecas t Er r or s ( pr e$r es i dual s )

![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.034.jpeg)

Get the above normal distribution map, we could say, the normality test: pass.

2. Then we carry out the independence test of residual

acf ( pr e$r es i dual s )![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.035.png)

![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.036.jpeg)

Box. t es t ( pr e$r es i dual s , l ag=1, t ype="Lj ung- Box")![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.037.png)

##![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.038.png)

\## Box- Lj ung t es t

\##

\## dat a: pr e$r es i dual s

\## X- s quar ed = 8. 595e- 06, df = 1, p- val ue = 0. 9977

From the result, we could see p-value = 0.9977>0.05, so we have to reject the hypothesis Ho, which means that the residual is considered to be independent.

Both tests are passed, so we could say our model seems good.

aut opl ot ( i nf l at i on)![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.039.png)

![](Aspose.Words.fa51f5a2-2a73-4458-98e3-f4c8168038b7.040.jpeg)

***Conclusion***

In this paper, we did the time series analysis and prediction.From ACF and PACF,we could get the final ARIMA(1,1,1) model, and our prediction basically conforms to the trend law of data.
