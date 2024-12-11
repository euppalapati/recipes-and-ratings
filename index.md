## Exploratory and Predictive Analyses on Recipes and Ratings


### Introduction
The food blogging industry is thriving, with more people than ever sharing their passion for cooking and baking. Enthusiasts and professionals alike are diving into the art of creating, reviewing, and perfecting dishes, making food blogging a vibrant and influential space. Here, we'll explore **how key aspects of a recipe affect its overall rating**. Understanding this question is valuable not only for the avid chefs who aim to refine their recipes, but also for those looking to predict trends or create better recipe platforms. This dive into recipe success factors is both informative and engaging for anyone who loves food and data.

The first dataset we will explore is `recipes`, which contains recipes posted at Food.com since 2008. This dataset contains 83782 recipes (rows) and the following 12 columns: `name`, `id`, `minutes`, `contributor_id`, `submitted`, `tags`, `nutrition`, `n_steps`, `steps`, `description`, `ingredients`, and `n_ingredients`. For the purposes of this project, we will only be using the following columns:

| `id` | Recipe ID |
| `submitted` | Date recipe was submitted |
| `tags` | Food.com tags for recipe |
| `nutrition` | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `n_steps` | Number of steps in recipe |
| `n_ingredients` | Number of ingredients in recipe |

The second dataset we will use for analysis is `ratings`, which contains reviews and ratings submitted for recipes on Food.com. This dataset contains 731927 ratings/reviews (rows) and the following 5 columns: `user_id`, `recipe_id`, `date`, `rating`, and `review`. For the purposes of this project, only the following columns are relevant:

| `recipe_id` | Recipe ID |
| `rating` | Rating given |


### Data Cleaning and Exploratory Data Analysis
#### Data Cleaning
The following cleaning steps were conducted to ensure efficient and effective analysis on the data.

1. Left merged the `recipes` and `ratings` datasets on `id` and `recipe_id`, respectively, which allowed for each review to correspond to its recipe.
2. Filled all ratings of 0 with `NaN`, because 0 is the rating automatically assigned when the reviewer only adds a comment. This helped streamline missingness analysis.
3. Grouped the merged dataset on `id` and `rating` to find the average rating for each recipe, then added these values to a new column in the original `recipes` DataFrame. This allowed for each recipe in the original dataset to have a corresponding average rating, which helps us explore our central question.
4. Converted the `tags`, `steps`, and `ingredients` columns from strings to lists of strings, which lets us pull specific values from the lists.
5. Converted the `submitted` column to datetime, helping us conduct analysis on trends over time.
6. Converted `nutrition` to a list of strings, then assign each aspect of the nutrition information (as described in the introduction) to their own columns. By splitting up this data, we can see how individual nutritional factors play a part in recipe rating.
7. Created the `sugar_prop` column, which contains the proportion of sugar per recipe in relation to the number of calories, which allows us to better estimate the sugar content of individual recipes.
8. Created the `year_submitted` column, which extracts the year from each date in `submitted` for looking at general trends over the decade.

The head of the merged and cleaned DataFrame is shown below, with only columns relevant to this analysis displayed.

| name                                 |   submitted_year |   protein_pdv |   cal_count |   sugar_prop | tags                                                                                                                                                                                                                                                                                               |   n_steps |   n_ingredients |   avg_rating |
|:-------------------------------------|-----------------:|--------------:|------------:|-------------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------:|----------------:|-------------:|
| 1 brownies in the world... |             2008 |             3 |       138.4 |    0.361272  | ['60-minutes-or-less', ...]                                                                        |        10 |               9 |            4 |
| 1 in canada chocolate chip cookies   |             2011 |            13 |       595.1 |    0.354562  | ['60-minutes-or-less', ...]                                                                                                                                      |        12 |              11 |            5 |
| 412 broccoli casserole               |             2008 |            22 |       194.8 |    0.0308008 | ['60-minutes-or-less', ...]                                                                                                                                               |         6 |               9 |            5 |
| millionaire pound cake               |             2008 |            20 |       878.3 |    0.371172  | ['time-to-make', ...] |         7 |               7 |            5 |
| 2000 meatloaf                        |             2012 |            29 |       267   |    0.0449438 | ['time-to-make', ...]                                                                                                                                             |        17 |              13 |            5 |

#### Univariate Analysis
Below is the distribution of the dates that recipes were submitted. The distribution skewed right, which indicates that most of the recipes in this dataset were published during earlier years. As the date becomes more recent, the proportion of recipes in the dataset decreases.
<iframe
  src="assets/fig1a.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Below is the distribution of the sugar proportions of recipes that were submitted. The distribution is skewed right, indicating that most of the recipes in the dataset have lower sugar proportions. As the sugar proportion increases, the proportion of recipes in the dataset decreases.
<iframe
  src="assets/fig1c.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### Bivariate Analysis
Below is a grouped boxplot of the proportion of sugar in recipes per year. We see that the number of recipes with high sugar proportions generally decreases as the year submitted becomes more recent.
<iframe
  src="assets/fig2b.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### Interesting Aggregates
Below is a grouped table with aggregate statistics describing the relationship between the number of steps in a recipe and its sugar proportion and calorie count.

|   n_steps |   sugar_prop_mean |   sugar_prop_median |   sugar_prop_min |   sugar_prop_max |   cal_count_mean |   cal_count_median |   cal_count_min |   cal_count_max |
|----------:|------------------:|--------------------:|-----------------:|-----------------:|-----------------:|-------------------:|----------------:|----------------:|
|         1 |          0.276673 |           0.186546  |                0 |          1.07898 |          285.133 |             161.4  |             0.1 |          4541.2 |
|         2 |          0.26871  |           0.157729  |                0 |          1.04322 |          293.095 |             181.1  |             0   |          6730.7 |
|         3 |          0.241514 |           0.124146  |                0 |          1.07674 |          307.189 |             197.55 |             0   |          5976.1 |
|         4 |          0.214639 |           0.107362  |                0 |          1.06583 |          344.003 |             230.1  |             0   |         45609   |
|         5 |          0.195669 |           0.0902151 |                0 |          1.06583 |          358.074 |             256.1  |             0   |         17551.6 |

By plotting the statistics of sugar proportion across number of steps, we see that the median tends to increase and fluctuate more as number of steps increase.
<iframe
  src="assets/fig3a.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
By plotting the statistics of calorie count across number of steps, we see that the median stays consistent as number of steps increase.
<iframe
  src="assets/fig3b.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### Assessment of Missingness
#### NMAR Analysis
I believe the `description` column of the dataset is not missing at random (NMAR). Oftentimes, recipe descriptions include additional information from the chef to persuade their audience to try their recipe. Some common examples include mouthwatering descriptions of the food, anecdotes of fond memories associated with the recipe, and boasts of overhwleming positive feedback. However, if chefs/recipe bloggers deem their recipe title and Food.com tags to provide enough information, or they don't have any stories to tell about their recipe, they are less likely to write a lengthy description about them. In order to explain missingness, obtaining data on whether the recipe holds emotional significance for the chef and running missingness dependency on that column would help us understand whether this column's missingness is missing at random (MAR) depending on the chef/blogger's connection to the recipe.

#### Missingness Dependency
Below is a kernel density estimation (KDE) plot of the calorie count of recipes that are missing vs. are not missing average rating.
<iframe
  src="assets/fig4a.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
*Null Hypothesis*: The missingness of average ratings does not depend on the calorie count of the recipe.

*Alternate Hypothesis*: The missingness of average ratings does depend on the calorie count of the recipe.

*Test Statistic*: Absolute median difference in calorie count between missing and non-missing groups.

*Significance Level*: 0.05

By running permutation tests shuffling the missingness of average rating 1000 times, we find that the missingness of ratings depends on the calorie count (p-value of 0.0). Below is an empirical distribution of the absolute median difference in calorie count.
<iframe
  src="assets/dist1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Below is a kernel density estimation (KDE) plot of the number of ingredients of recipes that are missing vs. are not missing average rating.
<iframe
  src="assets/fig4b.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
*Null Hypothesis*: The missingness of average ratings does not depend on the number of ingredients in the recipe.

*Alternate Hypothesis*: The missingness of average ratings does depend on the number of ingredients in the recipe.

*Test Statistic*: Absolute median difference in number of ingredients between missing and non-missing groups.

*Significance Level*: 0.05

By running a permutation test shuffling the missingness of average rating 1000 times, we find that the missingness of ratings does not depend on the number of ingredients (p-value of 1.0). Below is an empirical distribution of the absolute median difference in number of ingredients.
<iframe
  src="assets/dist2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### Hypothesis Testing
In this section, we will explore the question **does the sugar proportion of recipes decrease over time?**

*Null Hypothesis*: Sugar proportions in recipes are consistent over time.

*Alternative Hypothesis*: Recent recipes have lower sugar proportions than older recipes.

*Test Statistic*: Median difference in proportion of sugar (recent - older).

*Significance Level*: 0.05.

We are using the median difference in proportion of sugar to observe a one-tailed difference. Since the data for both sugar proportions and dates are skewed right, we will use the median instead of the mean so that our statistic is more robust to outliers.

Below is an empirical distribution of the median difference in sugar proportion between recent and older recipes. With a p-value of 0.002, we reject the null hypothesis and conclude that sugar proportions are significantly lower in more recent recipes.
<iframe
  src="assets/dist3.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### Framing a Prediction Problem
The *multiclass classification* problem explored in the following sections is the prediction of average rating of recipes. By rounding each recipe's average rating to the nearest whole number, we essentially allow rating to become a categorical variable, with possible outcomes being [1, 2, 3, 4, 5] after dropping null values.

By choosing the average rating to predict, we can generalize how different aspects of a recipe influence its perception to Food.com users. Previously, we explored the relationships between different aspects of recipes, and now we can use those findings to build a model. This model can be especially useful for food bloggers and chefs, so they have a better understanding of their audience's preferences.

Since the distribution of average ratings is skewed left, we know that more of the ratings are higher (around the 4-5 range). This makes accuracy a poor choice for an evaluation metric because it does not account for class distribution. Instead, we will use F-1 score, which is the harmonic mean of precision and recall for a classification model.

At time of prediction, we would have access to all of the information included in the recipes dataset, since the recipe would be posted with all relevant information prior to rating.


### Baseline Model
For the baseline model, we will use a DecisionTree classifier and split the data points into training and test sets using `sklearn`'s `train_test_split`. The features used in this model are `sugar_prop` and `protein_pdv`, which both contain quantitative numerical values. Since low-sugar and high-protein foods are trending, these attributes can be indicative of what recipes are rated higher.

According to the FDA, 20% of daily value or more of a single nutrient makes that food considered "high" in that nutrient. By using a Binarizer on the `protein_pdv` column, we encode these values to determine if a recipe is considered "high-protein" or not. We will leave `sugar_prop` as is.

The test F-1 score for this model is 0.5688, with individual score for each category as follows: 0.0000, 0.0000, 0.0000, 0.0080, and 0.8180. This means that the model performs better for classifying 5s than any other rating, which is likely because there is a higher proportion of 5-star ratings in the dataset. This F-1 score indicates that there is somewhat of a trade-off between precision and recall, and that the model is average but not necessarily "good".


### Final Model
For the final model, we will switch to using a RandomForest classifier––which is essentially multiple DecisionTrees "voting" on a classification––to decrease variance. We will also use `carbs_pdv` (quantitative numerical values), `time_tags` (categorical values), `cal_count` (quantitative numerical values), `n_steps` (quantitative numerical values), and `submitted_year` (quantitative numerical values) in addition to `sugar_prop` and the Binarized `protein_pdv`. Likewise to `sugar_prop` and `protein_pdv`, the `carbs_pdv` and `cal_count` columns can provide useful insight into whether recipes are rated higher, since low-carb and low-calorie foods are popular in today's diet-centered culture. As we saw before, `submitted_year` and `sugar_prop` are inversely related, so including this column can give our model more time context. Lastly, it is likely that longer recipes may receive lower ratings, because many people might not have the time or patience for them. As such, we will include `n_steps` and encoded `time_tags` for our model.

Imputing values with the median in all of the quantitative columns allows for the dataset to be complete with plausible values. The values in `carbs_pdv` are on a large scale, so we will use StandardScaler to conduct z-score standardization of this column. `time_tags` is a pre-transformed column that finds tags from the `tags` column that have the word "minutes". We then use OrdinalEncoder to transform the values ['15-minutes-or-less', '30-minutes-or-less', '60-minutes-or-less', 'longer'] into ordinal numerical values. Since the range in `cal_count` is very large, and there are extreme outliers, we can use RobustScaler to transform this column. We will leave the remaining columns as is.

The hyperparameters designated to optimize are the `max_depth` and `n_estimators` of the RandomForest, and a GridSearchCV was used to find them in order to avoid overfitting the data. The best performing `max_depth` and `n-estimators` were 100 and 50, respectively.

The test F-1 score for this model is 0.6730, with individual score for each category as follows: 0.0000, 0.0000, 0.0028, 0.1006, and 0.8025. While this model still performs better for higher ratings, its performance for 3s and 4s increased, indicating that the new features helped generalize the classification. Additionally, the overall F-1 score for the test set increased, which means this model was better at balancing precision and recall than the baseline model was.

Below is a confusion matrix for the final model's test set classification.
<iframe
  src="assets/cm.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### Fairness Analysis
For fairness analysis, we will split recipes into groups of *short recipes* (30 minutes or shorter) and *long recipes* (longer than 30 minutes). We will evaluate whether the model performs similarly for these two groups based on precision parity, because it is important for the model to be correct about its classification so that chefs/bloggers can clearly understand how aspects of their recipe will affect their audience's ratings.

*Null Hypothesis*: The model is fair; the precision for short and long recipes are roughly equal, and any differences are due to random chance.

*Alternative Hypothesis*: The model is unfair; its precision for short recipes is better than long recipes.

*Test Statistic*: Difference in precision (short - long).

*Significance Level*: 0.05.

Below is an empirical distribution of the difference in precision scores of the recipes. With a p-value of 0.012, we reject the null hypothesis and conclude that the model's precision is significantly better for short recipes compared to long ones.
<iframe
  src="assets/dist4.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>