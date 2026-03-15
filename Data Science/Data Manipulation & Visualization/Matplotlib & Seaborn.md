```Plain
import seaborn as sns
import matplotlib.pyplot as plt
```
# Matplotlib
```Python
# theme
plt.style.use('fivethirtyeight')
plt.rcParams['figure.figsize'] = (8, 6)
plt.rcParams['font.size'] = 14
# load data
housing_csv = 'data/boston_housing_data.csv'
housing = pd.read_csv(housing_csv)
housing.ZN.plot(kind="bar")
# display/save
plt.show()
plt.savefig('beer_histogram.png')
```
Plot Details
```Python
df # DataFrame
# .plot common parameters: figsize, color, title, style
df.plot(kind='line') # default
df.plot(kind='bar/barh', stacked=True)
df.plot(kind='hist', bins=20)
df.plot(kind='density', xlim=(0, 500))
df.plot(kind='box') # or .boxplot(column='beer', by='continent')
df.plot(kind='scatter', x='beer', y='wine')
plt.plot(xArray, yArray)
plt.imshow(data, cmap='viridis')
ax = df.plot()
ax.set_title('title')
ax.legend(loc=1)
plt.xlabel('y-axis info')
plt.ylabel('x-axis info')
ax.set_ylabel('y-axis info', fontsize=16)
ax.set_xlabel('x-axis info', fontsize=16)
```
# Seaborn
  
```Python
# pairplot
sns.pairplot(housing)
# heatmap
housing_correlations = housing.corr()
sns.heatmap(housing_correlations)
```