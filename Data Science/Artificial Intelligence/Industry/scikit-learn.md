# Workflow
```shell
pip install scikit-learn imblearn
```
## Data Preparation
```Python
from sklearn.preprocessing import OneHotEncoder, StandardScaler
from sklearn.model_selection import train_test_split

data = pd.read_csv('./data.csv', header=0, dtype=str)
```
### Remove None Data
```python
for column in data.columns:
	data[column] = pd.to_numeric(data[column], errors='coerce')
```
1. Remove the row/column
	row: `df.dropna()`
	column: `df.dropna(axis=1)`
2. Imputation: replace None with mean/medium/mode/constant(like -1) etc.
	`df['column_name'].fillna(df['column_name'].mean(), inplace=True)`
### Encoding
> Categorical Data --> Numerical Data
#### Label Encoding / Integer Encoding
```python
example_col = ['apple', 'banana', 'apple', 'orange', 'banana', 'orange', 'apple']
codes, uniques = pd.factorize(example_col)

# codes: [0 1 0 2 1 2 0]
# uniques: ['apple' 'banana' 'orange']
```
#### One-hot Encoding
```Python
#Encoding binary variables
binary_cols = ['is_manager', 'certifications']
for c in binary_cols:
    data[c] = data[c].replace(to_replace=['Yes'], value=1)
    data[c] = data[c].replace(to_replace=['No'], value=0)
data = pd.get_dummies(data, columns=cat_cols, drop_first=True)
data.shape
```
### Differentiate features & labels
```python
y = final_data['salary']
X = final_data.drop(columns=['salary'])
```
### Under-sampling the majority class
```python
from imblearn.under_sampling import RandomUnderSampler
rus = RandomUnderSampler(sampling_strategy=1, random_state=42)
X, y = rus.fit_resample(X, y)
```
### Train Test Split
```Python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
print("Training Set Dimensions:", X_train.shape)
print("Validation Set Dimensions:", X_test.shape)
```
### Normalizing Data
```Python
num_cols = ['job_years','hours_per_week','telecommute_days_per_week']
scaler = StandardScaler()
scaler.fit(X_train[num_cols])
X_train[num_cols] = scaler.transform(X_train[num_cols])
```
## Training & Evaluation
```Python
from sklearn.linear_model import LinearRegression, Ridge, Lasso
from sklearn.metrics import mean_absolute_error, mean_squared_error
from sklearn.tree import DecisionTreeRegressor
from sklearn.ensemble import RandomForestRegressor
```
### Training
```Python
reg=LinearRegression()
reg.fit(X_train, y_train)
reg.coef_
reg.intercept_
```
### Prediction & Evaluation
```Python
mean_absolute_error(y_test,reg.predict(X_test))
mean_squared_error(y_test,reg.predict(X_test))**0.5
reg.score(X_test, y_test)
```