# Do Healthier Foods Take More Time to Prepare...?

# Introduction

In a world with constant scientific developments, we are learning more and more about the human body and how it functions each day. As a result, more resources are being used for the research of food and naturally, new recipes are being invented that advertise themselves as cleaner and healthier overall compared to some other options. **However, do these healthy recipes take longer to prepare compared to their unhealthy counterparts?**

This page explores a dataset obtained from food.com, presenting the information that supports and addresses the question above. The analysis involves various data science techniques, including data cleaning, exploratory data analysis, assessment of missing data, and hypothesis testing. We will delve into the processes and interpretations derived from the data.

# Cleaning and EDA

### Data Cleaning
### Univariate Analysis
### Bivariate Analysis
### Interesting Aggregates

# Assessment of Missingness

# Hypothesis Testing

**Observation:** There is about a 12 minutes difference between the prepation times of healthy food and non-healhty food

**Null Hypothesis (h0):** There is no significant difference in the mean preparation time between recipes with the healthy tag and recipes without the healthy tag.

**Alternate Hypothesis (h1):** There is a significant difference in the mean preparation time between recipes with the healthy tag and recipes without the healthy tag.

**Test Statistic:** The mean absolute difference in healthy preparation times and non healhty preparation times 

**Significance Level:** We will be running this permutation test at the 0.05 significance level.

**P-value:** 0.733

**Histogram:**

**Conclusion:** Since our P-Value (0.733) >= our significance level(0.05), we fail to reject the null and conclude that there is not enough evidence to suggest a significant difference in the mean 'minutes' between healthy and non-healthy groups based on the permutation test. The observed difference in means may be attributed to random sampling variability, and the results should be interpreted with caution.




## The Data

Let's take a look at our data, which contains both recipes as well as reviews. 

## Visuals
<iframe src="assets/cals-mins-scatter.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/cooking-time-hist-1.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/cooking-time-hist-2.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/healthy-nonhealthy-pie.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/hyp-testing-hist.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/mins-ratings-scatter.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/prep-time-healthy-1.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/prep-time-healthy-2.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/recipe-length-missingness-1.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/recipe-length-missingness-2.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/recipe-length-missingness-3.html" width=800 height=600 frameBorder=0></iframe>
