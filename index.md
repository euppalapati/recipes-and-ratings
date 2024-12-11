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

The head of the merged and cleaned DataFrame is shown below.

TABLE

#### Univariate Analysis
Below is the distribution of the dates that recipes were submitted. The distribution skewed right, which indicates that most of the recipes in this dataset were published during earlier years. As the date becomes more recent, the proportion of recipes in the dataset decreases.

PLOTLY

Below is the distribution of the sugar proportions of recipes that were submitted. The distribution is skewed right, indicating that most of the recipes in the dataset have lower sugar proportions. As the sugar proportion increases, the proportion of recipes in the dataset decreases.

PLOTLY

#### Bivariate Analysis
Below is a grouped boxplot of the proportion of sugar in recipes per year. We see that the number of recipes with high sugar proportions generally decreases as the year submitted becomes more recent.

PLOTLY

#### Interesting Aggregates
Below is a grouped table with aggregate statistics describing the relationship between the number of steps in a recipe and its sugar proportion and calorie count.

TABLE

By plotting the statistics of sugar proportion across number of steps, we see that the median tends to increase and fluctuate more as number of steps increase.

PLOTLY

By plotting the statistics of calorie count across number of steps, we see that the median stays consistent as number of steps increase.

PLOTLY


### Assessment of Missingness
#### NMAR Analysis
I believe the `description` column of the dataset is not missing at random (NMAR). Oftentimes, recipe descriptions include additional information from the chef to persuade their audience to try their recipe. Some common examples include mouthwatering descriptions of the food, anecdotes of fond memories associated with the recipe, and boasts of overhwleming positive feedback. However, if chefs/recipe bloggers deem their recipe title and Food.com tags to provide enough information, or they don't have any stories to tell about their recipe, they are less likely to write a lengthy description about them. In order to explain missingness, obtaining data on whether the recipe holds emotional significance for the chef and running missingness dependency on that column would help us understand whether this column's missingness is missing at random (MAR) depending on the chef/blogger's connection to the recipe.

#### Missingness Dependency
Below is a kernel density estimation (KDE) plot of the calorie count of recipes that are missing vs. are not missing average rating.

PLOTLY

**Null Hypothesis**: The missingness of average ratings does not depend on the calorie count of the recipe.

**Alternate Hypothesis**: The missingness of average ratings does depend on the calorie count of the recipe.

**Test Statistic**: Absolute median difference in calorie count between missing and non-missing groups.

**Significance Level**: 0.05

By running permutation tests, we find that the missingness of ratings depends on the calorie count. Below is an empirical distribution of the absolute median difference in calorie count.

PLOTLY

Below is a kernel density estimation (KDE) plot of the number of ingredients of recipes that are missing vs. are not missing average rating.

PLOTLY

**Null Hypothesis**: The missingness of average ratings does not depend on the number of ingredients in the recipe.

**Alternate Hypothesis**: The missingness of average ratings does depend on the number of ingredients in the recipe.

**Test Statistic**: Absolute median difference in number of ingredients between missing and non-missing groups.

**Significance Level**: 0.05

By running permutation tests, we find that the missingness of ratings does not depend on the number of ingredients. Below is an empirical distribution of the absolute median difference in number of ingredients.

PLOTLY

### Hypothesis Testing
In this section, we will explore the question **does the sugar proportion of recipes decrease over time?**

**Null Hypothesis**: Sugar proportions in recipes are consistent over time.

**Alternative Hypothesis**: Recent recipes have lower sugar proportions than older recipes.

**Test Statistic**: Median difference in proportion of sugar (recent - older).

**Significance Level**: 0.05.

We are using the median difference in proportion of sugar to observe a one-tailed difference. Since the data for both sugar proportions and dates are skewed right, we will use the median instead of the mean so that our statistic is more robust to outliers.

Below is an empirical distribution of the median difference in sugar proportion between recent and older recipes. With a p-value of 0.002, we reject the null hypothesis and conclude that sugar proportions are significantly lower in more recent recipes.

PLOTLY


### Framing a Prediction Problem
Clearly state your prediction problem and type (classification or regression). If you are building a classifier, make sure to state whether you are performing binary classification or multiclass classification. Report the response variable (i.e. the variable you are predicting) and why you chose it, the metric you are using to evaluate your model and why you chose it over other suitable metrics (e.g. accuracy vs. F1-score).

Note: Make sure to justify what information you would know at the “time of prediction” and to only train your model using those features. For instance, if we wanted to predict your final exam grade, we couldn’t use your Final Project grade, because the project is only due after the final exam! Feel free to ask questions if you’re not sure.


### Baseline Model
Describe your model and state the features in your model, including how many are quantitative, ordinal, and nominal, and how you performed any necessary encodings. Report the performance of your model and whether or not you believe your current model is “good” and why.

Tip: Make sure to hit all of the points above: many projects in the past have lost points for not doing so.


### Final Model
State the features you added and why they are good for the data and prediction task. Note that you can’t simply state “these features improved my accuracy”, since you’d need to choose these features and fit a model before noticing that – instead, talk about why you believe these features improved your model’s performance from the perspective of the data generating process.

Describe the modeling algorithm you chose, the hyperparameters that ended up performing the best, and the method you used to select hyperparameters and your overall model. Describe how your Final Model’s performance is an improvement over your Baseline Model’s performance.

Optional: Include a visualization that describes your model’s performance, e.g. a confusion matrix, if applicable.


### Fairness Analysis
Clearly state your choice of Group X and Group Y, your evaluation metric, your null and alternative hypotheses, your choice of test statistic and significance level, the resulting 
p-value, and your conclusion.

Optional: Embed a visualization related to your permutation test in your website.