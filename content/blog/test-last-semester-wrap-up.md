+++
author = "Larissa"
categories = ["R", "Paper"]
tags = ["simulation", "first-differences", "ols", "regression", "fall2017", "jupyter-notebook"]
date = "2017-12-09"
description = "Data Essay - Lecture Quantitative Methods - Fall Semester 2017"
featured = "output_15_3_2.jpg"
featuredpath = "date"
linktitle = ""
title = "Investigating Number of Speeches in Parliament"
type = "post"

+++

The purpose of this data essay is to examine whether different electoral regimes affect the
participation of members of parliament (MP) in parliamentary debates. The task was to
empirically test two hypotheses that claim opposing effects of ideological distance on the
number of speeches delivered by MPs in proportional and majoritarian systems.

**Hypothesis 1** *In majoritarian systems, the larger the distance between an MP’s ideological position to the party, the more speeches the MP delivers in parliament.*
**Hypothesis 2** *In proportional systems, the larger the distance between MP’s ideological
position to the party, the fewer speeches the MP delivers in parliament.*

### Overview
- Setting up the Working Environment
- Getting a Feeling for the Data
- Basic and Extended Models
- Simulating the Differences between Countries
- Modelling the UK Case
- Modelling the German Case
- Simulating the Differences between Parties in Germany


```R
rm(list = ls())
#setwd("C:/...") # be sure to set the right working directory
library(MASS)
library(foreign)
library(ggplot2)
library(hydroGOF)
library(stargazer)
```

    Warning message:
    "package 'ggplot2' was built under R version 3.4.2"Warning message:
    "package 'hydroGOF' was built under R version 3.4.3"Loading required package: zoo
    Warning message:
    "package 'zoo' was built under R version 3.4.2"
    Attaching package: 'zoo'
    
    The following objects are masked from 'package:base':
    
        as.Date, as.Date.numeric
    
    
    Please cite as: 
    
     Hlavac, Marek (2015). stargazer: Well-Formatted Regression and Summary Statistics Tables.
     R package version 5.2. http://CRAN.R-project.org/package=stargazer 
    
    


```R
setwd("C:/Users/laris/Desktop/MMDS/Semester 2 - HWS2017/Multivariate Analyses/Data Essay")
```

The Dataset contains two data sets, one for Germany (2005-2009) and one for the
UK (2001-2005). The data sets contain information on the number of legislative speeches
for a subset of the MPs, 169 in the UK and 209 in Germany. Both data sets contain
a measurement of each MP’s distance to the position of the party leaders. The data
sets further include party affiliation and if the MP is one of the party leaders. In the
German case the data set includes further variables on whether a candidate was elected
via the party list or as a district candidate, committee assignments and where a MP is
ideologically located in comparison to the governing coalition


```R
# LOADING THE DATA AND GETTING A FEELING FOR IT

dta <- load("Dataessay.Rdata")
#summary(GERdata)
#summary(UKdata)

# RE-CODING SOME OF THE VARIABLES
GERdata$FDP <- ifelse(GERdata$party_affiliation == "FDP", 1, 0)
GERdata$CDU_CSU <- ifelse(GERdata$party_affiliation == "CDU/CSU", 1, 0)
GERdata$SPD <- ifelse(GERdata$party_affiliation == "SPD", 1, 0)
GERdata$Linke <- ifelse(GERdata$party_affiliation == "DIE LINKE.", 1, 0)

GERdata$ingov <- GERdata$SPD + GERdata$CDU_CSU

GERdata$list_candidate_new <- ifelse(GERdata$list_candidate == "District", 0, 1)
```

## Descriptive Statistics

The variable, which is going to be dependent in our models to test the given hypotheses,
counts the number of speeches of selected members of parliaments (MP) during an
election period. The data was collected for Germany and the United Kingdom. When we
look at the distribution of this (obviously) count data (Figure 1), we see its fast falling
curve and its long tail. This pattern is typical for a so called Poisson Distribution or a
Negative Binomial Distribution (we will test later, what specific kind of distribution we
find here).


```R
#stargazer(GERdata)
#stargazer(UKdata)

hist(GERdata$number_speeches, 
     main = "Frequency of Number of Speeches (Count Data) \n 
     in Germany", 
     xlab = "Number of Speeches (counted per MP)")
hist(UKdata$number_speeches, 
     main = "Frequency of Number of Speeches (Count Data) \n 
     in the United Kingdom",
     xlab = "Number of Speeches (counted per MP)")

#hist(GERdata$ideological_distance)
#hist(UKdata$ideological_distance)
```


![png](/img/2017/12/output_6_0.png)



![png](/img/2017/12/output_6_1.png)


In this figure we also see immediately the different ranges of number of speeches in Germany and the UK: While in Germany the parliament member with the highest number
of speeches talked 68 times, in UK we observed a maximum number of 711 speeches (see
also the Appendix Overview: Provided Variables for a more details). Therefore, and
from now on, we will investigate the variable dependencies for both countries separately.

## Model Selection
### Underlying Distribution

First, we will test whether the pattern in the count data shows a poisson or a negative
binomial distribution. They differ in the value of α, what is 0 in the poisson distribution.
This difference would affect our estimates, because they are based on the assumption of
an underlying distribution. The result of the Likelihood-Ratio-Test between the poisson
model and the respective negative binomial model shows, that we can at least at a 95%-
confidence be sure to reject the H0 that α equals 0. The following models will therefore
be build upon the assumption of an underlying negative binomial distribution.


```R
# BASIC MODELS AND LIKELIHOOD-RATIO-TEST

# BASIC POISSON MODEL
m0a <- glm(number_speeches ~ ideological_distance, 
           data = GERdata, family = "poisson")
m0b <- glm(number_speeches ~ ideological_distance, 
           data = UKdata, family = "poisson")

# BASIC NEGATIVE BINOMIAL MODEL
m1a <- glm.nb(number_speeches ~ ideological_distance, 
              data=GERdata, control = glm.control(maxit=100))
m1b <- glm.nb(number_speeches ~ ideological_distance, 
              data=UKdata, control = glm.control(maxit=100))
```


```R
# FUNCTION: LIKELIHOOD-RATIO-TEST
compute.lrt <- function(model.nb, model.po, sign.level){
  L1 <- logLik(model.po) 
  L2 <- logLik(model.nb) 
  LRT <- -2*L1 + 2*L2
  return(LRT > qchisq(sign.level, df = 1))
}

compute.lrt(m1a, m0a, 0.95) # TRUE --> negative binomial model is better!
```


TRUE



```R
# EXPANDED MODELS
m2a <- glm.nb(number_speeches ~ party_leader + 
              ideological_distance,
              data=GERdata, 
              control = glm.control(maxit=100))
m2b <- glm.nb(number_speeches ~ party_leader + 
              ideological_distance, 
              data=UKdata, 
              control = glm.control(maxit=100))

#stargazer(m1a, m1b, m2a, m2b)
```


```R
anova(m1a, m2a)
anova(m1b, m2b)

newGER <- GERdata
newGER$m1a <- predict(m1a, newGER, type = "response")
newGER$m2a <- predict(m2a, newGER, type = "response")

rmse(newGER$m1a, GERdata$number_speeches)
rmse(newGER$m2a, GERdata$number_speeches)

newUK <- UKdata
newUK$m1b <- predict(m1b, newUK, type = "response")
newUK$m2b <- predict(m2b, newUK, type = "response")

rmse(newUK$m1b, UKdata$number_speeches)
rmse(newUK$m2b, UKdata$number_speeches)
```


<table>
<thead><tr><th scope=col>Model</th><th scope=col>theta</th><th scope=col>Resid. df</th><th scope=col>   2 x log-lik.</th><th scope=col>Test</th><th scope=col>   df</th><th scope=col>LR stat.</th><th scope=col>Pr(Chi)</th></tr></thead>
<tbody>
	<tr><td>ideological_distance               </td><td>1.542051                           </td><td>195                                </td><td>-1546.604                          </td><td>                                   </td><td>NA                                 </td><td>      NA                           </td><td>        NA                         </td></tr>
	<tr><td>party_leader + ideological_distance</td><td>1.580399                           </td><td>194                                </td><td>-1541.899                          </td><td>1 vs 2                             </td><td> 1                                 </td><td>4.705125                           </td><td>0.03007281                         </td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col>Model</th><th scope=col>theta</th><th scope=col>Resid. df</th><th scope=col>   2 x log-lik.</th><th scope=col>Test</th><th scope=col>   df</th><th scope=col>LR stat.</th><th scope=col>Pr(Chi)</th></tr></thead>
<tbody>
	<tr><td>ideological_distance               </td><td>1.305019                           </td><td>151                                </td><td>-1842.362                          </td><td>                                   </td><td>NA                                 </td><td>       NA                          </td><td>       NA                          </td></tr>
	<tr><td>party_leader + ideological_distance</td><td>1.312192                           </td><td>150                                </td><td>-1841.409                          </td><td>1 vs 2                             </td><td> 1                                 </td><td>0.9527573                          </td><td>0.3290184                          </td></tr>
</tbody>
</table>




15.071932635599



14.8527808549615



129.867854313204



129.312636830255


In this basic model we can see that for Germany, the coefficient of ideological distance is
negative (in accordance to our hypothesis 2) and the coefficient is significant on a 90%
significance level. Looking at the UK we see a positive coefficient (as expected in hypothesis 1), but unfortunately the coefficient is not significant because the p-value is too high.


In a next step we want to expand this basic model with the remaining comparable variable party leader. It makes sense to include this variable, because a party leader may
have a higher number of speeches in parliament, and simultaneously a very low ideological distance (measured to his/her own position, obviously). As we can see in Table 1,
the variable ideological distance remains significant for Germany, and also the variable
party leader has some impact on the number of speeches. The comparison of the AIC
as well as the RMSE support the impression, that the Expanded Model works better for
the German data. For the United Kingdom it seems that this additional variable has no
additional impact, because it is not significant. This is also supported by the AIC values,
because it rises from the Basic Model to the Expanded Model.

## Simulating the Differences between Countries


```R
# SIMULATING THE DIFFERENCE BETWEEN THE SYSTEMS

gamma.hat.uk1 <- coef(m2b)
V.hat.uk1 <- vcov(m2b)
S.uk1 <- mvrnorm(1000, gamma.hat.uk1, V.hat.uk1)

gamma.hat.ger1 <- coef(m2a)
V.hat.ger1 <- vcov(m2a)
S.ger1 <- mvrnorm(1000, gamma.hat.ger1, V.hat.ger1)

# SCENARIOS: HIGHEST AND LOWEST VALUE OF IDEOLOGICAL DISTANCE,
# SIMULATE THE VALUES FOR EACH COUNTRY SEPARATELY
germin.ideo.scen <- cbind(1, 0, min(GERdata$ideological_distance, na.rm=TRUE))
germax.ideo.scen <- cbind(1, 0, max(GERdata$ideological_distance, na.rm=TRUE))
ukmin.ideo.scen <- cbind(1, 0, min(UKdata$ideological_distance, na.rm=TRUE))
ukmax.ideo.scen <- cbind(1, 0, max(UKdata$ideological_distance, na.rm=TRUE))

Xbeta.germin <- S.ger1 %*% t(germin.ideo.scen)
Xbeta.germax <- S.ger1 %*% t(germax.ideo.scen)
Xbeta.ukmin <- S.uk1 %*% t(ukmin.ideo.scen)
Xbeta.ukmax <- S.uk1 %*% t(ukmax.ideo.scen)

lambda.germin <- exp(Xbeta.germin)
lambda.germax <- exp(Xbeta.germax)
lambda.ukmin <- exp(Xbeta.ukmin)
lambda.ukmax <- exp(Xbeta.ukmax)

theta.ger <- m2a$theta
theta.uk <- m2b$theta

exp.germin <- sapply(lambda.germin, 
                    function(x) mean(rnbinom(100, size = theta.ger, mu = x)))
exp.germax <- sapply(lambda.germax, 
                    function(x) mean(rnbinom(100, size = theta.ger, mu = x)))
exp.ukmin <- sapply(lambda.ukmin, 
                    function(x) mean(rnbinom(100, size = theta.uk, mu = x)))
exp.ukmax <- sapply(lambda.ukmax, 
                    function(x) mean(rnbinom(100, size = theta.uk, mu = x)))

exp.ger <- c(exp.germin, exp.germax)
exp.uk <- c(exp.ukmin, exp.ukmax)
df.ger <- data.frame(exp.ger)
df.uk <- data.frame(exp.uk)

df.ger$id <- c(rep("min", 1000), rep("max", 1000))
df.uk$id <- c(rep("min", 1000), rep("max", 1000))

ggplot(df.ger, aes(x = exp.ger, fill = id)) + 
  geom_density(alpha = 0.4) + 
  guides(fill = guide_legend(title = "")) +
  xlab("Expected Speeches") +
  ylab("Density") + 
  ggtitle("Simulating German Number of Speeches") + 
  theme_bw()

ggplot(df.uk, aes(x = exp.uk, fill = id)) + 
  geom_density(alpha = 0.4) + 
  guides(fill = guide_legend(title = "")) +
  xlab("Expected Speeches") +
  ylab("Density") + 
  ggtitle("Simulating British Number of Speeches") + 
  theme_bw()
```






![png](/img/2017/12/output_15_2.png)



![png](/img/2017/12/output_15_3.png)


Figure 2 used the Expanded Models from table 1 to simulate outcomes with interesting
variable values. For each of the two countries we used the highest and the lowest value
for ideological distance and defined the variable party leader equals 0, because it was the
most common value for this variable. The resulting outcomes are shown in red for the
highest value and in blue for the lowest value.

By comparing this two figures, we see that the order of the two curves is swapped. This
supports the impression we had from the coefficients from table 1, that the hypotheses
are supported, at least in their general direction. But we can also see in this comparison,
that the curves of the UK overlap more than for Germany, which is another indicator that
the impact of ideological distance may be more meaningful for Germany than for the UK.

## Deeper Modelling
### United Kingdom

For the United Kingdom there is only one variable left that could be used for further model
building. When we add this variable to the already expanded model (see Appendix United
Kingdom: Further Models for detailed results), we observe that ideological distance is now
significant, too, and also AIC and RMSE are reduced in comparison to the Expanded
Model in table 1. When we exclude now the not significant variable party leader we
remain with the (in this comparison) best model for predicting the number of speeches in
the British parliament.


```R
# MODELLING THE UK CASE

# MODEL 3: FULL MODEL
m3b <- glm.nb(number_speeches ~ party_leader + 
              ideological_distance + 
              conservative_MP, 
              data=UKdata, 
              control = glm.control(maxit=100))

# MODEL 4: REDUCED (BEST) MODEL
m4b <- glm.nb(number_speeches ~ ideological_distance + 
              conservative_MP, 
              data=UKdata, 
              control = glm.control(maxit=100))

anova(m4b, m3b)

newUK$m3b <- predict(m3b, newUK, type = "response")
rmse(newUK$m3b, UKdata$number_speeches)

newUK$m4b <- predict(m4b, newUK, type = "response")
rmse(newUK$m4b, UKdata$number_speeches)

#stargazer(m3b, m4b)
m3b
```


    Error in glm.nb(number_speeches ~ party_leader + ideological_distance + : konnte Funktion "glm.nb" nicht finden
    Traceback:
    


### Germany

For Germany our possibilities to build further models are greater. Appendix Germany:
Further Models shows an overview over the different calculated models. Model 3 was
calculated with all the available variables except for the party dummies and showed immediately an improvement as well for the AIC as for the RMSE, this measurement dropped
by roughly 2.5 points. But in this model, compared to the previous one from table 1,
party leader lost its significance. Ideological distance remained significant at an 0.1-level,
while the new variables number of committee memberships and whether the speaker was
member of a governing party are significant at a 0.01-level. This could support the suspicion that there is a general "engagement" level underlying, affecting both the number of
speeches and the number of committee memberships in a positive manner. The coefficient
calculated whether the MP is member of a governing party shows a negative sign, so it
seems that the "smaller" parties (because we have a coalition between the two biggest
parties at this point in time) are over-proportionally represented in speeches.

The fourth model adds the party dummy variables to the previosly described model, but
this step worsens both the AIC and the RMSE. No party dummy is significant (CDU/CSU
was excluded, because there would be a perfect collinearity with "member of government
party" together with the SPD dummy). This can be seen also in the fact, that the signs and significance levels of the coefficients of the other variables stay the same and the
coefficients only change slightly.

In a fifth model we want to summarize only those variables that were found to be
significant. Indeed, this model is the best of the models we calculated so far, measured
by the RMSE as well as by the AIC. There would have been a model with an even lower
RMSE (when excluding ideological distance), but this would increase the AIC a lot.

The last model, model 6 respectively, was only build to be a good foundation for the later
on simulation of party differences, therefore, besides the party dummies, only the previous
significant variables were included, except for member of governing party, because this
would lead to redundancy with the party dummies of SPD and CDU/CSU. This model is
not as good as the best model (compared by AIC and RMSE), but it is interesting that
SPD and CDU/CSU become significant on a 0.01-level, when the member of governing
party variable was removed.

All in all, the best model for Germany achieves a Root-Mean-Squared-Error of roughly 12. This value indicates that we are able to predict the number of speeches a MP does
during the observed period with an average error of +- 12. Compared to the range of
values the dependent variable can have, this number is quite high. Therefore I would
expect that there is still a better model for this data.


```R
# MODELLING THE GERMAN CASE

# MODEL 3: EXPANDED MODEL
m3a <- glm.nb(number_speeches ~ party_leader + 
            ideological_distance + 
            list_candidate_new + 
            committee + 
            caolMPoutside + 
            ingov, 
            data = GERdata, 
            control = glm.control(maxit = 100))

anova(m2a, m3a)

newGER$m3a <- predict(m3a, newGER, type = "response")
rmse(newGER$m3a, GERdata$number_speeches)

# MODEL 4: FULL MODEL
m4a <- glm.nb(number_speeches ~ party_leader + 
              ideological_distance + 
              list_candidate_new + 
              committee + 
              caolMPoutside + 
              ingov + 
              FDP + 
              SPD + 
              Linke, 
              data = GERdata, 
              control = glm.control(maxit = 100))

anova(m3a, m4a)

newGER$m4a <- predict(m4a, newGER, type = "response")
rmse(newGER$m4a, GERdata$number_speeches)

# MODEL 5: BEST MODEL
m5a <- glm.nb(number_speeches ~ ideological_distance + 
              committee + 
              ingov, 
              data = GERdata, 
              control = glm.control(maxit = 100))

anova(m5a, m4a)

newGER$m5a <- predict(m5a, newGER, type = "response")
rmse(newGER$m5a, GERdata$number_speeches)

# MODEL 6: PARTY MODEL (FOR SIMULATIONS)
m6a <- glm.nb(number_speeches ~ ideological_distance + 
              committee + 
              FDP + 
              CDU_CSU + 
              Linke + 
              SPD, 
              data = GERdata,
              control = glm.control(maxit = 100))

newGER$m6a <- predict(m6a, newGER, type = "response")
rmse(newGER$m6a, GERdata$number_speeches)

#stargazer(m3a, m4a, m5a, m6a)
```


<table>
<thead><tr><th scope=col>Model</th><th scope=col>theta</th><th scope=col>Resid. df</th><th scope=col>   2 x log-lik.</th><th scope=col>Test</th><th scope=col>   df</th><th scope=col>LR stat.</th><th scope=col>Pr(Chi)</th></tr></thead>
<tbody>
	<tr><td>party_leader + ideological_distance                                                         </td><td>1.580399                                                                                    </td><td>194                                                                                         </td><td>-1541.899                                                                                   </td><td>                                                                                            </td><td>NA                                                                                          </td><td>    NA                                                                                      </td><td>          NA                                                                                </td></tr>
	<tr><td>party_leader + ideological_distance + list_candidate_new + committee + caolMPoutside + ingov</td><td>2.455717                                                                                    </td><td>190                                                                                         </td><td>-1463.705                                                                                   </td><td>1 vs 2                                                                                      </td><td> 4                                                                                          </td><td>78.194                                                                                      </td><td>4.440892e-16                                                                                </td></tr>
</tbody>
</table>




12.3182518934873



<table>
<thead><tr><th scope=col>Model</th><th scope=col>theta</th><th scope=col>Resid. df</th><th scope=col>   2 x log-lik.</th><th scope=col>Test</th><th scope=col>   df</th><th scope=col>LR stat.</th><th scope=col>Pr(Chi)</th></tr></thead>
<tbody>
	<tr><td>party_leader + ideological_distance + list_candidate_new + committee + caolMPoutside + ingov                    </td><td>2.455717                                                                                                        </td><td>190                                                                                                             </td><td>-1463.705                                                                                                       </td><td>                                                                                                                </td><td>NA                                                                                                              </td><td>      NA                                                                                                        </td><td>       NA                                                                                                       </td></tr>
	<tr><td>party_leader + ideological_distance + list_candidate_new + committee + caolMPoutside + ingov + FDP + SPD + Linke</td><td>2.483081                                                                                                        </td><td>187                                                                                                             </td><td>-1461.958                                                                                                       </td><td>1 vs 2                                                                                                          </td><td> 3                                                                                                              </td><td>1.746746                                                                                                        </td><td>0.6265917                                                                                                       </td></tr>
</tbody>
</table>




12.3262798051617



<table>
<thead><tr><th scope=col>Model</th><th scope=col>theta</th><th scope=col>Resid. df</th><th scope=col>   2 x log-lik.</th><th scope=col>Test</th><th scope=col>   df</th><th scope=col>LR stat.</th><th scope=col>Pr(Chi)</th></tr></thead>
<tbody>
	<tr><td>ideological_distance + committee + ingov                                                                        </td><td>2.403064                                                                                                        </td><td>193                                                                                                             </td><td>-1467.611                                                                                                       </td><td>                                                                                                                </td><td>NA                                                                                                              </td><td>      NA                                                                                                        </td><td>       NA                                                                                                       </td></tr>
	<tr><td>party_leader + ideological_distance + list_candidate_new + committee + caolMPoutside + ingov + FDP + SPD + Linke</td><td>2.483081                                                                                                        </td><td>187                                                                                                             </td><td>-1461.958                                                                                                       </td><td>1 vs 2                                                                                                          </td><td> 6                                                                                                              </td><td>5.652997                                                                                                        </td><td>0.4631611                                                                                                       </td></tr>
</tbody>
</table>




12.0748504936798



12.0986683850752


## First Differences for Germany


```R
# SIMULATION FOR FIRST DIFFERENCES

quants.mean.fun <- function(x){
  c(quants = quantile(x, probs=c(0.025,0.5,0.975), mean = mean(x)))
}

gamma.hat.ger2 <- coef(m3a)
V.hat.ger2 <- vcov(m3a)
S.ger2 <- mvrnorm(1000, gamma.hat.ger2, V.hat.ger2)
```

To support the results interpreted from the regression table, we applied some simulation
to have a look at the expected values of number of speeches in different scenarios. These
scenarios were built based on Model 3 and took the median values of the variables, except
for those the first difference was calculated on.

As we can see (in table 2) our results from the coefficient interpretation were supported.
The only First Difference with substantial meaning is whether the MP is member of a
governing party or not, because the confidence interval does not include 0. But when we
look at the size of the difference, it means 1 speech more or less per election period.


```R
# scenario: (y/n) party leader, median distance, median list, 
# media committees, median outside, median ingov
scenario.lead <- cbind(1, 1, 0.1253, 1, 3, 0, 1) 
scenario.notl <- cbind(1, 0, 0.1253, 1, 3, 0, 1) 

X.lead <- as.matrix(rbind(scenario.lead, scenario.notl))
lead.combined <- S.ger2 %*% t(X.lead)
fd.lead <- lead.combined[,1] - lead.combined[,2]
quants.fd.lead <- apply(as.matrix(fd.lead), 2, quants.mean.fun)
quants.fd.lead

hist(fd.lead, main = "First Differences between Partyleaders 
     and Non-Partyleaders" )
abline(v = quants.fd.lead, lty = 2)
```


<table>
<tbody>
	<tr><th scope=row>quants.2.5%</th><td>-0.08922938</td></tr>
	<tr><th scope=row>quants.50%</th><td> 0.24013696</td></tr>
	<tr><th scope=row>quants.97.5%</th><td> 0.56187497</td></tr>
</tbody>
</table>




![png](/img/2017/12/output_24_1.png)



```R
# scenario: no party leader, median distance, (y/n) list candidate, 
# media committees, median outside, median ingov
scenario.list <- cbind(1, 0, 0.1253, 1, 3, 0, 1) 
scenario.dire <- cbind(1, 0, 0.1253, 0, 3, 0, 1) 

X.list <- as.matrix(rbind(scenario.list, scenario.dire))
list.combined <- S.ger2 %*% t(X.list)
fd.list <- list.combined[,1] - list.combined[,2]
quants.fd.list <- apply(as.matrix(fd.list), 2, quants.mean.fun)
quants.fd.list

hist(fd.list, main = "First Differences between List Candidates and 
     Direct Mandates" )
abline(v = quants.fd.list, lty = 2)
```


<table>
<tbody>
	<tr><th scope=row>quants.2.5%</th><td>-0.1183215</td></tr>
	<tr><th scope=row>quants.50%</th><td> 0.1222505</td></tr>
	<tr><th scope=row>quants.97.5%</th><td> 0.3452989</td></tr>
</tbody>
</table>




![png](/img/2017/12/output_25_1.png)



```R
# scenario: no party leader, median distance, median list, 
# media committees, (y/n) outside, median inreg
scenario.inco <- cbind(1, 0, 0.1253, 1, 3, 0, 1) 
scenario.outc <- cbind(1, 0, 0.1253, 1, 3, 1, 1) 

X.inco <- as.matrix(rbind(scenario.inco, scenario.outc))
inco.combined <- S.ger2 %*% t(X.inco)
fd.inco <- inco.combined[,1] - inco.combined[,2]
quants.fd.inco <- apply(as.matrix(fd.inco), 2, quants.mean.fun)
quants.fd.inco

hist(fd.inco, main = "First Differences between Speekers \n within 
     the Coalition Interval and outside" )
abline(v = quants.fd.inco, lty = 2)
```


<table>
<tbody>
	<tr><th scope=row>quants.2.5%</th><td>-0.41507740</td></tr>
	<tr><th scope=row>quants.50%</th><td>-0.15265534</td></tr>
	<tr><th scope=row>quants.97.5%</th><td> 0.09423774</td></tr>
</tbody>
</table>




![png](/img/2017/12/output_26_1.png)



```R
# scenario: no party leader, median distance, median list, 
# media committees, median outside, (y/n) inreg
scenario.ingo <- cbind(1, 0, 0.1253, 1, 3, 0, 1) 
scenario.outg <- cbind(1, 0, 0.1253, 1, 3, 0, 0) 

X.ingo <- as.matrix(rbind(scenario.ingo, scenario.outg))
ingo.combined <- S.ger2 %*% t(X.ingo)
fd.ingo <- ingo.combined[,1] - ingo.combined[,2]
quants.fd.ingo <- apply(as.matrix(fd.ingo), 2, quants.mean.fun)
quants.fd.ingo

hist(fd.ingo, main = "First Differences between Speekers \n from 
     the Governing Parties and the Opposition" )
abline(v = quants.fd.ingo, lty = 2)
```


<table>
<tbody>
	<tr><th scope=row>quants.2.5%</th><td>-1.1553990</td></tr>
	<tr><th scope=row>quants.50%</th><td>-0.8804044</td></tr>
	<tr><th scope=row>quants.97.5%</th><td>-0.6083633</td></tr>
</tbody>
</table>




![png](/img/2017/12/output_27_1.png)


## Differences between German Parties

Because it was quite surprising that the parties didn’t show any difference in the number
of speeches, we simulated this difference, too, to get a better feeling of the data. In the
figure below we see clearly the difference between government and opposition parties.
This difference may result out of the unusual predominant size of the government (which
controlled nearly 70% of the parliament seats [1]).


```R
# SIMULATING OTHER QUANTITIES OF INTEREST

gamma.hat.ger3 <- coef(m6a)
V.hat.ger3 <- vcov(m6a)
S.ger3 <- mvrnorm(1000, gamma.hat.ger3, V.hat.ger3)

# SCENARIOS: TAKE MEDIAN EXCEPT FOR PARTY VALUES
scenario.FDP <- cbind(1, 0.1253, 3, 1, 0, 0, 0) 
scenario.CDU <- cbind(1, 0.1253, 3, 0, 1, 0, 0) 
scenario.LIN <- cbind(1, 0.1253, 3, 0, 0, 1, 0) 
scenario.SPD <- cbind(1, 0.1253, 3, 0, 0, 0, 1)
scenario.GRU <- cbind(1, 0.1253, 3, 0, 0, 0, 0) 

XbetaFDP <- S.ger3 %*% t(scenario.FDP)
XbetaCDU <- S.ger3 %*% t(scenario.CDU)
XbetaLIN <- S.ger3 %*% t(scenario.LIN)
XbetaSPD <- S.ger3 %*% t(scenario.SPD)
XbetaGRU <- S.ger3 %*% t(scenario.GRU)

lambdaFDP <- exp(XbetaFDP)
lambdaCDU <- exp(XbetaCDU)
lambdaLIN <- exp(XbetaLIN)
lambdaSPD <- exp(XbetaSPD)
lambdaGRU <- exp(XbetaGRU)

thetam6a <- m6a$theta

exp.FDP <- sapply(lambdaFDP, 
                  function(x) mean(rnbinom(1000, size = thetam6a, mu = x)))
exp.CDU <- sapply(lambdaCDU, 
                  function(x) mean(rnbinom(1000, size = thetam6a, mu = x)))
exp.LIN <- sapply(lambdaLIN, 
                  function(x) mean(rnbinom(1000, size = thetam6a, mu = x)))
exp.SPD <- sapply(lambdaSPD, 
                  function(x) mean(rnbinom(1000, size = thetam6a, mu = x)))
exp.GRU <- sapply(lambdaGRU, 
                  function(x) mean(rnbinom(1000, size = thetam6a, mu = x)))

exp.values <- c(exp.FDP, exp.CDU, exp.LIN, exp.SPD, exp.GRU)
df.parties <- data.frame(exp.values)
df.parties$id <- c(rep("FDP", 1000), 
                   rep("CDU/CSU", 1000), 
                   rep("LINKE", 1000), 
                   rep("SPD", 1000),
                   rep("GRUENE", 1000))

ggplot(df.parties, aes(x = exp.values, fill = id)) + 
  geom_density(alpha = 0.4) + 
  guides(fill = guide_legend(title = "")) +
  xlab("Expected Speeches") +
  ylab("Density") +
  ggtitle("Simulated expected Number of Speeches, separated 
          by Party Affiliation") +
  theme_bw()
```




![png](/img/2017/12/output_29_1.png)


## Conclusion

All in all we can say, that we can support both hypotheses at least in the direction they
stated above. The strength may be worth to be discussed, as well as the importance of
other variables, which were not available for the computation of models in this case.

It would be interesting, for example, if the appeared difference in government and opposition parties for Germany is also found for the UK, but unfortunately there was no
separation between the Labour and the Liberal party. We saw for the German case, that
this difference is highly significant, so maybe we could observe this in other countries as
well.

Besides that there is still much work to do when we look at the sizes of effects (for
example in table 2) or the size of the RMSE, which is still quite high compared to the
range of actual values.
