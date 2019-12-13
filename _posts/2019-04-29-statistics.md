---
layout: post
tags: data
---

# This is a notebook about Statistic, taken while watching online course Statistic from Khan Academy

## Basic notation

>
> + sample size : $n$
> + population size : $N$
> + popualation mean : $\mu$ 
> + sample mean : $\bar{x}$ 
> + the  population variance : $\sigma^2 = { \sum{(x_i -\bar{x})^2} \over N}$
>
> + the sample variance : $S_n^2 = {\sum(x_i - \bar{x})^2 \over n }$
>
> **often the sample variance is less than the population variance, so in order to estimate the population variance, we need to modify it as follows:**
>
> + the **unbiased** sample variance : $S_n^2 = {\sum(x_i - \bar{x})^2 \over n -1}$ 
> + stanard deviation : $\sigma = \sqrt{\sigma^2} = \sqrt{\sum(x_i - \mu)^2 \over N}$ 
> + stanard deviation : $s= \sqrt{s^2} = \sqrt{\sum(x_i - \bar{x})^2 \over n-1}$ 
>

## Random Variable
> A function mapping a experiment to a variable 
>
> There are two types:
> 1. discrete random variable
> 2. continues random variable
>

## Bernoulli distribution
> A single trial with two possible outcome, p% to succed and (1-p)% to fail 

**Expectation of Bernoulli distribution**
> + $$E = p$$
> + $$\sigma^2 = p(1-p)$$

## Binomial distribution
The binomial distribution is a Bernoulli distribution if n tries, n>1.

> example: 
>
> A person took 6 shoots with 30% likely to make the shot

p(X = 0) = 0.7 * 0.7 * 0.7 * 0.7 * 0.7 * 0.7 = $0.7^6 * 0.3^0 * C_6^0$ 

p(X = 1) = 0.3 * 0.7 * 0.7 * 0.7 * 0.7 * 0.7 = $0.7^5 * 0.3^1 * C_6^1$ 

p(X = 2) = 0.3 * 0.3 * 0.7 * 0.7 * 0.7 * 0.7 = $0.7^4 * 0.3^2 * C_6^2$ 

p(X = 3) = 0.3 * 0.3 * 0.3 * 0.7 * 0.7 * 0.7 = $0.7^3 * 0.3^3 * C_6^3$ 

p(X = 4) = 0.3 * 0.3 * 0.3 * 0.3 * 0.7 * 0.7 = $0.7^2 * 0.3^4 * C_6^4$ 

p(X = 5) = 0.3 * 0.3 * 0.3 * 0.3 * 0.3 * 0.7 = $0.7^1 * 0.3^5 * C_6^5$ 

p(X = 6) = 0.3 * 0.3 * 0.3 * 0.3 * 0.3 * 0.3 = $0.7^0 * 0.3^6 * C_6^6$ 

> $$p(X=n) = 0.3^n * 0.7^{6-n} * C_6^n$$
>
> $$C_n^k = \binom{n}{k} = \frac{n!}{k!(n-k)!}$$

---

**Expectation of Binomial distribution**

> n tries with possibility of p to success for each try

> + $$ p(x=k) = \binom{n}{k}*p^k*(1-p)^{n-k} $$
> + $$ E(x) = np $$
> + $$ \sigma^2 = np(1-p) $$
> + $$E = p$$
> + $$\sigma^2 = p(1-p)$$

## Poisson distribution
> based on two assumption:
> 1. the event happens at stable rate, any period of time is no different than another 
> 2. the events between different time period are independent

The expectation

$$E(X) = \lambda = np$$

say X = number of cars passion in 1 hour

$\lambda$ cars/hour = $np$ = ${60 * {\lambda \over 60}}$

this is to split one hour into 60 min and examin if 1 car will pass in each minute with the possibility of $\lambda \over 60$ and test rize n = 60

WHAT IF more than 1 cars passes ---> MORE GRANULAR

Set the interval to 1 sec:

$$p(X=k) = \binom{3600}{k}*({\lambda \over 3600})^k*({1-{\lambda \over 3600}})^{3600-k}$$
--> more and more granular --> $\infty$

### Possion distribution formular

$$p(X=k)=\lim_{n\to\infty} \binom{n}{k}*({\lambda \over n})^k*({1-{\lambda \over n}})^{n-k} = ......$$
$$=\frac{\lambda^k}{k!} \cdot e^{-\lambda}$$


## Normal Distribuation

$$p(X) = \frac{1}{\sigma \sqrt{2\pi}}  e ^{-\frac{1}{2}(\frac{x-\mu}{\sigma})^2}$$

$$Z-score = \frac{x-\mu}{\sigma}$$

$$p(X) = \frac{1}{\sigma \sqrt{2\pi}}  e ^{-\frac{1}{2}z^2}$$
Z-score is how many standard deviations away from the sample mean and it can be used on any distributions

**NOTE** : 
>the Z table is a accumulative distribution

**Significant level** :
> + $\mu \pm \sigma$: p = 68%
> + $\mu \pm 2\sigma$ : p = 95% --> common confidential Interval
> + $\mu \pm 3\sigma$: p = 99.7%

## Central limit theorem

> sample sum or sample mean from any distribution will display a normal distribution with large enough sampling

Standard Error : $S_{Err}$  = Sampling distribution of the sample mean
and in the sampling distribution :

$$\mu = \mu_0$$

> $\mu$ : mean of sampling distribution
>
> $\mu_0$ : mean of original population

$$\sigma_{\bar{x}} = \frac{\sigma}{\sqrt{n}} \approx \frac{s}{\sqrt{n}} $$

> In most cases, we aren't not able to get the standard deivation of the population and std of sample is used to estimate the true std of population
, and subsequently estimate the std of the sampling distribution

> p($\bar{x}$ is within 2 $\sigma_{\bar{x}}$ if $\mu_{\bar{x}}$) =  
> p($\mu_{\bar{x}}$ is within 2 $\sigma_{\bar{x}}$ if $\bar{x}$) 

### One tail test
> if a point where the normalized z score is 2, the area under the curve is before this point is 97.5%, the p value is 1-0.25 = 97.5%

### two tail test
> if a point where the normalized z score is 2, the area under the curve is before this point is 97.5%, the p value is 1-2*0.25 = 95%

so two tail test is recommended for its stricter p value

### About sample size
 > if n > 30, the sd is considered close to $\sigma$
 > if n < 30, we use t-distribution to estimate its significant level instead of normal distribution

## Independent random variables
for two independent variables X, Y

$$E(X) = \mu_X$$
$$var(X) = E((X-\mu_X)^2) = \sigma_X^2$$
$$E(Y) = \mu_Y$$
$$var(Y) = E((Y-\mu_Y)^2) = \sigma_Y^2$$

For another variable Z, if Z = X + Y:

$$E(Z) = E(X) + E(Y)$$
$$\mu_Z = \mu_X + \mu_Y$$
$$var(Z) = var(X) + var(Y)$$
$$\sigma_Z^2 =  \sigma_{X+Y}^2 = \sigma_X^2 + \sigma_Y^2$$


For another variable Z, if G = X - Y:

$$E(G) = E(X) - E(Y)$$
$$\mu_G = \mu_X - \mu_Y$$
$$var(G) = var(X) + var(Y)$$
$$\sigma_G^2 =  \sigma_{X-Y}^2 =  \sigma_X^2 + \sigma_{-Y}^2 = \sigma_X^2 + \sigma_Y^2$$

**NOTE** :
> It is variance, not standard deviation
---
If we have the sample of x and y, even with different sample size(n,m), 
we wil have their sampling distribution, and if  we are interested in $z = \bar{x} - \bar{y}$ (the difference between their sampling distribution):

We will have a diff sampling distribution like this:

$$\mu_{\bar{x}-\bar{y}} = \mu_{\bar{x}} - \mu_{\bar{y}}$$
$$\sigma_{\bar{x}-\bar{y}}^2 = \sigma_{\bar{x}}^2 + \sigma_{\bar{y}}^2 = \frac{\sigma_x^2}{n} + \frac{\sigma_y^2}{m}$$
$$\sigma_{\bar{x}-\bar{y}} = \sqrt{\frac{\sigma_x^2}{n} + \frac{\sigma_y^2}{m}}$$

## **NOTE**

The one random variable test is to  test:

1. Given a population mean, we can calculate the probability of getting that sample, and do a hypothesis test(test if the sample mean is the population mean), accept or reject the hypothesis based a p value. 
2. calculate the confidentila interval without the population mean provided.


The two random variable test is to  test how different these two varibles are:

1. Given the mean, we can calculate the probability of getting those sample, and do a hypothesis test(test if they are different, null : they are the not different, the mean of diff is 0 and they should have the same sd, use the overall sd to estimate if possible), accept or reject the hypothesis based a p value.
2. calculate the confidentila interval without the mean provided.



## Linear regression
> To find a line that has represent the data point best,  
> the fitted line should have the minimized squared error  

with the line being 

$$y = mx+b$$

SE_line (squared error agianst the line):

$$SE_{line} =(y_1-(mx_1+b)^2)+(y_2-(mx_2+b)^2)+...+(y_n-(mx_n+b)^2)$$

> We can formulate m and b using partial derivative

$$m =\frac{\bar{xy} -\bar{x}\bar{y}}{\bar{xx} - \bar{x}\bar{x}}$$
$$b = ..$$

## Assessment of the fit

> + total variation of y:

$$SE_{\tilde{y}} = (y_1 -\bar{y})^2 +(y_2 -\bar{y})^2+...+y_n -\bar{y})^2$$


> + total variation **NOT** described by the line :

$$SE_{line} =(y_1-(mx_1+b)^2)+(y_2-(mx_2+b)^2)+...+(y_n-(mx_n+b)^2)$$

> Total variation  = those described by the line + those not derribed by the line

$$R^2 = 1- {SE_{line} \over SE_{\tilde{y}}}$$
> Here, R squared is the coeffiicient of determination, showing what % of the total variation is descrubed by variation in x according to the line


## Co-variance 


$$cov(x,y)= E((x-E(x))(y-E(y))) =\bar{xy} -\bar{x}\bar{y}$$


$$m =\frac{\bar{xy} -\bar{x}\bar{y}}{\bar{xx} - \bar{x}\bar{x}} = \frac{cov(x,y)}{cov(x,x)} = \frac{cov(x,y)}{var(x)}$$

## Chi-squared Distribution
> A test of whether distributions are different

|Day|Mon|Tue|Wen|Thu|Fri|Sat
|---|---|---|---|---|---|---|
|Expected %|10|10|15|20|30|25|
|Oberserved|30|14|34|45|57|20| total =200
|Expected|20|20|30|40|60|30|

Chi-square statistic 

$$\chi^2 =\frac{(30-20)^2}{20}+\frac{(14-20)^2}{20}+\frac{(34-30)^2}{30}+\frac{(45-40)^2}{40}+\frac{(57-60)^2}{60}+\frac{(20-30)^2}{30}$$ 

df = 6-1 = 5 as we take 6 sums


$$\chi^2 = \sum_{i=1}^{n}\frac{(O_i-E_i)^2}{E_i}$$
O_i = number of the observation  
E_i = number of the expectation, E_i = NP_i


## Anova - Analysis of variance

**F statistic**

$$F = \frac{\frac{SSB}{dfB}}{\frac{SSW}{dfW}}$$
F statistic can be considered the ratio of two chisquare distribution

> + SSB : sum of square between groups
> + SSW : sum of square within groups
> + SST : sum of square total

Example :


|c1|c2|c3|
|-|-|-|
|3|5|5|
|2|3|6|
|1|4|7|


> + $$\bar{x_1} = 2, \bar{x_2} = 4, \bar{x_3} = 6$$
> + $$\bar{\bar{x}} = 4$$

> + $$SST = (3-4)^2 + (2-4)^2+ (1-4)^2+ (5-4)^2+(3-4)+... = 30$$
> + $$SSW = (3-2)^2 + (2-2)^2+ (1-2)^2+ (5-3)^2+(3-3)+... = 6$$
> + $$SST = (2-4)^2*3 + (4-4)^2*3 + (6-4)^2*3 = 24$$

> + dfT = mn -1 = 8
> + dfW = mn-m = 6
> + dfB = m -1 = 2

$$SST = SSB+ SSW$$
$$dfT = dfB+ dfW$$


## Fisher exact test

G1|G2|sum
-|-|-
a|b|a+b
c|d|c+d
a+c|b+d|n= a+b+c+d

$$p = \frac{\binom{a+b}{a}\binom{c+d}{c}}{\binom{n}{a+c}} = \frac{(a+b)!(c+d)!(a+c)!(b+d)!}{a!b!c!d!n!}$$

Example

M|F|sum 
-|-|-
1|9|10
11|3|14
12|12|24

$$p = \frac{\binom{10}{1}\binom{14}{11}}{\binom{24}{12}}$$

> **NOTE**  
> the question to answer here is, Given the row and col sum  
how likely it is to get distribution observed.
