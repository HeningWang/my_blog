---
title: Statistische Methoden
date: 2020-04-30 21:37:16
tags:  [R, 统计]
categories: [语言学]
---
 Dozent: Dr. Fabian Schlotterbeck
 Deutsches Seminar, Universität Tübingen

<!--more-->  
load package and data frame

``` r
library(lme4)
```

    ## Loading required package: Matrix

``` r
library(lmerTest)
```

    ## 
    ## Attaching package: 'lmerTest'

    ## The following object is masked from 'package:lme4':
    ## 
    ##     lmer

    ## The following object is masked from 'package:stats':
    ## 
    ##     step

``` r
library(ggplot2)
load("data_zur_Bearbeitung.RData")
```

Zunächst werden die Faktoren und Faktorstufen definiert.

``` r
factor1 <- as.factor(c("level1.1","level1.2"))
factor2 <- as.factor(c("level2.1","level2.2"))
```

aggregate…

``` r
agg_data <- aggregate(rt~factor1+factor2+participant, data, mean)
```

Aufgabe 1 Werft einen Blick auf die Verteilungen der Reaktionszeiten in
den vier Bedingungen. Histogram aller Reaktionszeiten in den vier
Bedingungen

``` r
par(mfrow=c(2,2))
for (i in levels(factor1)){
  for (j in levels(factor2)){
    hist(data[data$factor1==i & data$factor2==j,]$rt, main = paste(i,j, sep=", "))
  }
}
```
{% asset_img unnamed-chunk-4-1.png %}


    ##Beschreibung der Aufgabe 1:
    Die vier Histogramme zeigen deutlich, wie die Reaktionszeiten in den vier Bedingungen verteilt sind. Auf der x-Achse sind die Reaktionszeiten angegeben, während auf der y-Achse die Häufigkeit aufgeführt ist. 
    Im Vergleich zu den anderen vier Bedingungen ist der Zahlenraum der Bedingung [level1.2, level2.2] am größten, nämlich von 0 bis 2500, danach kommt die Bedingung [level1.1, level2.1], dann die Bedingung [level1.1, level2.2] und zuletzt die Bedingung [level1.2, level2.1]. 
    Es lässt sich eine deutliche Tendenz erkennen: die meisten Reaktionszeiten in den ersten drei Diagrammen sind von 300 bis 700. Die Bedingung [level1.2, level2.2] ist etwas anders, der Großteil reagiert im Zeitraum von 800 bis 1200.
    Wie die y-Achse auch zeigt, ist die Häufigkeit der Bedingung [level1.1, level2.1] am höchsten und der Zahlenraum der Bedingung [level1.1, level2.1] ist am größten.

Aufgabe 2 Betrachtet eine simulierte Versuchsperson genauer. Histogram
der Reaktionszeiten des ersten Teilnehmers in den vier Bedingungen

``` r
sub1 <- subset(data, participant=="pp1")
par(mfrow=c(2,2))
for (i in levels(factor1)){
  for (j in levels(factor2)){
    hist(sub1[sub1$factor1==i & sub1$factor2==j,]$rt, main = paste(i,j, sep=", "))
  }
}
```

{% asset_img unnamed-chunk-5-1.png %}

Quantil der Reaktionszeiten des ersten Teilnehmers in den vier
Bedingungen

``` r
par(mfrow=c(2,2))
for (i in levels(factor1)){
  for (j in levels(factor2)){
    qqnorm(sub1[data$factor1==i & data$factor2==j,]$rt, main = paste(i,j, sep=", "))
    qqline(sub1[data$factor1==i & data$factor2==j,]$rt, main = paste(i,j, sep=", "))
  }
}
```

{% asset_img unnamed-chunk-6-1.png %}

    Anhand der Form des Histograms und Quantils ist es deutlich, dass die Verteilung grob der Normalverteilung folgt.

    Beschreibung der Aufgabe 2:
    In der Bedingung [level1.1, level2.1]:
    µ = 615.4299
    σ = 202.0015
    In der Bedingung [level1.1, level2.2]:
    µ =419.1368
    σ = 206.9496
    In der Bedingung [level1.2, level2.1]:
    µ =377.204
    σ = 208.7926
    In der Bedingung [level1.2, level2.2]:
    µ =913.4193
    σ = 266.4935
    Wir nennen pp1 als simulierte Versuchsperson. Wie das Schaubild darstellt, folgt die Reaktionszeit der Bedingung [level1.1, level2.1] der Normalverteilung. Der Mittelwert ist 615.4299, es entspricht dem Diagram.
    Die Standardabweichung der Bedingung 1 ist fast gleich wie die der Bedingung 2 und Bedingung 3, deshalb sehen die Verteilungen von diesen drei Bedingungen ähnlich aus.
    Bei der Bedingung [level1.2, level2.2] ist alles anders. der Mittelwert ist 913.4193, er ist viel größer als die Mittelwerte der anderen 3 Bedingen. Obwohl die Reaktionszeiten der Bedingung 4 fast der Normalverteilung folgen, ist die Standardabweichung ein bisschen größer als die der anderen drei.

Aufgabe 3 Bildet für alle Versuchspersonen die Mittelwerte der
Reaktionszeiten in den vier Bedingungen.

``` r
aggregate(rt~factor1+factor2, data, mean)
```

    ##    factor1  factor2        rt
    ## 1 level1.1 level2.1  605.0275
    ## 2 level1.2 level2.1  361.4941
    ## 3 level1.1 level2.2  357.7497
    ## 4 level1.2 level2.2 1048.5678

    Beschreibung der Aufgabe 3:
    Die Tabelle zeigt die Mittelwerte der Reaktionszeiten in den vier Bedingungen für alle Versuchspersonen. 
    Aus dieser Tabelle ist ersichtlich, dass die Mittelwerte der Bedingung [level1.2, level2.1] und der Bedingung [level1.1, level2.2] fast gleich sind, nämlich 361.4941 und 357.7497. 
    Es fällt auf, dass der Mittelwert von der Bedingung 4 1048.5678 ist, fast dreimal so viel wie die in den zweiten und dritten Bedingungen. Der Wert von der Bedingung [level1.1, level2.1] (605.0275) ist auch größer als die der anderen drei.

Aufgabe 4 Beschreibt die Verteilungen der Versuchspersonenmittelwerte in
den vier Bedingungen. Da die Verteilung der Reaktionszeiten der
Normalverteilung ähnelt, verwenden wir Mittelwert als Lagemaße und
Standard Derivation als Streuungsmaße. Lage- und Streuungsmaße von
Bedingung 1

``` r
Bedingung_1 <- subset(agg_data, factor1=="level1.1" & factor2=="level2.1")
Bedingung_1.means<-round(mean(Bedingung_1$rt),4)
Bedingung_1.sd <- round(sd(Bedingung_1$rt),4)
Bedingung_1.se <- round(Bedingung_1.sd/sqrt(60),4)
paste("Bedingung_1.means=",Bedingung_1.means)
```

    ## [1] "Bedingung_1.means= 605.0275"

``` r
paste("Bedingung_1.sd=",Bedingung_1.sd)
```

    ## [1] "Bedingung_1.sd= 31.5911"

Lage- und Streuungsmaße von Bedingung 2

``` r
Bedingung_2 <- subset(agg_data, factor1=="level1.1" & factor2=="level2.2")
Bedingung_2.means <- round(mean(Bedingung_2$rt),4)
Bedingung_2.sd <- round(sd(Bedingung_2$rt),4)
Bedingung_2.se <- round(Bedingung_2.sd/sqrt(60),4)
paste("Bedingung_2.means=",Bedingung_2.means)
```

    ## [1] "Bedingung_2.means= 357.7497"

``` r
paste("Bedingung_2.sd=",Bedingung_2.sd)
```

    ## [1] "Bedingung_2.sd= 41.9532"

Lage- und Streuungsmaße von Bedingung 3

``` r
Bedingung_3 <- subset(agg_data, factor1=="level1.2" & factor2=="level2.1")
Bedingung_3.means <- round(mean(Bedingung_3$rt),4)
Bedingung_3.sd <- round(sd(Bedingung_3$rt),4)
Bedingung_4.se <- round(Bedingung_3.sd/sqrt(60),4)
paste("Bedingung_3.means=",Bedingung_3.means)
```

    ## [1] "Bedingung_3.means= 361.4941"

``` r
paste("Bedingung_3.sd=",Bedingung_3.sd)
```

    ## [1] "Bedingung_3.sd= 33.524"

Lage- und Streuungsmaße von Bedingung 4

``` r
Bedingung_4 <- subset(agg_data, factor1=="level1.2" & factor2=="level2.2")
Bedingung_4.means <- round(mean(Bedingung_4$rt),4)
Bedingung_4.sd <- round(sd(Bedingung_4$rt),4)
Bedingung_4.se <- round(Bedingung_4.sd/sqrt(60),4)
paste("Bedingung_4.means=",Bedingung_4.means)
```

    ## [1] "Bedingung_4.means= 1048.5678"

``` r
paste("Bedingung_4.sd=",Bedingung_4.sd)
```

    ## [1] "Bedingung_4.sd= 82.6853"

    Beschreibung der Aufgabe 4:
    Da die Verteilung der Reaktionszeiten der Normalverteilung ähnelt, verwenden wir Mittelwert als Lage Maße und Standard Derivation als Streuungsmaße. Weil die Mittelwerte schon in Aufgabe 3 beschrieben wurden, legen wir hier mehr Wert auf die Standard Derivation.
    Die Standard Derivation der Bedingungen [level1.1, level2.1], [level1.2, level2.2] und [level1.1, level2.2] sind fast gleich, sie liegen im Raum 31 bis 42. Das heißt, die Graden der Korrelation der Anzahlen in den ersten drei Bedingungen sind ziemlich ähnlich. Die Standard Derivation der Bedingungen [level1.2, level2.2] (82.6853) ist im Vergleich zu der von anderen Bedingungen sehr groß. Laut diesem Wert ist es möglich, dass die Statistiken der Bedingung 4 unregelmäßig sind. Es kann auch sein, dass die Schätzfunktion bei Bedingung 4 nicht gut funktioniert.

Aufgabe 5 Berechnet Konfidenzintervalle für die
Versuchspersonenmittelwerte in den vier Bedingungen.

``` r
Liste_aller_vier_Bedin. <- list(Bedingung_1,Bedingung_2,Bedingung_3,Bedingung_4)
t.test(Bedingung_1$rt)
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  Bedingung_1$rt
    ## t = 148.35, df = 59, p-value < 2.2e-16
    ## alternative hypothesis: true mean is not equal to 0
    ## 95 percent confidence interval:
    ##  596.8666 613.1883
    ## sample estimates:
    ## mean of x 
    ##  605.0275

``` r
upper <- rep(NA,4)
lower <- rep(NA,4)

for (i in 1:4) {
  lower[i] <- round(t.test(Liste_aller_vier_Bedin.[[i]]$rt)$conf.int[1],4)
  upper[i] <- round(t.test(Liste_aller_vier_Bedin.[[i]]$rt)$conf.int[2],4)
}
cis <- cbind(lower,upper)
cis
```

    ##          lower     upper
    ## [1,]  596.8666  613.1883
    ## [2,]  346.9121  368.5874
    ## [3,]  352.8339  370.1543
    ## [4,] 1027.2079 1069.9277

    Beschreibung der Aufgabe 5:
    In der 5. Aufgabe wird ein einseitiger T-Test verwendet, um die Konfidenzintervalle (LB auf der linken Seite der Tabelle und UB auf der rechten Seite) in allen 4 Bedingungen auszurechnen. Hier fällt auf, dass die Spannweite der KIs je nach Bedingung unterschiedlich ist. Der Grund dafür liegt daran, dass wir stets verschiedene Stichprobenmittelwerte und Standardabweichungen haben . 
    In [1,] und [3,] sind die Konfidenzintervalle am engsten (613.1883-596.86664=~16.3 für [1,] und 370.1543-352.8339= ~17.3 für [3,]). Dies hängt von der Stichprobengröße und der Variation ab, d.h. in [1,] liegen LB und UB am engsten beieinander, weil der Variant kleiner wird. Daher lässt sich auch sagen, dass Bedingung [2,] und [4,] ein breiteres Konfidenzintervall haben und eine größere Varianz (368.5874-346.9121= ~21.7 für [2,] und 1069.9277-1027.2079= ~42,7 für [4,]). Laut unserem T-test liegen LB und UB bei Bedingung [4,] am allerweitesten voneinander. 

Aufgabe 6

``` r
summary(lmer(rt~factor1*factor2+(1|participant), data = agg_data))
```

    ## Linear mixed model fit by REML. t-tests use Satterthwaite's method [
    ## lmerModLmerTest]
    ## Formula: rt ~ factor1 * factor2 + (1 | participant)
    ##    Data: agg_data
    ## 
    ## REML criterion at convergence: 2547.2
    ## 
    ## Scaled residuals: 
    ##     Min      1Q  Median      3Q     Max 
    ## -3.1900 -0.5747  0.0101  0.5876  3.8684 
    ## 
    ## Random effects:
    ##  Groups      Name        Variance Std.Dev.
    ##  participant (Intercept)  197.3   14.05   
    ##  Residual                2482.4   49.82   
    ## Number of obs: 240, groups:  participant, 60
    ## 
    ## Fixed effects:
    ##                                 Estimate Std. Error       df t value Pr(>|t|)
    ## (Intercept)                      605.027      6.683  232.222   90.53   <2e-16
    ## factor1level1.2                 -243.533      9.096  177.000  -26.77   <2e-16
    ## factor2level2.2                 -247.278      9.096  177.000  -27.18   <2e-16
    ## factor1level1.2:factor2level2.2  934.351     12.864  177.000   72.63   <2e-16
    ##                                    
    ## (Intercept)                     ***
    ## factor1level1.2                 ***
    ## factor2level2.2                 ***
    ## factor1level1.2:factor2level2.2 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Correlation of Fixed Effects:
    ##             (Intr) fc11.2 fc22.2
    ## fctr1lvl1.2 -0.681              
    ## fctr2lvl2.2 -0.681  0.500       
    ## fc11.2:22.2  0.481 -0.707 -0.707

    Beschreibung der Aufgabe 6:
    Ein gemischtes lineares Modell der aggregierten Daten wird in der 6. Aufgabe berechnet und die Nullhypothese getestet. Was die fixen Effekten angeht, sind alle Variablen (sowohl bei dem Interzept als auch bei factor1level1.2 und factor2level2.2) hochsignifikant (***=<0.001). Außerdem liegt eine hochsignifikante Interaktion zwischen den beiden Faktoren vor. Pr(>|t|) wird hier als p-Wert interpretiert. Da der p-Wert bei allen Variablen erheblich weniger als 0.05 auftaucht, verwerfen wir die Hypothese, dass β=0 sei und neigen der Alternativenhypothese zu, dass β nicht gleich 0 sei. Die T-Werte bei dem Interzept und factor1level1.2:factor2level2.2 sind größer als 2 und signifikant. Die Variablen factor1level1.2 und factor2level2.2 haben dagegen einen negativen T-Wert. 

Aufteilung der Aufgaben: Hening Wang ist zuständig fürs Programieren des
R-Codes und Erstellen und Zusammenfassen der RMD-Datei. Yuanyuan Hou ist
zuständig für die Beschreibung der Aufgabe 1-4. Ingibjörg
Steingrímsdóttir ist zuständig für die Beschreibung der Aufgabe 5-6.
