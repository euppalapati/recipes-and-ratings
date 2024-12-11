## Exploratory and Predictive Analyses on Recipes and Ratings


### Introduction
The food blogging industry is thriving, with more people than ever sharing their passion for cooking and baking. Enthusiasts and professionals alike are diving into the art of creating, reviewing, and perfecting dishes, making food blogging a vibrant and influential space. Here, we'll explore **how key aspects of a recipe affect its overall rating**. Understanding this question is valuable not only for the avid chefs who aim to refine their recipes, but also for those looking to predict trends or create better recipe platforms. This dive into recipe success factors is both informative and engaging for anyone who loves food and data.

The first dataset we will explore is `recipes`, which contains recipes posted at Food.com since 2008. This dataset contains 83782 recipes (rows) and the following 12 columns: `name`, `id`, `minutes`, `contributor_id`, `submitted`, `tags`, `nutrition`, `n_steps`, `steps`, `description`, `ingredients`, and `n_ingredients`. For the purposes of this project, we will only be using the following columns:
`id`: Recipe ID.
`submitted`: Date recipe was submitted.
`tags`: Food.com tags for recipe.
`nutrition`: Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value”.
`n_steps`: Number of steps in recipe.
`n_ingredients`: Number of ingredients in recipe.

The second dataset we will use for analysis is `ratings`, which contains reviews and ratings submitted for recipes on Food.com. This dataset contains 731927 ratings/reviews (rows) and the following 5 columns: `user_id`, `recipe_id`, `date`, `rating`, and `review`. For the purposes of this project, only the following columns are relevant:
`recipe_id`: Recipe ID.
`rating`: Rating given.


### Data Cleaning and Exploratory Data Analysis
#### Data Cleaning
The following cleaning steps were conducted to ensure efficient and effective analysis on the data.

1. Left merged the `recipes` and `ratings` datasets on `id` and `recipe_id`, respectively, which allowed for each review to correspond to its recipe.
2. Filled all ratings of 0 with `NaN`, because 0 is the rating automatically assigned when the reviewer only adds a comment. This helped streamline missingness analysis.
3. Grouped the merged dataset on `id` and `rating` to find the average rating for each recipe, then added these values to a new column in the original `recipes` DataFrame. This allowed for each recipe in the original dataset to have a corresponding average rating, which helps us explore our central question.
4. Converted the `tags`, `steps`, and `ingredients` columns from strings to lists of strings, which lets us pull specific values from the lists.
5. Converted the `submitted` column to datetime, helping us conduct analysis on trends over time.
6. Converted `nutrition` to a list of strings, then assign each aspect of the nutrition information (as described in the introduction) to their own columns. By splitting up this data, we could see how individual nutritional factors play a part in recipe rating.
7. Created the `sugar_prop` column, which contains the proportion of sugar per recipe in relation to the number of calories, which allows us to better estimate the sugar content of individual recipes.
8. Created the `year_submitted` column, which extracts the year from each date in `submitted` for looking at general trends over the decade.

The head of the merged and cleaned dataset is shown below.


#### Univariate Analysis
Embed at least one plotly plot you created in your notebook that displays the distribution of a single column (see Part 2: Report for instructions). Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one univariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)

#### Bivariate Analysis
Embed at least one plotly plot that displays the relationship between two columns. Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one bivariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)

#### Interesting Aggregates
Embed at least one grouped table or pivot table in your website and explain its significance.


### Assessment of Missingness
#### NMAR Analysis
State whether you believe there is a column in your dataset that is NMAR. Explain your reasoning and any additional data you might want to obtain that could explain the missingness (thereby making it MAR). Make sure to explicitly use the term “NMAR.”

#### Missingness Dependency
Present and interpret the results of your missingness permutation tests with respect to your data and question. Embed a plotly plot related to your missingness exploration; ideas include:
• The distribution of column Y when column X is missing and the distribution of column Y when column X is not missing, as was done in Lecture 8.
• The empirical distribution of the test statistic used in one of your permutation tests, along with the observed statistic.


### Hypothesis Testing
Clearly state your null and alternative hypotheses, your choice of test statistic and significance level, the resulting 
p-value, and your conclusion. Justify why these choices are good choices for answering the question you are trying to answer.

Optional: Embed a visualization related to your hypothesis test in your website.

Tip: When making writing your conclusions to the statistical tests in this project, never use language that implies an absolute conclusion; since we are performing statistical tests and not randomized controlled trials, we cannot prove that either hypothesis is 100% true or false.


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