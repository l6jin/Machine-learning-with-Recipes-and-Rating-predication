# Machine-learning-with-Recipes-and-Rating-predication
### Author: Luming (Selina) Jin and Jialing Li (Freya)


### Framing the Problem

### Baseline Model

For our baseline model, we implemented the DecisionTreeClassifier with default hyperparameters. In our decision tree, we included two features: calories and reciepe names. Calories is quantative. Reciepe name feature is nomial. For calories, we didn't implement any feature engineerig. For recipe names, we transformed it with CountVectorizer from sklearn.feature_extraction.text. 

After we fit and transform, the prediction accuracy from our model is around 0.6. Since different average rating levels has different average caloires, we believe it is reasonable to predict average rating levels with calories. For the recipe names, we believe recipe names contain valuable information pertaining to the cuisine type, probable ingredient composition, and cooking techniques utilized. We think that these factors could potentially impact the average rating of a recipe, and therefore, we have incorporated recipe names as a feature in our analysis. Since it is a nomial feature, we used CountVectorizer for our model. So far, we think this is a fair model that has a moderate accuracy. However, we believe with more features implemented, we shall have a better accuracy. 


### Final Model 



