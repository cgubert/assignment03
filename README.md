# Assignment 3
Patrick D. Schloss  
September 26, 2014  

Complete the exercises listed below and submit as a pull request to the [Assignment 3 repository](http://www.github.com/microbialinformatics/assignment03).  Format this document approapriately using R markdown and knitr. For those cases where there are multiple outputs, make it clear in how you format the text and interweave the solution, what the solution is.

Your pull request should only include your *.Rmd and *.md files. You may work with a partner, but you must submit your own assignment and give credit to anyone that worked with you on the assignment and to any websites that you used along your way. You should not use any packages beyond the base R system and knitr.

This assignment is due on October 10th.

------

1.  Generate a plot that contains the different pch symbols. Investigate the knitr code chunk options to see whether you can have a pdf version of the image produced so you can print it off for yoru reference. It should look like this:

    <img src="pch.png", style="margin:0px auto;display:block" width="500">
    
```r
symbols<-c(1:25)
y<-rep(0.5,25)
pch.list<-as.numeric(symbols)
plot(x=symbols,y=y,pch=c(pch.list),main="PCH Symbols",ylab=NA,xlab="PCH value",at=1:25,frame.plot=FALSE)
abline(v=1:25,col="grey")
```

2.  Using the `germfree.nmds.axes` data file available in this respositry, generate a plot that looks like this. The points are connected in the order they were sampled with the circle representing the beginning ad the square the end of the time course:

    <img src="beta.png", style="margin:0px auto;display:block" width="700">

```r
germfree<-read.table(file="germfree.nmds.axes",header=T)
mouse337<-subset(germfree,mouse==337)
mouse343<-subset(germfree,mouse==343)
mouse361<-subset(germfree,mouse==361)
mouse387<-subset(germfree,mouse==387)
mouse389<-subset(germfree,mouse==389)
plot(mouse337$axis1,mouse337$axis2,type="l",lwd=1.5,ylim=c(-0.55,0.4),xlim=c(-0.3,0.7),xlab="NMDS Axis 1",ylab="NMDS Axis 2")
lines(mouse343$axis1,mouse343$axis2,type="l",col="blue",lwd=1.5)
lines(mouse343$axis1,mouse343$axis2,type="l",col="blue",lwd=1.5)
lines(mouse361$axis1,mouse361$axis2,type="l",col="red",lwd=1.5)
lines(mouse387$axis1,mouse387$axis2,type="l",col="green",lwd=1.5)
lines(mouse389$axis1,mouse389$axis2,type="l",col="brown",lwd=1.5)
init337<-head(mouse337,n=1)
end337<-tail(mouse337,n=1)
points(init337$axis1,init337$axis2,pch=16,col="black")
points(end337$axis1,end337$axis2,pch=15,col="black")
init343<-head(mouse343,n=1)
end343<-tail(mouse343,n=1)
points(init343$axis1,init343$axis2,pch=16,col="blue")
points(end343$axis1,end343$axis2,pch=15,col="blue")
init361<-head(mouse361,n=1)
end361<-tail(mouse361,n=1)
points(init361$axis1,init361$axis2,pch=16,col="red")
points(end361$axis1,end361$axis2,pch=15,col="red")
init387<-head(mouse387,n=1)
end387<-tail(mouse387,n=1)
points(init387$axis1,init387$axis2,pch=16,col="green")
points(end387$axis1,end387$axis2,pch=15,col="green")
init389<-head(mouse389,n=1)
end389<-tail(mouse389,n=1)
points(init389$axis1,init389$axis2,pch=16,col="brown")
points(end389$axis1,end389$axis2,pch=15,col="brown")
legend(0,-0.2,c("Mouse 337","Mouse 343","Mouse 361","Mouse 387","Mouse 389"),lty=1,col=c("black","blue","red","green","brown"),cex=0.7)
```

3.  On pg. 57 there is a formula for the probability of making x observations after n trials when there is a probability p of the observation.  For this exercise, assume x=2, n=10, and p=0.5.  Using R, calculate the probability of x using this formula and the appropriate built in function. Compare it to the results we obtained in class when discussing the sex ratios of mice.

```r
x<-2
n<-10
p<-0.5
((n/x)*(p^x)*((1-p)^(n-x)))
```

Or using the dbinom function in r:

```r
dbinom(x,n,p)
```

4.  On pg. 59 there is a formula for the probability of observing a value, x, when there is a mean, mu, and standard deviation, sigma.  For this exercise, assume x=10.3, mu=5, and sigma=3.  Using R, calculate the probability of x using this formula and the appropriate built in function

```r
x<-10.3
mu<-5
sigma<-3
e <- exp(1) 
((1/(sqrt(2*pi*sigma)))*e^(-((x-mu)^2)/((2*sigma)^2)))
```

or with rnorm function:

```r
rnorm(x,mu,sigma)
```


5.  One of my previous students, Joe Zackular, obtained stool samples from 89 people that underwent colonoscopies.  30 of these individuals had no signs of disease, 30 had non-cancerous ademonas, and 29 had cancer.  It was previously suggested that the bacterium *Fusobacterium nucleatum* was associated with cancer.  In these three pools of subjects, Joe determined that 4, 1, and 14 individuals harbored *F. nucleatum*, respectively. Create a matrix table to represent the number of individuals with and without _F. nucleatum_ as a function of disease state.  Then do the following:

```r
colon<-c(15,14,55,5)
colon<-matrix(data=colon,nrow=2,ncol=2,dimnames=list(c("non-cancer","cancer"),c("neg","pos")))
as.data.frame(colon)
```
Null hypothesis: the prevalence of *F.nulceatum* in non-cancer subjects equals the prevalenve or *F. nucleatum* in cancer subjects.
Alternative hypothesis: the prevalence of *F.nulceatum* in non-cancer subjects DEOS NOT equal the prevalenve or *F. nucleatum* in cancer subjects.

    * Run the three tests of proportions you learned about in class using built in R  functions to the 2x2 study design where normals and adenomas are pooled and compared to carcinomas.
```r
prop.test(colon)
```
  2-sample test for equality of proportions with continuity
	correction

data:  colon
X-squared = 16.2736, df = 1, p-value = 5.482e-05
alternative hypothesis: two.sided
95 percent confidence interval:
 -0.7761149 -0.2689979
sample estimates:
   prop 1    prop 2 
0.2142857 0.7368421 
```r
fisher.test(colon)
```
  Fisher's Exact Test for Count Data

data:  colon
p-value = 4.094e-05
alternative hypothesis: true odds ratio is not equal to 1
95 percent confidence interval:
 0.02429846 0.35312657
sample estimates:
odds ratio 
 0.1007433 
```r
chisq.test(colon)
```
Pearson's Chi-squared test with Yates' continuity correction

data:  colon
X-squared = 16.2736, df = 1, p-value = 5.482e-05

*p-value is less than 0.05; reject the null hypothesis.*

    * Without using the built in chi-squared test function, replicate the 2x2 study design in the last problem for the Chi-Squared Test...

      * Calculate the expected count matrix and calculate the Chi-Squared test statistics. Figure out how to get your test statistic to match Rs default statistic.
      *	Generate a Chi-Squared distributions with approporiate degrees of freedom by the method that was discussed in class (hint: you may consider using the `replicate` command)
      * Compare your Chi-Squared distributions to what you might get from the appropriate built in R functions
      * Based on your distribution calculate p-values
      * How does your p-value compare to what you saw using the built in functions? Explain your observations.


6.  Get a bag of Skittles or M&Ms.  Are the candies evenly distributed amongst the different colors?  Justify your conclusion.

From one 11.4 oz bag of M&Ms, the following numbers of each color candy were observed:
```r
mm<-c(7,15,20,20,21,29)
MM<-matrix(data=mm,nrow=6,ncol=1,dimnames=list(c("yellow","blue","brown","orange","green","red"),c("quantity")))
as.data.frame(MM)
chisq.test(MM)
```
Chi-squared test for given probabilities

data:  MM
X-squared = 14.2143, df = 5, p-value = 0.0143

The colors are not evenly distributed. This can be gathered from the data (range from 7 yellow candies to 29 red candies) over a small (6) number of different colors. A Chi-squared test gives a p value of 0.0143, which is less than 0.05, which suggests to reject the null hypothesis. In this case, the null hypothesis would be that there is no difference in the number of candies from each color category. Therefore, we must assume that there is a statistically signifigant difference in the number of candies between colors. Fortunately, I don't like the yellow M&Ms. 



