# MLBasics
Starting Date: Aug 14, 2021

# 1. Supervised Learning
## Decision Trees
Decision tree builds regression or classification models in the form of a tree structure. It breaks down a dataset into smaller and smaller subsets while at the same time an associated decision tree is incrementally developed.

<details>
  <summary>simple model training</summary>

  ```python
  # Code you have previously used to load data
  import pandas as pd
  from sklearn.metrics import mean_absolute_error
  from sklearn.model_selection import train_test_split
  from sklearn.tree import DecisionTreeRegressor


  # Path of the file to read
  iowa_file_path = '../input/home-data-for-ml-course/train.csv'

  home_data = pd.read_csv(iowa_file_path)
  # Create target object and call it y
  y = home_data.SalePrice
  # Create X
  features = ['LotArea', 'YearBuilt', '1stFlrSF', '2ndFlrSF', 'FullBath', 'BedroomAbvGr', 'TotRmsAbvGrd']
  X = home_data[features]

  # Split into validation and training data
  train_X, val_X, train_y, val_y = train_test_split(X, y, random_state=1)

  # Specify Model
  iowa_model = DecisionTreeRegressor(random_state=1)
  # Fit Model
  iowa_model.fit(train_X, train_y)

  # Make validation predictions and calculate mean absolute error
  val_predictions = iowa_model.predict(val_X)
  val_mae = mean_absolute_error(val_predictions, val_y)
  print("Validation MAE: {:,.0f}".format(val_mae))

  def get_mae(max_leaf_nodes, train_X, val_X, train_y, val_y):
    model = DecisionTreeRegressor(max_leaf_nodes=max_leaf_nodes, random_state=0)
    model.fit(train_X, train_y)
    preds_val = model.predict(val_X)
    mae = mean_absolute_error(val_y, preds_val)
    return(mae)

  cd = [5, 25, 50, 100, 250, 500]
  # Write loop to find the ideal tree size from candidate_max_leaf_nodes

  scores = {leaf_size: get_mae(leaf_size, train_X, val_X, train_y, val_y) for leaf_size in cd}

  # Store the best value of max_leaf_nodes (it will be either 5, 25, 50, 100, 250 or 500)
  best_tree_size = min(scores, key=scores.get)

  # Fill in argument to make optimal size and uncomment
  final_model = DecisionTreeRegressor(max_leaf_nodes = best_tree_size, random_state=1)

  # fit the final model and uncomment the next two lines
  final_model.fit(X, y)
  val_predictions = final_model.predict(val_X)
  print(mean_absolute_error(val_y, val_predictions))
  ```
  </details>

## Random Forests
The random forest uses many trees, and it makes a prediction by averaging the predictions of each component tree. It generally has much better predictive accuracy than a single decision tree and it works well with default parameters.

There are parameters which allow you to change the performance of the Random Forest much as we changed the maximum depth of the single decision tree. But one of the best features of Random Forest models is that they generally work reasonably even without this tuning.

<details>
  <summary>simple model training</summary>

  ```python
  from sklearn.ensemble import RandomForestRegressor
  from sklearn.metrics import mean_absolute_error

  forest_model = RandomForestRegressor(random_state=1)
  forest_model.fit(train_X, train_y)
  melb_preds = forest_model.predict(val_X)
  print(mean_absolute_error(val_y, melb_preds))
  ```
  </details>

# 2. Unsupervised Learning

# 3. Problems in Machine Learning
## Missing values

1) A Simple Option: Drop Columns with Missing Values
2) A Better Option: Imputation, fill missing values with number
3) An Extension To Imputation, fill and add another column

**Imputation removes column names while using sklearn**
```python
from sklearn.impute import SimpleImputer
# Imputation removed column names; put them back
imputed_X_train.columns = X_train.columns
imputed_X_valid.columns = X_valid.columns
```

## Categorical Values
1. drop categorical values
```python
drop_X_train = X_train.select_dtypes(exclude=['object'])
```
2. ordinal encoding (0-to-5 or something)
3. one-hot encoding (up to 15 values)


# 4. Libraries
## Pandas
- to choose rows   
  - axis = 'index'
  - axis = 0
- cols
  - axis = 'columns'
  - axis = 1

**Importing and reading data**
```python
import pandas as pd
filepath = '<something>'
data_frame = pd.read_csv(filepath)
```
**Describing and seeing the data**
```python
pd.set_option('display.max_rows', 0)                 # show all rows
pd.set_option('display.max_columns', None)           # show all columns
home_data.describe()                                 # statistical summmary of data
home_data["<SomeColumn>","AnotherColumn"].describe() # summary of col 
```

**Print labels with nan values**
* print sum of nan values in corresponding column
  ```python
  print(type(test_data.isna().sum()))
  ```
* print nan valued labels' columns
  ```python
  cols_with_missing = [col for col in X_train.columns if X_train[col].isnull().any()]
  ```
* number of missing values in each column
  ```python
  missing_val_count_by_column = (X_train.isnull().sum())
  print(missing_val_count_by_column[missing_val_count_by_column > 0]
          #içerisi indisleri döndürüyor. gelen indislerdeki değerlerin hepsini yazdırıyoruz.s
  ```
# General Terms

| Term  	| Explanation        	 |
|------------	|----------------- |
| Training Data   | used to train the ML model (%80 of our dataset) |
| Validation Data | used to measure candidate model's accuracy (%10)|
| Test Data | used to test the model in unseen data (%10) |
| Overfitting | capturing spurious patterns that won't recur in the future, leading to less accurate predictions |
|Underfitting | failing to capture relevant patterns, leading to less accurate predictions|
| Cardinality | number of unique entries of a categorical variable |
| Cross Validation | to train a model on different subsets of the data to get multiple measures of model quality. <br> -  For small datasets, where extra computational burden isn't a big deal <br> - For larger datasets, a single validation set is sufficient. <br>    - if your model takes a couple minutes or less to run, it's probably worth switching to cross-validation.|
|  | |
|  | |
|  | |
|  | |
|  | |
|  | |
|  | |
|  | |
