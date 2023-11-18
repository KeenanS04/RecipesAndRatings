# Do Healthier Foods Take More Time to Prepare...?

# Introduction

In a world with constant scientific developments, we are learning more and more about the human body and how it functions each day. As a result, more resources are being used for the research of food and naturally, new recipes are being invented that advertise themselves as cleaner and healthier overall compared to some other options. **However, do these healthy recipes take longer to prepare compared to their unhealthy counterparts?**

This page explores a dataset obtained from food.com, presenting the information that supports and addresses the question above. The analysis involves various data science techniques, including data cleaning, exploratory data analysis, assessment of missing data, and hypothesis testing. We will delve into the processes and interpretations derived from the data.

### The Data

The first part of our dataset consists of 83782 recipes collected since 2008, containing the following information:

| Column          | Description                                      |
|-----------------|--------------------------------------------------|
| `'name'`          | Recipe name                                      |
| `'id'`            | Recipe ID                                        |
| `'minutes'`       | Minutes to prepare recipe                        |
| `'contributor_id'`| User ID who submitted this recipe                |
| `'submitted'`     | Date recipe was submitted                        |
| `'tags'`          | Food.com tags for recipe                         |
| `'nutrition'`     | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'`       | Number of steps in recipe                        |
| `'steps'`         | Text for recipe steps, in order                  |
| `'description'`   | User-provided description                        |

The second part of our dataset consists of 731927 reviews for the recipes given above. This dataset contains the following information:

| Column     | Description       |
|------------|-------------------|
| `'user_id'`  | User ID           |
| `'recipe_id'`| Recipe ID         |
| `'date'`     | Date of interaction|
| `'rating'`   | Rating given      |
| `'review'`   | Review text       |

In our analysis, we mainly used data from the `rating`, `minutes`, `tags` and `nutrition` column to determine differences between aspects of healthy and non-healthy foods. 

# Cleaning and EDA

### Data Cleaning

To create our final dataset that will be used for our analysis, we performed multiple operations on our data. 

1. Merged the recipes with the reviews, using the `recipe_id` as our key to merge on.
2. Replaced each 0 in the `rating` column with `np.nan` to account for missing ratings in reviews.
3. Created a new column, `avg_rating` that stores the average rating among reviews from each recipe.
4. Converted `date` column to datetime objects
5. Converted list-like strings to actual lists (`tags`, `nutrition`, `steps`, `ingredients`)
6. Drop rows with too much missing data

1. **Converting Object to List**: Our initial step involves addressing the `tags`, `steps`, `nutrition`, and `ingredients` columns in the dataframe. Although these columns appear to be lists, closer inspection reveals that they are not true lists. To fix this, we implement a function to convert these three columns into lists of their respective datatypes.

2. **Converting 'Submitted' Column into Datetime**: The `submitted` column in the dataframe is currently represented as an object. To increase ease-of-use, we convert it into a datetime data type.

3. **Merging Two Dataframes**: Given that both dataframes share common columns, namely 'id' and 'recipe_id', we merge the two dataframes to present a comprehensive view of recipes alongside their corresponding ratings and reviews.

4. **Adding Average Rating Column**: Following the merge, we identify the crucial data point of recipe ratings. To enhance our analysis, we introduce a new column named 'avg_rating', containing the average rating for each recipe. Additionally, we acknowledge that a rating of 0 may signify an empty field where users did not provide a rating. Consequently, we replace these 0 values with 'nan' for more accurate representation.

5. **Added NaN Values**: Replaced all 0 ratings with `np.nan` to account for missing ratings. It is impossible to give a rating of 0, so to account for this we use `np.nan`.

6. **Drop unnecessary columns**: Because will not use all columns, we drop most of them to improve readability of our dataframe.

After our data cleaning, our dataframe looks like this:

|    | name                                 |     id |   minutes | tags                                                                                                                                                                                                                        | nutrition                                    |   n_steps |   n_ingredients |   rating |   avg_rating |
|----|--------------------------------------|--------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------|-----------|-----------------|----------|--------------|
|  0 | 1 brownies in the world    best ever | 333281 |        40 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings'] | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]     |        10 |               9 |        4 |            4 |
|  1 | 1 in canada chocolate chip cookies   | 453467 |        45 | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                               | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0] |        12 |              11 |        5 |            5 |
|  2 | 412 broccoli casserole               | 306168 |        40 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 |               9 |        5 |            5 |
|  3 | 412 broccoli casserole               | 306168 |        40 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 |               9 |        5 |            5 |
|  4 | 412 broccoli casserole               | 306168 |        40 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 |               9 |        5 |            5 |


### Univariate Analysis
<iframe src="assets/healthy-nonhealthy-pie.html" width=800 height=600 frameBorder=0></iframe>

This pie chart shows the distribution of healthy and non healthy recipes in our dataset. As you can see there is much greater percentage of non-healthy recipes over healthy recipes. 

<iframe src="assets/prep-time-healthy-1.html" width=800 height=600 frameBorder=0></iframe>

This histogram shows the how the preparation time of each healthy and non-healthy recipe stacks up with each other. In this plot you can't see a trend because of the outliers.
7
Getting rid of outliers, we get this histogram.
<iframe src="assets/prep-time-healthy-2.html" width=800 height=600 frameBorder=0></iframe>

This histogram clearly shows a similar distribution between the preperation time of healthy and non-healthy foods. There are also some recipes from both groups that have some differences, but we will determine if they are significant in the sections after.

### Bivariate Analysis
Now, we will go over the bivariate analysis. In this scatter plot we see the the correlation between minutes and calories and whether the recipe is healthy or not.

<iframe src="assets/scatter_cals.html" width=800 height=600 frameBorder=0></iframe>

As you can see from the plot, there are no clear trends within the minutes as both groups are similar. However, you can see that healthier foods have slightly lower amount of calories compared to non-healthy recipes. We see this because the blue points are much higher than the red points.


### Interesting Aggregates

The pivot table groups the non-healthy and healthy recipes and finds the mean prep time and rating.

|            | Average Preparation Time | Average Ratings |
| is_healthy |                          |                 |
|------------|--------------------------|------------------|
| False      | 117.060898               | 4.659893         |
| True       | 104.681646               | 4.620729         |

This pivot table shows the mean prepartion time and rating for the non-healthy and healthy recipes. We can see the similarities of the ratings and the slight difference in the mean preparation time (about 12 mins). 

# Assessment of Missingness

### NMAR Analysis

In our dataset, we believe that the `review` column is NMAR (Not Missing At Random) due to the opinions of the reviewers of recipes, and this can be explained by a little bit of psychology. People are more likely to write a review when they are upset, as the brain processes negative emotions more than positive ones. When people are happy, they tend to be content and do not usually write many positive reviews. Thus, we can be led to think that the missingness of a value in the `review` column depends on the value itself as a negative review is more likely to get written than a positive review.

### Missingness Dependency

Because there were missing values in the `ratings` column, we decided to run tests to see if the missingness of this column was dependent on other columns.

> Ratings and Minutes

We first started looking at the distributions of recipe length, as indicated by the `minutes` column, based on the missingness of the `ratings` column. Our initial histogram looked like this:

<iframe src="assets/recipe-length-missingness-1.html" width=800 height=600 frameBorder=0></iframe>

Upon closer inspection that excludes outliers, we get this:

<iframe src="assets/recipe-length-missingness-2.html" width=800 height=600 frameBorder=0></iframe>

Now that we have seen the distributions, we perform a permutation test to see if the ratings are missing at random
**Null hypothesis**: The missingness of `ratings` does not depend on the minutes for each recipe
**Alternative hypothesis**: The missingness of `ratings` depends on the minutes for each recipe
We will use the absolute difference in means as our test statistic

<iframe src="assets/recipe-length-missingness-3.html" width=800 height=600 frameBorder=0></iframe>

After performing a permutation test, shuffling the `ratings` column 1000 times and calculating the absolute difference in mean minutes, we get a p value of .118. Because this is greater than .05, we fail to reject the null hypothesis and do not have convincing evidence that the missingness of `ratings` depends on `minutes`.

> Ratings and Calories

We first started looking at the distributions of recipe length, as indicated by the `calories` column, based on the missingness of the `ratings` column. Our initial histogram looked like this:

<iframe src="assets/calories-by-missingness-1.html" width=800 height=600 frameBorder=0></iframe>

Upon closer inspection that excludes outliers, we get this:

<iframe src="assets/calories-by-missingness-2.html" width=800 height=600 frameBorder=0></iframe>

Now that we have seen the distributions, we perform a permutation test to see if the ratings are missing at random
**Null hypothesis**: The missingness of `ratings` does not depend on the calories in each recipe
**Alternative hypothesis**: The missingness of `ratings` depends on the calories in each recipe
We will use the absolute difference in means as our test statistic

<iframe src="assets/calories-by-missingness-3.html" width=800 height=600 frameBorder=0></iframe>

After performing a permutation test, shuffling the `ratings` column 1000 times and calculating the absolute difference in mean minutes, we get a p value of 0.0. Because this is less than .05, we reject the null hypothesis and have convincing evidence that the missingness of `ratings` depends on `calories`.



# Hypothesis Testing

**Observation:** There is about a 12 minute difference between the prepation times of healthy food and non-healthy food

**Null Hypothesis (h0):** There is no significant difference in the mean preparation time between recipes with the healthy tag and recipes without the healthy tag.

**Alternate Hypothesis (h1):** There is a significant difference in the mean preparation time between recipes with the healthy tag and recipes without the healthy tag.

**Test Statistic:** The mean absolute difference in healthy preparation times and non healthy preparation times 

**Significance Level:** We will be running this permutation test at the 0.05 significance level.

**P-value:** 0.733

**Histogram:**
<iframe src="assets/perm_test_H.html" width=800 height=600 frameBorder=0></iframe>

**Conclusion:** Since our P-Value (0.733) >= our significance level(0.05), we fail to reject the null and conclude that there is not enough evidence to suggest a significant difference in the mean 'minutes' between healthy and non-healthy groups based on the permutation test. The observed difference in means may be attributed to random sampling variability, and the results should be interpreted with caution.