---
layout: post
title: pandas cheat sheet
image: 
tags: [python, pandas]
---

pandas is a powerful and flexible open source tool for data analysis and manipulain in python.

## Reading a CSV file

```python
#import pandas module
import pandas as pd

#read csv file using read_csv() method
df = pd.read_csv('path/to/csvfile.csv');

```

Birds eye view of data

```python

df #displays the dataframe

df.info() #prints information about the data

df.columns #returns an array of column names

df.head() #returns first 5 enteries of data
df.tail() #returns last 5 enteries of data

df.head(n) #returns first n enteries of the data
df.tail(n) #returns last n enteries of the data

df.sample() #returns one random entry from the data
df.sample(n) # returns n random samples from the data

df.describe() #provides the statistical information of the data

df.corr() # provides correlation between the fields

df.cov() #provides covariance between the fields

df.count() #returns count of non-NA/null observations for each column

```

