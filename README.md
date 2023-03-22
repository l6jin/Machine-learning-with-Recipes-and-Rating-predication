# Machine-learning-with-Recipes-and-Rating-predication
### Author: Luming (Selina) Jin and Jialing Li (Freya)


### Framing the Problem
#### Problem Identification
Our prediction problem is to predict the rating levels of each recipe (i.e. "bad","good","excellent") using the combined dataframe of Recipes and Rating Data. We categorized the average rating (derived from rating column) into six levels:"bad","ok","good","outstanding","remarkable", "excellent" and tried to predict the rating levels of each recipe. This is a classification problem and our model is a multiclass classifier. We conducted a conditional mean imputation for the missing value of averge rating based on the n_steps since we discovered that the missing mechanism of average rating is MAR based on n_steps.

#### Response variable
Rating level is our response variable because first, it captures the level of satisfaction of users and second, text description is more informative compared to numbers to most of people. 

#### Features overview
At the 'time of prediction', we are able to access to the features like year of the recipe posted, calories, number of ingredients each recipe would take, and names of recipes. We built our classifer based on these acquired features. These information should all be known at the time of our prediction as they are just basic information that associated with each recipe when they are first created. 

#### Evaluation method
We used a "one vs. rest" evaluation with accuracy score to assess our model. This is because the other suitable metrics are not suitable in our multi-class predication model. For example, F1 score only applies to binary classification. We chose to use this evaluation since the number of classes is large and the data is imbalanced. It can provide a simple and interpretable metric for evaluating the model's performance. 


### Baseline Model
For our baseline model, we implemented the DecisionTreeClassifier with default hyperparameters. 

#### Features description and encoding
In our decision tree, we included two features: calories and recipe names. Calories is quantative. Recipe name feature is nominal. For calories, we didn't implement any feature engineerig. For recipe names, we transformed it with CountVectorizer from sklearn.feature_extraction.text. 

#### Baseline Model performance and model description
After we fitted and transformed, the prediction accuracy from our model is around 0.6. Since different average rating levels have different average caloires, we believe it is reasonable to predict average rating levels with calories. For the recipe names, we believe recipe names contain valuable information pertaining to the cuisine type, probable ingredient composition, and cooking techniques utilized. We think that these factors could potentially impact the average rating of a recipe, and therefore, we have incorporated recipe names as a feature in our analysis. Since it is a nominal feature, we used CountVectorizer for our model. So far, we think this is a fair model that has a moderate accuracy. However, we believe with more features implemented, we shall have a better accuracy. 


### Final Model

#### New features and justification to include them
Besides the previous features, we also added another two features: year of the recipes submission date and the number of ingredients each recipe would need. We believe these two features will increase our model perrformace for the following reasons:

First, the year of the recipes submission date embeds the information for the relative ages between each recipe. If a recipe was submitted in the early years, then more people might have tried it, which in result would affect the average ratings. If the recipe is relativly new, then it is possible that most people haven't tried it. And the people who tried and rated those recipes might have a tendency to give a partisan rating, which would affect the average ratings. 
 
Second, the number of ingredients shows how complicated each recipe is. We reckoned that when a recipe has more ingredients, the food itself would take more efforts to make. With more efforts, the food typically should taste better and have a higher rating. On the other hands, if the recipe is too complicated to make, people then may have an averse attitude towards it. This would then cause it to have a lower average rating level. 
 
Year of the recipes submission date is an ordinal feature. Therefore, we used OneHotEncoder for its feature engineering. The number of ingredients each recipe would need is a quantative feature and we left it as is. With this additional two features, we were able to update our pipeline. 

#### Final Model selection and Hyperparameters 

After comparing the performance accuracy from RandomForestClassifier and DecisionTreeClassifier, we noticed that RandomForestClassifier outperfoms DecisionTreeClassifier with higher accuracy. This is possiblely because 1.RandomForestClassifer is robust to outliers since the model averages over the predictions of multiple decision trees 2. DecisionTreeClassifier can be prone to overfittinbg while RandomForestClassifier reduces overfitting by constructing multiple decision trees and averaging their predictions.

Therefore, in order to optimize the power of prediction and identify the most related features with the classification, we implemented RandomforestClassifer. Our model utilizes RandomforestClassifer with the followng four features: calories, recipe names, year of recipes submission date, and the number of ingredients for each recipes. By performing GridSearchCV from sklearn,model, we get the best performing hyperparameters: criterion='gini', max_depth=5, min_samples_split=5. 

We chose these hyperparameters to tune for the following reasons. Max_depth determines the overall-fit of our model. We want to make sure it is not too large to overfit the data. We also need to make sure it will not be too shallow. For min_samples_split, we also want to tune it so that we can find an apporiate number that would not cause overfit or underfit of the training data. This can help us fit the optimal number of samples for splitting internal nodes.
We also tuned criterion becuase it specifies what the model would use to measure the quality of a split. Tuning this can help us find the optimal metric for splitting nodes based on the data chacracteristics.

#### Final model performance
Our final model has a better performance comparing to our baseline model. After testing the accuracy rate by OneVsRestClassifier, we can see that our accuracy score is almost 0.76. Which is better than what we saw in baseline model (around 0.6). 


### Fairness Analysis
Our targed variable for the fairness analysis is the 'year' column. We noitced the collected year is ranging from 2008-2018. We divided the year into Group X which stands for the recipes that were submitted between year 2008-2013, and Group Y which stands for the recipes that were submitted between year 2014-2018. The evaluation metric is the accuracy matrix. 

We ran a permutation test to investigate whether there is a significant difference in accuracy between two group.
##### Null Hypothesis: Our model is fair. Our classifier's accuracy is the same for both before 2013 group and after 2013 group, and any differences are due to chance.

##### Alternative Hypothesis: Our model is unfair. The classifier's accuracy is higher for before 2013 group.

##### Test statistic: Absolute Difference in accuracy (before 2013-after 2013)

##### Significance level: 0.05

#### p-value and conclusion
Our resulted p-value is larger than 0.05 significance level so we failed to reject the null hypothesis. That is to say, it seems like the difference in accuracy across the two groups is not statistically significant. Our model is fair in terms years before 2013 and years after 2013. Our classifier is likely to achieve an accuracy parity between two time periods. 


Below is the visulization of the our permutation test for fairness.

<iframe src="assests/fairness_permutation.html" width=800 height=600 frameBorder=0></iframe>



 