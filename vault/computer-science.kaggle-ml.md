---
id: WRMwiWiI4hRbBgyg
title: Kaggle Ml
desc: ''
updated: 1628778527914
created: 1628775555636
---

VIEW

# Kaggle 30 days challange Python  and ML


import

help(): what a built-in function does/can do?
dir() : to check all the names (magic methods) available in a namespace/module/ what I can do with a ...?
type() : what this object is?


Models: 
Decision tree
[Random Forests](https://www.kaggle.com/dansbecker/random-forests)


fit/train the model
predict
mae


---

## Building a Model:

0. **Prepare data**: 
```python
import pandas as pd

# Load data
# save filepath to variable for easier access
melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
# read the data and store data in DataFrame titled melbourne_data
melbourne_data = pd.read_csv(melbourne_file_path) 
# dropna drops missing values (think of na as "not available")
melbourne_data = melbourne_data.dropna(axis=0)

# Optionals:
# 1. print a summary of the data in Melbourne data
# melbourne_data.describe()
# 2. To choose variables/columns for prediction, we'll need to see a list of all columns in the dataset.
# melbourne_data.columns

# Choose target and features:
# X = chosen "features" or columns
melbourne_features = ['Rooms', 'Bathroom', 'Landsize', 'Lattitude', 'Longtitude']
X = melbourne_data[melbourne_features]
# y = prediction target (column I want to predict)
y = melbourne_data.Price

# X.describe()
# X.head()

```

1. **Split**: 

```python
# Import the train_test_split function and uncomment
from sklearn.model_selection import train_test_split

# split data into training and validation data, for both features and target
# The split is based on a random number generator. Supplying a numeric value to
# the random_state argument guarantees we get the same split every time we run this script.
train_X, val_X, train_y, val_y = train_test_split(X, y, random_state = 1)
```

1. **Define**: What type of model will it be? A decision tree? Some other type of model? Some other parameters of the model type are specified too.

Option 1:
```python
from sklearn.tree import DecisionTreeRegressor

# Define model. Specify a number for random_state to ensure same results each run
melbourne_model = DecisionTreeRegressor(random_state=1)
```

...or option 2:

```python
from sklearn.ensemble import RandomForestRegressor

melbourne_model = RandomForestRegressor(random_state=1)
```

Option 3: ?...

2. **Fit**: Capture patterns from provided data. This is the heart of modeling.

```python
# Fit model
melbourne_model.fit(X, y)
# or
# melbourne_model.fit(train_X, train_y)
```

3. **Predict**: Just what it sounds like...

Model validation:
There are many metrics for summarizing model quality, but we'll start with one called Mean Absolute Error (also called MAE) where `error=actual−predicted`

```python
from sklearn.metrics import mean_absolute_error

# Predict with all validation observations
val_predictions = melbourne_model.predict(val_X)
# print the top few validation predictions
print( iowa_model.predict(val_X.head()) )
# print the top few actual prices from validation data
print(val_y.head().tolist())
```

4. **Evaluate**: Determine how accurate the model's predictions are.

```python
from sklearn.metrics import mean_absolute_error

# We can use a utility function to help compare MAE scores from different values for max_leaf_nodes:
def get_mae(max_leaf_nodes, train_X, val_X, train_y, val_y):
    model = DecisionTreeRegressor(max_leaf_nodes=max_leaf_nodes, random_state=0)
    model.fit(train_X, train_y)
    preds_val = model.predict(val_X)
    mae = mean_absolute_error(val_y, preds_val)
    return(mae)
val_mae = mean_absolute_error(val_y, val_predictions)

mean_absolute_error(val_y, predicted_home_prices)
```

## COmpare different tree sizes and _fit_ model using all data

````python
candidate_max_leaf_nodes = [5, 25, 50, 100, 250, 500]
# Write loop to find the ideal tree size from candidate_max_leaf_nodes
# I AM USING A LIST COMPREHENSION
scores = {leaf_size: get_mae(leaf_size, train_X, val_X, train_y, val_y) for leaf_size in candidate_max_leaf_nodes}

# Store the best value of max_leaf_nodes (it will be either 5, 25, 50, 100, 250 or 500)
best_tree_size = min(scores, key=scores.get)
print(best_tree_size)

# Fill in argument to make optimal size and uncomment
final_model = DecisionTreeRegressor(max_leaf_nodes=best_tree_size, random_state=1)

# fit the final model and uncomment the next two lines
final_model.fit(X,y)
```

At the end of this step, you will understand the concepts of underfitting and overfitting, and you will be able to apply these ideas to make your models more accurate.

Experimenting With Different Models
Now that you have a reliable way to measure model accuracy, you can experiment with alternative models and see which gives the best predictions. But what alternatives do you have for models?

You can see in scikit-learn's documentation that the decision tree model has many options (more than you'll want or need for a long time). The most important options determine the tree's depth. Recall from the first lesson in this course that a tree's depth is a measure of how many splits it makes before coming to a prediction. This is a relatively shallow tree

Depth 2 Tree

In practice, it's not uncommon for a tree to have 10 splits between the top level (all houses) and a leaf. As the tree gets deeper, the dataset gets sliced up into leaves with fewer houses. If a tree only had 1 split, it divides the data into 2 groups. If each group is split again, we would get 4 groups of houses. Splitting each of those again would create 8 groups. If we keep doubling the number of groups by adding more splits at each level, we'll have  210  groups of houses by the time we get to the 10th level. That's 1024 leaves.

When we divide the houses amongst many leaves, we also have fewer houses in each leaf. Leaves with very few houses will make predictions that are quite close to those homes' actual values, but they may make very unreliable predictions for new data (because each prediction is based on only a few houses).

This is a phenomenon called overfitting, where a model matches the training data almost perfectly, but does poorly in validation and other new data. On the flip side, if we make our tree very shallow, it doesn't divide up the houses into very distinct groups.

At an extreme, if a tree divides houses into only 2 or 4, each group still has a wide variety of houses. Resulting predictions may be far off for most houses, even in the training data (and it will be bad in validation too for the same reason). When a model fails to capture important distinctions and patterns in the data, so it performs poorly even in training data, that is called underfitting.

Since we care about accuracy on new data, which we estimate from our validation data, we want to find the sweet spot between underfitting and overfitting. Visually, we want the low point of the (red) validation curve in the figure below.

underfitting_overfitting

Example
There are a few alternatives for controlling the tree depth, and many allow for some routes through the tree to have greater depth than other routes. But the max_leaf_nodes argument provides a very sensible way to control overfitting vs underfitting. The more leaves we allow the model to make, the more we move from the underfitting area in the above graph to the overfitting area.


#### Problem with "In-sample" scores:

- Since models' practical value come from making predictions on new data, we measure performance on data that wasn't used to build the model. The most straightforward way to do this is to exclude some data from the model-building process, and then use those to test the model's accuracy on data it hasn't seen before. This data is called validation data.


- The scikit-learn library has a function train_test_split to break up the data into two pieces. We'll use some of that data as training data to fit the model, and we'll use the other data as validation data to calculate **mean_absolute_error**

***random_state:** Many machine learning models allow some randomness in model training. Specifying a number for random_state ensures you get the same results in each run. This is considered a good practice. You use any number, and model quality won't depend meaningfully on exactly what value you choose.

[Models can suffer from either:](https://www.kaggle.com/dansbecker/underfitting-and-overfitting)
Overfitting: capturing spurious patterns that won't recur in the future, leading to less accurate predictions, or
Underfitting: failing to capture relevant patterns, again leading to less accurate predictions.