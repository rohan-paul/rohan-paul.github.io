# Categorical-Variables-Handling-Blog

What is Categorical Data

![](https://cdn-images-1.medium.com/max/1200/1*i5WNbb4IOPQc9suY7lweuA.jpeg)

[**Link to Kaggle Notebook with UCI-Breast Cancer Data**](https://www.kaggle.com/paulrohan2020/categorical-variables-encoding-uci-breast-cancer)

#### What is Categorical Data

Categorical variables are those values in a dataset that are selected from a group of categories or labels. Typically, any data attribute which is categorical in nature represents discrete values that belong to a specific finite set of categories or classes. These are also often known as classes or labels in the context of attributes or variables which are to be predicted by a model (popularly known as response variables). These discrete values can be text or numeric in nature (or even unstructured data like images!).

#### There are two major classes of categorical data, nominal and ordinal.

In any nominal categorical data attribute, there is no concept of ordering amongst the values of that attribute. Consider a simple example of weather categories like — sunny, cloudy, rainy, etc. These are without any concept or notion of order (windy doesn’t always occur before sunny nor is it smaller or bigger than sunny).

For example, the variable may be “color” and may take on the values “red,” “green,” and “green.” Or the variable Gender with the values of male or female is categorical, and so is the variable marital status with the values of never married, married, divorced, or widowed.

Another example, in a survey about the preferred brand of car they owned, the result would be categorical (e.g. Tesla, Toyota, Ford, None, etc.). Responses fall into a fixed set of categories.

**Ordinal categorical** attributes have some sense or notion of order amongst its values. For instance, say shirt sizes. It is quite evident that order or in this case ‘size’ matters when thinking about shirts (S is smaller than M which is smaller than L and so on).

I will get an error if you try to plug these variables into most machine learning models without “encoding” them first.

Almost all Machine learning and deep learning neural networks algorithms require that input and output variables are numbers, requiring that categorical data must be encoded to numbers before we can use it to feed to models and evaluate a model.

There are quite a few techniques to encode categorical variables for modeling, although the three most common are as follows:

*   Integer Encoding: Where each unique label is mapped to an integer.
*   One Hot Encoding: Where each label is mapped to a binary vector.
*   Learned Embedding: Where a distributed representation of the categories is learned.

In some categorical variables, the labels have an intrinsic order, for example, in the variable Student’s grade, the values of A, B, C, or Fail are ordered, A being the highest grade and Fail the lowest. These are called ordinal categorical variables. Variables in which the categories do not have an intrinsic order are called nominal categorical variables, such as the variable City, with the values of London, Manchester, Bristol, and so on.

The values of categorical variables are often encoded as strings. Scikit-learn, does not support strings as values, therefore, we need to transform those strings into numbers. The act of replacing strings with numbers is called categorical encoding.

#### One-hot Encoding

One-hot encoding is where you represent each possible value for a category as a separate feature.

In one-hot encoding, we represent a categorical variable as a group of binary variables, where each binary variable represents one category. The binary variable indicates whether the category is present in an observation (1) or not (0).

One hot encoding is the most widespread approach, and it works very well unless our categorical variable takes on a large number of values (e.g. more than 20 different values)

undefined
undefined

Another example with a variable named ‘color’. The values in the variable are Red, Yellow, and Green. And then we create a separate column for each possible value. Wherever the original value was Red, we put a 1 in the Red column.

undefined
undefined

From the above Gender variable, we can derive the binary variable of Female, which shows the value of 1 for females, or the binary variable of Male, which takes the value of 1 for the males in the dataset. For the categorical variable of Color with the values of red, green, and green, we can create three variables called red, green, and green. These variables will take the value of 1 if the observation is red, green, or green, respectively, or 0 otherwise.

A categorical variable with k unique categories can be encoded in k-1 binary variables. For Gender, k is 2 as it contains two labels (male and female), therefore, we need to create only one binary variable (k — 1 = 1) to capture all of the information. For the color variable, which has three categories (k=3; red, green, and green), we need to create two (k — 1 = 2) binary variables to capture all the information, so that the following occurs:

*   If the observation is red, it will be captured by the variable red (red = 1, green = 0).
*   f the observation is green, it will be captured by the variable green (red = 0, green = 1).
*   If the observation is green, it will be captured by the combination of red and green (red = 0, green = 0).

There are a few occasions in which we may prefer to encode the categorical variables with k binary variables:

*   When training decision trees, as they do not evaluate the entire feature space at the same time
*   When selecting features recursively
*   When determining the importance of each category within a variable

```
# !pip install feature_engine# The above line for installing feature_engine in Kaggleimport numpy as npimport pandas as pdimport randomimport matplotlib.pyplot as pltfrom sklearn.model_selection import train_test_splitfrom sklearn.preprocessing import OneHotEncoderfrom sklearn.preprocessing import LabelEncoderfrom feature_engine.categorical_encoders import OneHotCategoricalEncoderfrom IPython.display import display # Allows the use of display() for DataFramesimport warningswarnings.filterwarnings('ignore')
```

```
breast_cancer_df = pd.read_csv('../input/breast-cancer-data/breast-cancer.data')print('Breast Cancer df number of rows and columns are ', breast_cancer_df.shape)
```

```
Breast Cancer df number of rows and columns are  (285, 10)
```

```
# Replace the question marks in the dataset with NumPy NaN values:breast_cancer_df = breast_cancer_df.replace('?', np.nan)
```

```
# Create a list with the variable names:# There are 10 columns as we know the from the shape of the dataframe# So create list of 10 column-headings starting with 'A1' and ending with 'A-10'# Meaning I have to traverser a range of 1 to 11column_labels = ['A' + str(s) for s in range(1, 11)]column_labels
```

```
['A1', 'A2', 'A3', 'A4', 'A5', 'A6', 'A7', 'A8', 'A9', 'A10']
```

```
# Now assign the above list of as column-labelbreast_cancer_df.columns = column_labels
```

```
# Make lists with categorical and numerical variables:category_columns = [c for c in breast_cancer_df.columns if breast_cancer_df[c].dtypes == 'O' ]numeric_columns = [c for c in breast_cancer_df.columns if breast_cancer_df[c].dtypes != 'O' ]print('breast_cancer_category_columns ', category_columns)print('breast_cancer_numeric_columns ', numeric_columns)
```

```
breast_cancer_category_columns  ['A1', 'A2', 'A3', 'A4', 'A5', 'A6', 'A8', 'A9', 'A10']breast_cancer_numeric_columns  ['A7']
```

From the above we see that column ‘A7’ is the Numeric Column and the, rest all are categorical column. Now, re-cast numerical variables to float types:

```
breast_cancer_df['A7'] = breast_cancer_df['A7'].astype(float)
```

#### Binary encoding — Re-code the target variable as binary:

Binary encodings are a special case of category features. Here’s a way to do this, do it to the column label of ‘A10’. That is making each ‘yes’ as 1 and each ‘no’ as 0 (zero)

```
breast_cancer_df['A10'] = breast_cancer_df['A10'].map({'yes':1, 'no':0})breast_cancer_df.head()
```

undefined
undefined

```
# Fill in the missing databreast_cancer_df[numeric_columns] =  breast_cancer_df[numeric_columns].fillna(0)breast_cancer_df[category_columns] = breast_cancer_df[category_columns].fillna(0)
```

```
# separate the data into train and test sets:X_train, X_test, y_train, y_test = train_test_split(breast_cancer_df.drop(labels=['A10'], axis=1), breast_cancer_df['A10'], test_size=0.3, random_state=0)
```

```
#  Let's inspect the unique categories of the A3 variableX_train['A3'].unique()
```

```
array(['ge40', 'premeno', 'lt40'], dtype=object)
```

So I have the unique values as

array(\[‘ge40’, ‘premeno’, ‘lt40’\], dtype=object)

#### one-hot encoding using pandas get\_dummies()

Let’s encode A3 into k-1 binary variables using pandas and then inspect the first five rows of the resulting dataframe:

```
tmp_1 = pd.get_dummies(X_train['A3'], drop_first=True)tmp_1.head()
```

undefined
undefined

```
tmp_2 = pd.get_dummies(X_train['A3'], drop_first=False)tmp_2.head()
```

undefined
undefined

`get_dummies` pandas function converts categorical variables into indicator variables and ignores missing data, unless we specifically indicate otherwise, in which case, it will return missing data as an additional category

To encode the variable into k binaries, use instead `drop_first=False`.

From the output above we can see each label is now a binary variable and there’s two (because we used k — 1 ) new columns for the label-names.

To understand how the get\_dummies() implementation take a look at the below code

```
df = pd.DataFrame({'country': ['russia', 'germany', 'australia','korea']})df_get_dummied = pd.get_dummies(df['country'], prefix='country')df_get_dummied
```

undefined
undefinedundefined
undefined

To encode all categorical variables at the same time, let’s first make a list with their names: i.e.

*   I am excluding A7 (which is numerical data) and
*   A10 (which is the target variable and I have make it to be binary previously.
*   Also excluding all the age ranges i.e ‘A2’, ‘A4’, ‘A5’, ‘A8’

```
vars_categorical = ['A1', 'A3', 'A6', 'A8', 'A9' ]# Now, let's encode all of the categorical variables into k-1 binaries each, capturing the result in a new dataframe:X_train_dummy_encoded_pandas = pd.get_dummies(X_train[vars_categorical], drop_first=True)X_test_dummy_encoded = pd.get_dummies(X_test[vars_categorical], drop_first=True )X_train_dummy_encoded_pandas.head()
```

undefined
undefined

So as we can see above, the pandas’ `get_dummies()` function will create one binary variable per found category. Hence, if there are more categories in the train set than in the test set, get\_dummies() will return more columns in the transformed train set than in the transformed test set.

### A note on `fit()/fit_transform()/transform()` from scikit-learn

Both `fit_transform()` and `transform()` are the methods of class [**sklearn.preprocessing.StandardScaler()**](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html#sklearn-preprocessing-standardscaler) and used almost together while scaling or standardizing our training and test data.

#### The standard score of a sample x is calculated as:

z = (x — u) / s

where u is the mean of the training samples or zero if with\_mean=False, and s is the standard deviation of the training samples or one if with\_std=False.

*   Data standardization is the process of rescaling the attributes so that they have mean as 0 and variance as 1.
*   Ultimate goal to perform standardization is to bring down all the features to a common scale without distorting the differences in the range of the values.
*   In sklearn.preprocessing.StandardScaler(), Centering and scaling happen independently on each feature by computing the relevant statistics on the samples in the training set. Mean and standard deviation are then stored to be used on later data using transform.
*   fit\_transform(X\[, y\]) => Fit to data, then transform it. Fits transformer to X and y with optional parameters fit\_params and returns a transformed version of X. fit\_transform() is used on the training data so that we can scale the training data and also learn the scaling parameters of that data. Here, the model built by us will learn the mean and variance of the features of the training set. These learned parameters are then used to scale our test data. The fit method is calculating the mean and variance of each of the features present in our data. The transform method is transforming all the features using the respective mean and variance.
*   transform(X\[, copy\]) => Perform standardization by centering and scaling. We call `fit_transform()` method on our training data and `transform()` method on our test data. Using the `transform()` method we can use the same mean and variance as it is calculated from our training data to transform our test data. Thus, the parameters learned by our model using the training data will help us to transform our test data.

So in further simple words — every sklearn’s transform’s `fit()` just calculates the parameters (e.g. Mean and Sigma in case of StandardScaler) and saves them as an internal objects state. Afterward, we can call its `transform()` method to apply the transformation to a particular set of examples.

`fit_transform()` joins these two steps and is used for the initial fitting of parameters on the training set x, but it also returns a transformed x. Internally, it just calls first `fit()` and then `transform()` on the same data.

#### Now one-hot encoding using scikit-learn

First, Create a label (category) encoder object with LabelEncoder() which is a utility class to help normalize labels such that they contain only values between 0 and n\_classes-1.

#### Why we need Label Encoding

Datasets in Machine Learning usually contain multiple labels in one or more than one column. These labels can be in the form of words, to make the data understandable i.e. to keep it in human-readable form.

Label Encoding refers to converting these labels into numeric form so as to convert it into the machine-readable form. Machine learning algorithms can then decide in a better way on how those labels must be operated. It is an important pre-processing step for the structured dataset in supervised learning.

```
example_df = pd.DataFrame(['India', 'Australia', 'USA'], columns= ['Country'])example_df
```

undefined
undefined

#### How do we do Numeric encoding from the above DataFrame for the Country feature?

Ans is with Scikit learn transformation, called LabelEncoder:

```
example_df['Country_encoded'] = LabelEncoder().fit_transform(example_df['Country'])example_df
```

undefined
undefined

Let’s take a closer look at what the LabelEncoder is doing

```
encoder = LabelEncoder()encoder.fit(example_df['Country'])encoder.classes_
```

Given the output — array(\[‘Australia’, ‘India’, ‘USA’\], dtype=object)

We see that the ordering of the list of classes above corresponds to their numeric values. Transformation is then as follows:

#### Now apply LabelEncoder() to our Breast-Cancer dataset

```
enc = LabelEncoder()enc.fit(vars_categorical)# View the labels (if you want)print("label (category) encoder List: ", list(enc.classes_))# ['A1', 'A3', 'A6', 'A8', 'A9']new_cat_features = enc.transform(vars_categorical)print(new_cat_features) # [0 1 2 3 4]new_cat_features = new_cat_features.reshape(-1, 1)
```

```
label (category) encoder List:  ['A1', 'A3', 'A6', 'A8', 'A9'][0 1 2 3 4]
```

Then create an OneHotEncoder transformer that encodes into k-1 binary variables and returns a NumPy array:

Scikit-learn’s `OneHotEncoder()` function will only encode the categories learned from the train set. If there are new categories in the test set, we can instruct the encoder to ignore them or to return an error with the `handle_unknown='ignore'` argument or the `handle_unknown='error'` argument, respectively.

setting the `categories='auto'` argument so that the transformer learns the categories to encode from the train set; `drop='first'` so that the transformer drops the first binary variable, returning k-1 binary features per categorical variable; and sparse=False so that the transformer returns a NumPy array (the default is to return a sparse matrix).

Now, let’s create the NumPy arrays with the binary variables for train and test sets:

```
ohe_scikit = OneHotEncoder(sparse=False, categories='auto', drop='first')
```

#### Now fit i.e. make scikit\_learn to learn the encoder to a slice of the train set with the categorical variables so it identifies the categories to encode:

```
output = ohe_scikit.fit_transform(new_cat_features)print(output)
```

```
[[1. 0. 0. 0. 0.] [0. 1. 0. 0. 0.] [0. 0. 1. 0. 0.] [0. 0. 0. 1. 0.] [0. 0. 0. 0. 1.]]
```

Unfortunately, the feature names are not preserved in the NumPy array, therefore, identifying which feature was derived from which variable is not straightforward.

The beauty of pandas’ `get_dummies()` function is that it returns feature names that clearly indicate which variable and which category each feature represents. On the downside, `get_dummies()` does not persist the information learned from the train set to the test set.

Contrarily, scikit-learn’s `OneHotEncoder()` function can persist the information from the train set, but it returns a NumPy array, where the information about the meaning of the features is lost.

Scikit-learn’s `OneHotEncoder()` function will create binary indicators from all variables in the dataset, so be mindful not to pass numerical variables when fitting or transforming your datasets.

#### Implement one-hot encoding with Feature-engine

`Feature-engine` has multiple advantages:

*   first, it allows us to select the variables to encode directly in the transformer.
*   Second, it returns a pandas dataframe with clear variable names, and
*   third, it preserves the information learned from the train set, therefore returning the same number of columns in both train and test sets.

#### With that, Feature-engine overcomes the limitations of pandas’ `get_dummies()` method and scikit-learn's `OneHotEncoder()` class.

From its [documentation](https://feature-engine.readthedocs.io/en/latest/encoders/OneHotCategoricalEncoder.html)

The OneHotCategoricalEncoder() replaces categorical variables by a set of binary variables, one per unique category. The encoder has the option to create k or k-1 binary variables, where k is the number of unique categories.

The encoder can also create binary variables for the n most popular categories, n being determined by the user. This means, if we encode the 6 more popular categories, we will only create binary variables for those categories, and the rest will be dropped.

The OneHotCategoricalEncoder() works only with categorical variables. A list of variables can be indicated, or the encoder will automatically select all categorical variables in the train set.

```
one_hot_enc_feature_engine = OneHotCategoricalEncoder(top_categories=None, drop_last=True)
```

#### Now, once we have the encoder object, we can encode our data using the fit()/transform() or the fit\_transform() methods

With top\_categories=None, we indicate that we want to encode all of the categories present in the categorical variables.

This breast-cancer data set above only has a few categories, but what if it had 500 ? That’s when, top\_categories become very useful, which has the effect of collapsing into a more manageable number. For example, you could set the top\_categories to 10 and that would get you the 10 most frequently occurring category columns and all others would be collapsed into an “other” column.

Feature-engine detects the categorical variables automatically. To encode only a subset of the categorical variables, we can pass the variable names in a list like below:

`one_hot_enc_feature_engine = OneHotCategoricalEncoder(variables=['A1', 'A4'])`

Since the OneHotCategoricalEncoder uses the fit()/fit\_transform()/transform() from scikit-learn, it can be used in a Pipeline object. Finally, and perhaps most important to me, is that the OneHotCategoricalEncoder returns a pandas DataFrame rather than numpy arrays or other sparse matrices. The reason this mattered to me was that I wanted to see which categorical columns actually are adding value to the model and which are not. Doing this from a numpy array without column references is exceedingly difficult.

Now, let’s fit the encoder to the train set so that it learns the categories and variables to encode:

```
one_hot_enc_feature_engine.fit(X_train)# Let's encode the categorical variables in train and test sets, and display the first five rows of the encoded train set:X_train_enc_feature_engine = one_hot_enc_feature_engine.transform(X_train)X_test_enc_feature_engine = one_hot_enc_feature_engine.transform(X_test)X_train.head()
```

#### one-hot encoding of frequent categories

#### What is high cardinality of a dataset

A dataset which has columns(feature) with high number of unique values. Another way to refer to variables that have a multitude of categories, is to call them variables with high cardinality. If we have categorical variables containing many multiple labels or high cardinality, then by using one-hot-encoding, we will expand the feature space dramatically, which is not an ideal situation to be in.

One approach used by many in Kaggle competitions is to replace each label of the categorical variable by the count, this is the number of times each label appears in the dataset. Or the frequency, this is the percentage of observations within that category.

But the cost of the above strategy is some loss of information because I am effectively turning a categorical feature into a “popularity” feature.

Check the “Count Encoding” section on [this link](https://www.kaggle.com/matleonard/categorical-encodings)

While dealing with a highly cardinal dataset, one thing to ensure that the cardinality of the categorical information in the training set resembles that in the test/validation sets. That is, if I have a feature with values {A,A,A,B,C,C,D} in train, but test only has {A, B, B}, then eliminating the C and D records, and undersampling the A or oversampling the B records may resist overfitting. Also, for individual features with low cardinality, it’s often worth bucketing them. In the above example, you may end up replacement values for A and C, and then bucketing B and D into an “Other” category

#### We will deal with High-Cardinality with **OneHotCategoricalEncoder** from `feature_engine`

One-hot encoding represents each category of a categorical variable with a binary variable. Hence, one-hot-encoding of highly cardinal variables or datasets with multiple categorical features can expand the feature space dramatically. To reduce the number of binary variables, we can perform one-hot encoding of the most frequent categories only. One-hot encoding of top categories is equivalent to treating the remaining, less frequent categories as a single, unique category

```
# Lets' look at the train data againX_train.head()
```

```
#  Let's inspect the unique categories of the A4 variable:X_train['A4'].unique()
```

#### Let’s count the number of observations per category of A4, sort them in decreasing order, and then display the five most frequent categories:

```
X_train['A4'].value_counts().sort_values(ascending=False).head(5)
```

we counted the number of observations in the train set, per category of A4 using pandas’ `value_counts()` method and sorted the categories from the one with most observations to the one with the least using pandas' `sort_values()` method.

#### Now let’s capture the most frequent categories of A4 in a list using the code in the above step inside a list comprehension:

```
top_5_A4 = [cat for cat in X_train['A4'].value_counts().sort_values(ascending=False).head(5).index]top_5_A4
```

#### Now, let’s add a binary variable per top category in the train and test sets

```
for category in top_5_A4:    X_train['A4' + '_' + category] = np.where(X_train['A4'] == category, 1, 0 )
```

So in above, I looped over each top category, and with NumPy’s `where()` method, created binary variables by placing a 1 if the observation showed the category, or 0 otherwise.

```
#  Let's print the top 10 rows of the original and encoded variable, A4, in the train set:print(X_train[['A4'] + ['A4' + '_' + c for c in top_5_A4]].head(10))
```

So in above, I named the new variables using the original variable name, A4, plus an underscore followed by the category name.

#### Now, We can simplify the above procedures, that is, the one-hot encoding of frequent categories, with Feature-engine. First, let’s load and divide the dataset.

```
one_hot_enc_feature_engine_top_5 = OneHotCategoricalEncoder(top_categories=5, variables=['A4'], drop_last=False)# Let's fit the encoder to the train set so that it learns and stores the most frequent categories of A4one_hot_enc_feature_engine_top_5.fit(X_train)X_train_enc_feature_engine_top_5 = one_hot_enc_feature_engine_top_5.transform(X_train)# Now finally new binary variables in the dataframe executing belowX_train_enc_feature_engine_top_5.head()
```

Feature-engine replaces the original variable with the binary ones returned by one-hot encoding, leaving the dataset ready to use in machine learning.

I could also perform one-hot encoding of the five most popular categories using scikit-learn’s `OneHotEncoder()` function. To do this, I need to pass a list of lists or an array of values to the categories argument where each list or each row of the array holds the top five categories expected in the relevant variable.

### Replacing categories with ordinal numbers

Ordinal encoding consists of replacing the categories with digits from 1 to k (or 0 to k-1, depending on the implementation), where k is the number of distinct categories of the variable. The numbers are assigned arbitrarily. Ordinal encoding is better suited for non-linear machine learning models, which can navigate through the arbitrarily assigned digits to try and find patterns that relate to the target.

Now we will perform ordinal encoding using pandas, scikit-learn, and Feature-engine.

_\# Let's load a clean version of the data again and split it for train and test_  
X\_train, X\_test, y\_train, y\_test = train\_test\_split(breast\_cancer\_df.drop(labels=\['A10'\], axis=1), breast\_cancer\_df\['A10'\], test\_size=0.3, random\_state=0)

_\# Lets' look at the train data again_  
X\_train.head()

![](https://cdn-images-1.medium.com/max/800/1*_L3DSf4Cv8XT6L2XPFKbEg.png)
undefined

_\# Let's encode the A4 variable for this demonstration. First, let's make a dictionary of category to integer pairs and then display the result:_  
  
ordinal\_mapping = {k: i for i, k **in** enumerate(  
    X\_train\['A4'\].unique(), 0)}  
ordinal\_mapping

![](https://cdn-images-1.medium.com/max/800/1*qZ_-jsO7mywng_LMyY4BDw.png)
undefined

Above we can see the digits that will replace each unique category in the following

The enumerate() function takes a collection (e.g. a tuple) and returns it as an enumerate object. The enumerate() function adds a counter to an iterable as the key of the enumerate object. This function is used when dealing with iterators, we also get a need to keep a count of iterations.

l1 = \["eat","sleep","repeat"\]  
print(list(enumerate(l1)))  
_\# \[(0, 'eat'), (1, 'sleep'), (2, 'repeat')\]_

#### Now, let’s replace the categories with numbers in the original variables:

X\_train\['A4'\] = X\_train\['A4'\].map(ordinal\_mapping)  
X\_test\['A4'\] = X\_test\['A4'\].map(ordinal\_mapping)

_\# Print X\_train to see its structure now_  
X\_train\['A4'\].head(10)

![](https://cdn-images-1.medium.com/max/800/1*yHkFB3vn1gPUWvDB0lTW8w.png)
undefined

_\# Now note we have already defined earlier a list with the categorical variables to encode:_  
_\# Lets re-define it again with a slight modification by removing some of the Columns that has string value._  
vars\_categorical = \['A2', 'A4', 'A5' \]  
  
_\# Let's start the ordinal encoder:_  
le = OrdinalEncoder()  
  
_\# Let's fit the encoder to the slice of the train set with the categorical variables so that it creates and stores representations of categories to digits:_  
le.fit(X\_train\[vars\_categorical\])  
  
_\# Scikit-learn's OrdinalEncoder() function will encode the entire dataset. To encode only a selection of variables, we need to slice the dataframe . Alternatively, we can use scikit-learn's ColumnTransformer()._  
  
_\# Now let's encode the categorical variables in the train and test sets:_  
X\_train\_encoded = le.fit\_transform(X\_train\[vars\_categorical\])  
X\_test\_encoded = le.transform(X\_test\[vars\_categorical\])  
  
X\_train\_encoded

It will output like below

![](https://cdn-images-1.medium.com/max/800/1*E6MbZPayQFQzQ_TaJgXWgA.png)
undefined

Remember scikit-learn returns a NumPy array and NOT a DataFrame, so with the above I can not use the `head()` function.

### Now let’s do ordinal encoding with Feature-engine.

First, let’s load and divide the dataset as earlier.

Let’s create an ordinal encoder that replaces categories with numbers arbitrarily and encodes the **categorical variables** we defined earlier.

In \[35\]:

ordinal\_encoder\_feature\_engine = OrdinalCategoricalEncoder(encoding\_method='arbitrary', variables=vars\_categorical  
                                                        )

#### A Very Important Note — categorical encoders work only with object type variables. So to treat numerical variables as categorical, we need to re-cast them as Object

_\# make a list of discrete variables_

discrete = \[ var for var **in** numerical if len(data\[var\].unique()) < 20\]

data\[discrete\]= data\[discrete\].astype('O')

Source — [https://www.trainindatablog.com/feature-engine-a-new-open-source-python-package-for-feature-engineering/](https://www.trainindatablog.com/feature-engine-a-new-open-source-python-package-for-feature-engineering/)

X\_test = X\_train\[vars\_categorical\].astype('O')  
  
_\# Let's fit the encoder to the train set so that it learns and stores the category-to-digit mappings:_  
ordinal\_encoder\_feature\_engine.fit(X\_train)

![](https://cdn-images-1.medium.com/max/800/1*28iUBMILAktKVR2gvZhdYg.png)
undefined

The category to digit mappings are stored in the `encoder_dict_attribute` and can be accessed by executing `ordinal_encoder_feature_engine.encoder_dict_`

**Finally, let’s encode the categorical variables in the train and test sets:**

X\_train = ordinal\_encoder\_feature\_engine.transform(X\_train)  
X\_test = ordinal\_encoder\_feature\_engine.transform(X\_test)  
  
X\_train.head()

X\_train = ordinal\_encoder\_feature\_engine.transform(X\_train)  
X\_test = ordinal\_encoder\_feature\_engine.transform(X\_test)  
  
X\_train.head()

With the `fit()` method, the transformer assigned an integer to each category of each variable in the train set. With the `transform()` method, the categories were replaced with integers, returning a NumPy array.

Feature-engine returns pandas dataframes where the values of the original variables are replaced with numbers, leaving the dataframe ready to use in machine learning models.

By [Rohan Paul](https://medium.com/@paulrohan) on [November 17, 2020](https://medium.com/p/3e26455d4d00).

[Canonical link](https://medium.com/@paulrohan/categorical-variables-handling-blog-3e26455d4d00)

Exported from [Medium](https://medium.com) on December 12, 2020.