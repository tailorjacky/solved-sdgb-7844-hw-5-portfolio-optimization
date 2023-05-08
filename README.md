Download Link: https://assignmentchef.com/product/solved-sdgb-7844-hw-5-portfolio-optimization
<br>
Submit two files through Blackboard: (a) .Rmd R Markdown file with answers and code and (b) Word document of knitted R Markdown file. Your file should be named as follows: “HW5-[Full Name]-[Class Time]” and include those details in the body of your file.

Complete your work individually and comment your code for full credit. For an example of how to format your homework see the files related to the Lecture 1 Exercises and the RMarkdown examples on Blackboard. <strong>Show all of your code in the knitted Word document.</strong>

Given the uncertainty about the future performance of financial markets, investors typically diversify their portfolios to improve the quality of their returns. In this assignment, you will be constructing a portfolio composed of two ETFs (exchange traded funds) that tracks US equity and fixed income markets. The equity ETF tracks the widely followed S&amp;P 500 while the fixed income ETF tracks long-term US Treasury bonds. You will use optimization techniques to determine what fraction of your money should be allotted to each asset (i.e., portfolio weights).

The Sharpe ratio is a widely used metric to gauge the quality of portfolio returns. It was developed by William F. Sharpe (a Nobel prize winner) and provides one way to construct an optimal portfolio. This computation allows investors to determine expected returns while taking into account a measure of how risky that investment is. Heuristically, it is computed by taking the expected return of an asset/portfolio, subtracting the risk free rate (i.e., how much you would make if you simply kept your money in the bank and let it collect interest) and dividing by the standard deviation of the portfolio returns. The higher the Sharpe ratio value, the better the investment is considered to be. This formula assumes that returns are normally distributed (not always the best assumption to make with financial data).

1

The data for the two assets were downloaded from Google Finance (ticker symbols: SPY, TLT) and the federal funds interest rate, representing the risk free rate, was taken from the U.S. Federal Reserve Bank website. We will be looking at weekly returns, as opposed to daily returns, as they are less correlated with each other; monthly returns would be even less correlated, however, we would need a much longer time period of data to have enough observations to do a robust analysis. Note that if we used a different time period to compute our optimal rates, or we used daily instead of weekly returns, we may get very different results.

<ol>

 <li>Upload the data in “asset data.txt” into R and call the tibble x. The columns are defined as follows: date, close.spy is the closing price for the S&amp;P500 ETF, close.tlt is the closing price for the long term treasury bond ETF, and fed.rate is the federal funds interest rate in percent form. Look at the data type for the column date; just like</li>

</ol>

the data types numeric, logical, etc., R has one for date/time data. <strong>Extract only the observations where the federal funds rate is available (so you are left with weekly data); this is the data you will use for the rest of the analysis. </strong>What is the start date and end date of this reduced data set? Graph the federal funds interest rate as a time series. Describe what you see in the plot and relate it briefly to the most recent financial crisis.

<ol start="2">

 <li>Now we will split the data into training and test sets. The training data will be used to compute our portfolio weights and our test set will be used to evaluate our portfolio. Make two separate tibbles: (a) the training set should contain all observations before 2014 and (b) the test set should contain all observations in 2014. (Install and load the R package lubridate and use the function year to extract the year from the date column of your data set x.) How many observations are in each subset?</li>

 <li>The federal funds interest rate is in percent form so convert it to decimal (i.e., fractional) form. Then, for the S&amp;P 500 and long term treasury bonds ETF assets, compute the returns using the following formula:</li>

</ol>

where <em>r<sub>t </sub></em>is the return at time <em>t</em>, <em>p<sub>t </sub></em>is the asset price at time <em>t</em>, and <em>p<sub>t</sub></em><sub>−1 </sub>is the asset price at time <em>t </em>− 1 (i.e., the previous period). Add both sets of returns to your training set

tibble. These returns are also called total returns. Construct a single time series plot with the returns for both assets plotted. Add a dotted, horizontal line at <em>y </em>= 0 to the plot. Compare the two returns series. What do you see?

<ol start="4">

 <li>The Sharpe ratio calculation assumes that returns are normally distributed<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>. Construct two normal quantile plots, one for training set returns of each asset. Is this assumption</li>

</ol>

satisfied? Justify your answer.

<ol start="5">

 <li>Compute the correlation between the S&amp;P500 and long term treasury bond returns in the training set and interpret it<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a>. Now, we will compute a rolling-window correlation as</li>

</ol>

follows: compute the correlation between the two asset returns only using the first 24 weeks of data (i.e., weeks 2 to 25), next compute the correlation between the two asset returns for data from week 3 through 26, then week 4 through 27, and so forth.

Once you compute the rolling-window correlations, make a time series plot of the rollingwindow correlation with each point plotted on the <em>last </em>day of the window. Add a horizontal, dotted, gray line at 0 to your plot. Is the correlation or rolling-window correlation a better way to describe the relationship between these two assets? Justify your answer.

<ol start="6">

 <li>Compute the Sharpe ratios<a href="#_ftn3" name="_ftnref3"><sup>[3]</sup></a> for each asset on the training set as follows:</li>

</ol>

Step 0. Let <em>r<sub>t </sub></em>be the return and <em>y<sub>t </sub></em>be the federal funds interest rate for week <em>t </em>= 1<em>,…,T</em>.

Step 1. Compute the excess returns, <em>e<sub>t</sub></em>, for each week in the data set<a href="#_ftn4" name="_ftnref4"><sup>[4]</sup></a>:

Excess returns are returns that you earn in excess to the risk free rate.

Step 2. Convert the excess returns into an excess returns index, <em>g<sub>t</sub></em>:

<em>g</em><sub>1 </sub>=     100

<em>g</em><em>t        </em>=     <em>g</em><em>t</em>−1 × (1 + <em>e</em><em>t</em>)

Step 3. Compute the number of years of data, <em>n</em>, by taking the number of weeks for which you have <em>returns </em>(i.e., number of observations in your training set minus 1) and dividing by 52 (since there are 52 weeks in a year); therefore the number of years of data can be a fractional amount.

Step 4. Compute the compounded annual growth rate, CAGR:

Step 5. Compute the annualized volatility, <em>ν</em>:

√ <em>ν    </em>= 52<em>SD</em>[<em>e<sub>t</sub></em>]

where <em>SD</em>[<em>e<sub>t</sub></em>] is the standard deviation of the excess returns.

Step 6. Compute the Sharpe Ratio, <em>SR</em>, which is the ratio of the compounded annual growth rate and the annualized volatility:

Which asset is a better investment? Justify your answer.

<ol start="7">

 <li>Write a function which takes the following inputs: (a) a vector of portfolio weights (call this argument x; weights are between 0 and 1), (b) a vector of returns for asset 1, (c) a vector of returns for asset 2, and (d) a vector of the corresponding weekly federal funds interest rates. The function will then do the following: for each weight value in your vector x, you will compute the Sharpe ratio for the corresponding portfolio. To obtain the returns for the portfolio, use the following equation<a href="#_ftn5" name="_ftnref5"><sup>[5]</sup></a>:</li>

</ol>

<em>r</em><em>t,portfolio          </em>=       <em>xr</em><em>t,S</em>&amp;<em>P</em>500 + (1 − <em>x</em>)<em>r</em><em>t,treasury</em>

That is, <em>x </em>proportion of the funds will be invested in the S&amp;P500 ETF and (1 − <em>x</em>) proportion of the funds will be invested into the treasury bond ETF. After you compute the returns for the portfolio, apply the steps in question 6 to get the Sharpe ratio for that portfolio. Your function should output a vector of Sharpe ratios, one for each portfolio weight in x.

Use stat function() to plot the function you just wrote. Weights between 0 and 1 should be on the <em>x</em>-axis and the Sharpe ration should be on the <em>y</em>-axis. The training set

data should be used as the input for (b), (c), and (d) above. Do you see a portfolio weight that produces the maximum Sharpe ratio?

<ol start="8">

 <li>Using the training set, use optimize() to determine the optimum weight for each asset</li>

</ol>

using the function you wrote in question 7; how much of the funds should be allocated to each asset? What is the Sharpe ratio of the overall portfolio? According to your analysis, is it better to invest in S&amp;P500 only, long term treasury bonds only, or your combined portfolio? Justify your answer.

<ol start="9">

 <li>For the remainder of this assignment, we will be evaluating our portfolio using the <u>test set </u> We will be comparing three strategies: investing only in the S&amp;P500, investing only in long term treasury bonds, and investing in the combined portfolio (computed in question 8).</li>

</ol>

In your <u>test set</u>, convert the federal funds interest rate from percent to decimal form and compute the returns series for each of the three assets (see question 3 for more details). Next, compute the excess returns index for each asset in the test set (as outlined in question 6). Plot the excess returns index for each asset on the same time series plot. Add a dotted, horizontal line at <em>y </em>= 100. Describe what you see.

<ol start="10">

 <li>The excess returns index can be interpreted as follows: if you invested in $100 in at time <em>t </em>= 1, the index value at time <em>T </em>represents how much you have earned in addition to (i.e., in excess of) the risk free interest rate. If you invested $100 in each asset (portfolio, all in long term treasury bonds, or all in S&amp;P500) in the first week of January, 2014 , how much would you have at the end of the <u>test set </u>period for each asset in addition to the risk-free interest rate? Did your portfolio perform well in the test set? Justify your answer.</li>

</ol>

<a href="#_ftnref1" name="_ftn1">[1]</a> We will continue with our analysis regardless of whether this assumption is satisfied.

<a href="#_ftnref2" name="_ftn2">[2]</a> If you construct a scatterplot of these two particular assets, you will get a linear relationship.

<a href="#_ftnref3" name="_ftn3">[3]</a> The Sharpe ratio can be positive or negative.

<a href="#_ftnref4" name="_ftn4">[4]</a> We divide the federal funds interest rate by 52, the number of weeks in a year, to turn it into a weekly rate from an annual rate.

<a href="#_ftnref5" name="_ftn5">[5]</a> Note to people with a finance background: these computations correspond to a setting where you are rebalancing the portfolio daily.