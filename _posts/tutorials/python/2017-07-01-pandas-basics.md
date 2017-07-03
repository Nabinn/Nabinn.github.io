---
layout: post
title: Pandas cheat sheet
image: 
tags: [python, pandas]
---

pandas is a powerful and flexible open source data analysis / manipulation tool for python programming language.

## Reading a CSV file

```python
#import pandas module
import pandas as pd

#read csv file using read_csv() method
df = pd.read_csv('path/to/csvfile.csv');


df #displays the dataframe

df.info() #prints information about the data

df.head() #returns first 5 enteries of data
df.tail() #returns last 5 enteries of data

df.head(n) #returns first n enteries of the data
df.tail(n) #returns last n enteries of the data

#train_df.describe() #describe the statistical information of the data
#train_df.corr() #calculate correlation between the fields
#train_df.cov() #covariance
#train_df.sample(100) # gives a random number of samples of data
#train_df.count() #returns count of non-NA/null observations
#train_df.columns #returns array of column names


```

