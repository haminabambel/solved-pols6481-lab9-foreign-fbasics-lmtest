Download Link: https://assignmentchef.com/product/solved-pols6481-lab9-foreign-fbasics-lmtest
<br>



<ol>

 <li><strong> Preparation</strong></li>

</ol>

1) Open RStudio by double-clicking the icon or selecting RStudio from the Windows Start menu.

2) Open the POLS6481-Spring2021-UH-lab Project and perform a Git pull.

3) If not using Projects, Git, and <em>here </em>download files, place in working directory, and make changes as needed.

6) Open the R script by typing <em>Ctrl</em>+<em>O</em> or by clicking on File in the upper-left corner, using the dropdown menu, and navigating to the script in your working directory.

7) Run lines 1-5 in the R script to load three packages that you will need. You can install the <em>fBasics</em> and <em>lmtest</em> packages using the <strong>Packages</strong> tab or by uncommenting and running line 3 prior to running lines 4-5.




<ol>

 <li><strong> Instructions for Lab Week 09</strong></li>

</ol>

<strong> </strong>

You will use data on employment and economic growth in 25 OECD countries.  The dependent variable in the analysis will be average annual percentage rate of growth in employment between 1988 and 1997 (<em>EMPLOY</em>) and the main independent variable will be average annual percentage rate of real GDP growth over the same time period (<em>GDP</em>). This is one of the major examples used in chapter 4 of Christopher Dougherty’s <em>Introduction to Econometrics</em>, 4<sup>th</sup> ed. (Oxford University Press, 2011).




Begin by loading data frame that you will work with, named oecd, by running line 7 or using the following code changing the directory as needed:

&gt; oecd &lt;- read.dta(“~:/oecd.dta”)




Run lines 9-10 to examine the normality of the dependent variable (<em>EMPLOY</em>) visually. Line 9 generates the normal quantile plot of <em>EMPLOY</em> against the normal distribution; any deviation from a straight line indicates a non-normal distribution. Line 10 generates a histogram of <em>EMPLOY</em>; any deviation from a bell curve would indicate a non-normal distribution.

&gt; qqnorm(oecd$EMPLOY)

&gt; hist(oecd$EMPLOY, freq = FALSE)




Run line 11 for the summary statistics of <em>EMPLOY</em>. A normally distributed variable has skewness equal to 0 and kurtosis equal to 3; the <em>kurtosis</em> command in R automatically subtracts 3, so look for deviations from 0. Based on your inspection, is <em>EMPLOY</em> approximately normally distributed?

&gt; skewness(oecd$EMPLOY)

&gt; kurtosis(oecd$EMPLOY)




For comparison, run line 13 for the normal quantile plot of our main independent variable (<em>GDP</em>), which is <u>not</u> normally distributed.

&gt; qqnorm(oecd$GDP)

We saw in lab 7 that there might be justification for taking the natural log of an independent variable when its distribution is abnormal. Run line 14 for the normal quantile plot of <em>log</em>(<em>GDP</em>):

&gt; qqnorm(log(oecd$GDP))




Line 17 plots <em>EMPLOY</em> against <em>GDP</em>, with a horizontal line for 0 growth in employment. You should observe a nonlinear pattern.




Line 18 and line 19 show two ways to plot <em>y</em> against <em>ln</em>(<em>x</em>). Line 18 implicitly creates a new variable, <em>log</em>(<em>GDP</em>), and uses these values for <em>x</em>. Line 19 does not re-code the variable, but instead tells R to use a logarithmic scale for the <em>x</em> axis. It turns out, this is as simple as including the code log=”x” inside the parentheses!




Now our attention moves to three methods of modeling the relationship between growth and employment growth. Run lines 21–23 to create three new variables representing average GDP growth, average GDP growth squared, and the reciprocal of average GDP growth.




<ol>

 <li><strong> Linear</strong></li>

</ol>

<strong> </strong>

Run line 26 to estimate the model in which employment growth is linearly related to GDP growth.

&gt; linmod&lt;-lm(EMPLOY~growth1, oecd); summary(linmod)

On page 6, I provided some space for you to interpret the effect of average growth on employment growth. Use the results of this estimation to fill in the “Linear” section.




The next five lines of R code provide different ways of considering the linearity of the relationship.

<ol>

 <li>Run line 27 to plot a scatterplot of the raw values of employment growth and the fitted values. In the R script file, I included extra codes to make the dots 25% smaller (cex = .75) and the fitted line red (col = “red”) and twice as thick (lwd = 2).</li>

</ol>

&gt; plot(oecd$growth1, oecd$EMPLOY, pch=19, cex=.75); abline(linmod)

<ol>

 <li>The residuals ought to be normally distributed. Run line 28, which generates a straight line plot only if residuals are normal.</li>

</ol>

&gt; qqnorm(linmod$residuals)

<ol>

 <li>Run line 29 to show the residuals-versus-predictor (growth1) plot. Do the residuals fit our expectations of zero expectation given growth and constant variance?</li>

</ol>

&gt; plot(oecd$growth1, linmod$residuals, cex=.5); abline(h=0)

<ol>

 <li>Run line 30 to load the white test function into R. Alternately, if you are not using <em>here </em>or you receive an error, you can open and run the <strong><em>white-test.R</em></strong> Then run line 31 to perform White’s test of homoscedasticity. Do you reject the null hypothesis of constant error variance?</li>

</ol>




<ol start="9">

 <li>Run line 32 to perform “Ramsey’s RESET” (regression specification error test). If this test yields a statistically significant <em>p</em> value, then it suggests we should reject the null hypothesis that the model is correctly specified. Details on this test are in Wooldridge’s section 9.1 (in fifth edition) as well as Dougherty’s section 4.3.</li>

</ol>




All in all, the numerical results from the linear model suggest that heteroskedasticity is not a problem; this should be confirmed by the residuals-versus-predictor or a residuals-versus-fitted plot. However, this is why a visual approach to regression is necessary: the scatterplot reveals a probable nonlinearity problem. Growth levels appear to have a large effect at low levels and a small effect at high levels, i.e., diminishing marginal returns, but a linear model cannot accommodate this pattern.




<ol>

 <li><strong> Quadratic</strong></li>

</ol>

<strong> </strong>

Run line 35 to estimate the model in which employment growth is a quadratic function of growth. (You created the variables growth1 and growth2 in line 21 and line 22, respectively.)

&gt; quadmod&lt;-lm(EMPLOY~growth1+growth2, data=oecd); summary(quadmod)

The next five lines of R code provide some methods to assess whether we have adequately addressed the nonlinearity problem using the quadratic model, and to re-evaluate the homoscedasticity assumption.

<ol>

 <li>First, run line 36 to graph a scatterplot of the raw values and the fitted values of employment growth against economic growth.</li>

 <li>Run line 37 to examine the normal quantile plot; are the residuals approximately normal?</li>

 <li>Run line 38 to examine the residuals-versus-predictor plot. Is heteroskedasticity still a problem?</li>

 <li>Having already run the <strong><em>white-test.R</em></strong> script earlier, run line 39 to perform White’s test to test the null hypothesis that our residuals are free from heteroskedasticity.</li>

 <li>Run line 40 to perform Ramsey’s RESET to see if the quadratic model is correctly specified.</li>

</ol>

Compare the significance of an unrestricted model (which contains both growth and growth-squared) and a restricted model (which contains just growth). You can use the anova command in line 42 to do so.

Finally, before we move into the issue of interpreting marginal effects in a nonlinear model, run line 43 to assess whether we introduced multicollinearity. Why is this important to do when using a quadratic model?




Let’s do some interpretation of the quadratic model. On page 6, I provided space for you to interpret the effect of average growth on average employment growth. Use the results of this second estimation to fill in the “Quadratic” section. Note that the code I am showing below is slightly simpler than what is in the R script; using the code shown below correctly requires you to remember that the coefficient for growth is <em>second</em> – the constant is the first – and the coefficient for squared-growth is <em>third</em>.




<ol>

 <li>Run lines 46-49 to preserve some summary statistics of the key independent variable, average growth. I am saving the average value of growth (you might choose the median instead, after looking at line 46’s results), as well as the minimum and maximum values.</li>

</ol>

&gt; summary(oecd$growth1)

&gt; meangdp &lt;- mean(oecd$growth1)

&gt; mingdp &lt;- min(oecd$growth1)

&gt; maxgdp &lt;- max(oecd$growth1)




<ol start="6">

 <li>Run line 50 to compute the marginal effect of growth on employment growth at the mean level of growth in the sample. Fill this in on page 6.</li>

</ol>

&gt; quadmod$coef[2] + 2*quadmod$coef[3]*meangdp




<ol start="6">

 <li>To examine how the relationship between economic growth and employment growth changes as GDP growth’s value changes, run line 51 and line 52 to compute the marginal effect of economic growth on employment growth at the minimum and maximum values of GDP growth, respectively. Fill in the marginal effects on page 6.</li>

</ol>

&gt; quadmod$coef[2] + 2*quadmod$coef[3]*mingdp

&gt; quadmod$coef[2] + 2*quadmod$coef[3]*maxgdp




<ol>

 <li><em>What happens to the effect of growth on employment growth as growth increases?</em> Run lines 53-55 to plot the two fitted lines against each other. Remember, most countries in our sample are clustered on the left part of the scale. The quadratic fitted line’s slope is steeper than the linear fitted line’s slope in the range of most countries. However, the quadratic curve bends so that its slope is shallower, and then its slope turns negative, so that higher levels of growth would correspond to lower employment growth!</li>

</ol>




Take the time to examine closely the equations for creating these plots. Students who have learned calculus will have an easier time understanding that the marginal effect is the first derivative of the function <em>y</em> = <em>f</em>(<em>x</em>), where <em>f</em>(<em>x</em>) = <em>b</em><sub>0</sub> + <em>b</em><sub>1</sub><em>x + b</em><sub>2</sub><em>x</em><sup>2</sup>; using the <strong>power rule</strong> and <strong>sum rule</strong> yields <em>f’</em>(<em>x</em>) = <em>b</em><sub>1</sub> + 2<em>b</em><sub>2</sub><em>x</em>.

So, we must add the coefficient for growth and the coefficient for growth-squared <u>times 2</u> <u>times <em>x</em></u>!




The next five lines of code suggest one way to plot marginal effects and confidence intervals around those marginal effects. Recall that in a linear model, the marginal effect equals the regression coefficient, and there is a single confidence interval around that effect. For a nonlinear model, however, the marginal effect is the first derivative, which will depend on the value of <em>x</em>. As we will see in lecture 18, the width of the confidence interval also varies depending on the value of <em>x</em>.




<ol>

 <li>Run line 57 to create the equation for the slope, using the equation for <em>f’</em>(<em>x</em>) shown above.</li>

</ol>

&gt; slope = quadmod$coefficients[2] + (2*quadmod$coefficients[3]*oecd$growth1)




<ol>

 <li>Run line 58 to create the variance-covariance matrix of the estimators, which we then use to generate the standard errors of the slope. Notice that the standard error depends on the variance of the coefficient for growth, the variance of the coefficient for squared growth, and the covariance of these coefficients. Notice also that <em>growth1 </em>(growth) and <em>growth2</em> (squared growth) enter into the equation.</li>

</ol>

&gt; vce = vcov(quadmod)

&gt; sequad = sqrt(vce[2,2] + 4*oecd$growth2*vce[3,3] + 4*oecd$growth1*vce[2,3])




<ol start="2">

 <li>Run line 59 to create the upper and lower bounds for the confidence interval for the slope; you are adding and subtracting 2.07 times the standard error (computed in line 57) to/from the slope.</li>

</ol>

&gt; cimax = slope + 2.07*sequad

&gt; cimin = slope – 2.07*sequad

The <em>t</em> critical value for 23 degrees of freedom is roughly 2.07.




<ol>

 <li>Run line 60 to plot the marginal effect and its confidence interval, with a horizontal line for zero. The curve shown by running line 60 is <u>not</u> the predicted value of employment growth given average growth; rather the curve shows the <u>slope</u> of the function relating employment growth to average growth.</li>

</ol>

When the marginal effect is above zero, it indicates that in this range, growth has a positive effect on text scores; when the curve is below zero, it indicates that in this range, growth has a negative effect on employment growth. Thus, a downward sloping marginal effect plot does not indicate that growth reduces employment growth; instead it indicates that the positive impact declines and ultimately becomes indistinguishable from zero.




<ol>

 <li>Run line 61 to plot the values of growth (as a box-and-whisker plot). By combining these plots in a single figure, you can see that most countries have values for growth that fall in a range where growth has a positive effect on employment growth. Only a few outliers are in the range of values of GDP growth where it has an effect on employment growth that is indistinguishable from zero.</li>

</ol>




[Note: this only looks right if the <strong>Plots</strong> window is close to a square; if it is a rectangle then the box-and-whisker plot might overlap the marginal effect plot.]




Run line 62 to restore the defaults for plotting. There are lots of resources for learning how to combine graphs; I used QuickR: <em>http://www.statmethods.net/advgraphs/layout.html</em>.




<ol>

 <li><strong> Reciprocal</strong></li>

</ol>




It is hard to imagine a situation in which higher average growth should lead to <u>lower</u> average employment growth. On this basis, you might prefer a model that asymptotes to a flat line, such as a reciprocal model.




Run line 65 to estimate the model in which employment growth are a function of 1/growth (defined way back in line 24). Notice the constant: this is the value to which the function will converge as economic growth approaches infinity (i.e., as 1/growth goes to zero). The next five lines of code allow you to consider the model’s adequacy.

<ol>

 <li>Run lines 66-67 to graph a scatterplot of the values of employment growth and their fitted values (i.e., predicted employment growth) against average growth.</li>

 <li>Run line 68 to examine the normal quantile plot; run line 69 to generate the residuals-versus-fitted-values plot. Have we resolved the heteroskedasticity problem to your satisfaction?</li>

 <li>Run line 70 to perform White’s test; do you retain or reject the null hypothesis that the residuals are homoscedastic?</li>

</ol>

Turn your attention now to interpretation; use the following to fill in the “Reciprocal” section on page 6.

<ol>

 <li>Run line 73 to compute the marginal effect of growth on employment growth at the mean level of growth.</li>

 <li>To examine how the relationship between growth and employment growth changes as growth’s value changes, run line 74 and line 75 to compute the marginal effect of growth on employment growth at the minimum and maximum values of economic growth, respectively.</li>

 <li>Run line 77 to generate the marginal effects, and then run line 78 to graph this curve against a histogram showing the values of average district growth.</li>

</ol>

The curve shown by running line 77 is <u>not</u> the predicted value of employment growth given average growth; that curve would be downward sloping, due to diminishing returns, but never reaching zero. Rather, the curve shown by running line 77 is the slope of the function relating employment growth to average growth, across all levels of average growth.




Be sure to take some time to inspect the equations for creating the marginal effects. Students who have learned calculus will have an easier time understanding that what we care about is the first derivative of the function <em>y</em> =<em> f</em>(<em>x</em>), where <em>f</em>(<em>x</em>) = <em>b</em><sub>0</sub> + <em>b</em><sub>1</sub><em>x</em><sup>–</sup><sup>1</sup>. Using the ‘power rule’ one finds that <em>f</em>’(<em>x</em>) = –<em>b</em><sub>1</sub><em>x</em><sup>–</sup><sup>2</sup>.




Run lines 80-83 to plot employment growth against growth, including predicted employment growth using all three models:

<ul>

 <li>the linear model’s fitted values are displayed with a <strong>purple </strong>line;</li>

 <li>the quadratic model’s fitted values are displayed with a <strong>neon green</strong> line;</li>

 <li>the reciprocal model’s fitted values are displayed with a <strong>royal blue</strong></li>

</ul>

<u> </u>

You have estimated three models this week: linear, quadratic, and reciprocal. Which seems to fit the data best? Which is the easiest to interpret? Based on these judgments, which model do you prefer for this application?




<strong> </strong>

<strong>Linear Model (R<sup>2</sup> = ______)</strong>




Marginal effect of growth at <strong>all values</strong> of growth:                   a 1-unit increase in economic growth results in a _____ unit change in employment growth




<strong>Quadratic Model (R<sup>2</sup> = ______)</strong>




Marginal effect of growth at <strong>minimum</strong> growth:                       a 1-unit increase in economic growth results in a _____ unit change in employment growth




Marginal effect of growth at <strong>mean </strong>growth:                                a 1-unit increase in economic growth results in a _____ unit change in employment growth




Marginal effect of growth at <strong>maximum</strong> growth:                       a 1-unit increase in economic growth results in a _____ unit change in employment growth




<strong>Reciprocal Model (R<sup>2</sup> = ______)</strong>




Marginal effect of growth at <strong>minimum</strong> growth:                       a 1-unit increase in economic growth results in a _____ unit change in employment growth




Marginal effect of growth at <strong>mean </strong>growth:                                a 1-unit increase in economic growth results in a _____ unit change in employment growth




Marginal effect of growth at <strong>maximum</strong> growth:                       a 1-unit increase in economic growth results in a _____ unit change in employment growth










<strong>For more information on polynomial regression, see </strong>

<strong> </strong>

<strong>https://bookdown.org/tpinto_home/Beyond-Linearity/polynomial-regression.html</strong>




I have uploaded a polynomial functions graph from this site in the “Additional Resources Folder” on GitHub”


