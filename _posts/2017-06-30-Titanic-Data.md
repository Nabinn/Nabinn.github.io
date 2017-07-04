---
layout: post
title: Estimating the Survival chance of Titanic passengers
image: /img/hello_world.jpeg
tags: [machine learing, classification]
---

## Overview of data
On April 15, 1912, the Titanic sank after colliding with an iceberg, killing 1502 out of 2224 passengers and crew. Although there was some element of luck involved in surviving the sinking, some groups of people were more likely to survive than others, such as women, children, and the upper-class. In this tutorial, we carry an analysis to find out who these people are.

The dataset can be downloaded from kaggle:

<a href="https://www.kaggle.com/c/titanic/data">click here to download data</a>

The data has been split into two groups:
training set (train.csv)
test set (test.csv)
The training set should be used to build your machine learning models. For the training set, we provide the outcome (also known as the “ground truth”) for each passenger. Your model will be based on “features” like passengers’ gender and class. You can also use feature engineering to create new features.

The test set should be used to see how well your model performs on unseen data. For the test set, we do not provide the ground truth for each passenger. It is your job to predict these outcomes. For each passenger in the test set, use the model you trained to predict whether or not they survived the sinking of the Titanic.



## Dataset
Let's take a look at the datase. For each passenger, the following information are provided:
```
VARIABLE        DESCRIPTIONS:
PassengerId     Id of passenger
Survived        Survived
                (0 = No; 1 = Yes)
Pclass          Passenger Class
                (1 = 1st; 2 = 2nd; 3 = 3rd)
Name            Name
Sex             Sex
Age             Age
SibSp           Number of Siblings/Spouses Aboard
Parch           Number of Parents/Children Aboard
Ticket          Ticket Number
Fare            Passenger Fare
Cabin           Cabin number
Embarked        Port of Embarkation
                ( C = Cherbourg, Q = Queenstown, S = Southampton )
```

There are 2 classes in our task 'not survived' (class 0) and 'survived' (class 1), and the passengers data have 8 features.



```python
import pandas as pd
import numpy as np
```


```python
df_training=pd.read_csv('data/train.csv')
display(df_training.head())

```


<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>



```python
print(df_training.info())
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 891 entries, 0 to 890
    Data columns (total 12 columns):
    PassengerId    891 non-null int64
    Survived       891 non-null int64
    Pclass         891 non-null int64
    Name           891 non-null object
    Sex            891 non-null object
    Age            714 non-null float64
    SibSp          891 non-null int64
    Parch          891 non-null int64
    Ticket         891 non-null object
    Fare           891 non-null float64
    Cabin          204 non-null object
    Embarked       889 non-null object
    dtypes: float64(2), int64(5), object(5)
    memory usage: 83.6+ KB
    None



```python
df_training.describe()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>891.000000</td>
      <td>891.000000</td>
      <td>891.000000</td>
      <td>714.000000</td>
      <td>891.000000</td>
      <td>891.000000</td>
      <td>891.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>446.000000</td>
      <td>0.383838</td>
      <td>2.308642</td>
      <td>29.699118</td>
      <td>0.523008</td>
      <td>0.381594</td>
      <td>32.204208</td>
    </tr>
    <tr>
      <th>std</th>
      <td>257.353842</td>
      <td>0.486592</td>
      <td>0.836071</td>
      <td>14.526497</td>
      <td>1.102743</td>
      <td>0.806057</td>
      <td>49.693429</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.420000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>223.500000</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>20.125000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>7.910400</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>446.000000</td>
      <td>0.000000</td>
      <td>3.000000</td>
      <td>28.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>14.454200</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>668.500000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>38.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>31.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>891.000000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>80.000000</td>
      <td>8.000000</td>
      <td>6.000000</td>
      <td>512.329200</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(df_training.isnull().sum())
```

    PassengerId      0
    Survived         0
    Pclass           0
    Name             0
    Sex              0
    Age            177
    SibSp            0
    Parch            0
    Ticket           0
    Fare             0
    Cabin          687
    Embarked         2
    dtype: int64


# Data Preprocessing


```python
#the input data
data=[]

#get passenger id
passenger_id = np.array(df_training['PassengerId'])
data.append(passenger_id)

#get the passenger class from the data
passenger_class=np.array(df_training['Pclass'])
data.append(passenger_class)

#convert sex to numeric value i.e., 0 for male and 1 for female 
passenger_sex=np.array([0 if x=='male' else 1 for x in df_training['Sex']])
data.append(passenger_sex)

#some ages are NAN use average age in place of nan
passenger_age=np.array([df_training['Age'].mean() if pd.isnull(x) else x for x in df_training['Age']])
data.append(passenger_age)

#get the passenger SibSp from the data
passenger_sibsp=np.array(df_training['SibSp'])
data.append(passenger_sibsp)

#get the passenger Parch from the data
passenger_parch=np.array(df_training['Parch'])
data.append(passenger_parch)

#get the passenger Fare from the data
passenger_fare=np.array(df_training['Fare'])
data.append(passenger_fare)

#the output label
passenger_survival=np.array(df_training['Survived'])
data.append(passenger_survival)


final_data=np.transpose(np.array(data))
print(final_data)

#shuffle the input data
#np.random.shuffle(final_data)
#print(final_data)




```

    [[   1.        3.        0.     ...,    0.        7.25      0.    ]
     [   2.        1.        1.     ...,    0.       71.2833    1.    ]
     [   3.        3.        1.     ...,    0.        7.925     1.    ]
     ..., 
     [ 889.        3.        1.     ...,    2.       23.45      0.    ]
     [ 890.        1.        0.     ...,    0.       30.        1.    ]
     [ 891.        3.        0.     ...,    0.        7.75      0.    ]]



```python
import matplotlib.pyplot as plt

N = 3 # The 3 classes
dead=[0,0,0]
alive=[0,0,0]
#print(alive[3])

for i in range(len(final_data)):
    if final_data[:, 7][i]== 0:
        if final_data[:, 1][i] == 1:
            dead[0] += 1
        elif final_data[:, 1][i] == 2:
            dead[1] +=1
        else:
            dead[2] +=1
    else:
        if final_data[:, 1][i] == 1:
            alive[0] += 1
        elif final_data[:, 1][i] == 2:
            alive[1] +=1
        else:
            alive[2] +=1
```


```python
import matplotlib.pyplot as plt
%matplotlib inline
labels=['class1', 'class2', 'class3']
classes=range(3)
plt.bar(classes, alive, color='g', label='alive')
plt.bar(classes, dead, color='red', label='dead',  bottom=alive)
plt.xticks(classes,labels)
plt.legend(loc='best')
plt.show()

print(dead)
print(alive)
```


![png](/img/output_10_0.png)


    [80, 97, 372]
    [136, 87, 119]



```python
def convert_array_to_onehot_encoding(array):
    array=np.array(array)
    number_of_classes=len(np.unique(array))
    output=[]
    
    for item in array:
        one_hot=np.zeros(number_of_classes)
        one_hot[item]=1
        output.append(one_hot)
    return np.array(output)

X=final_data[:, 1:7]
Y=convert_array_to_onehot_encoding(final_data[:, 7].astype(int))
```

## splitting the data into training and validation using sklearn

```python
from sklearn.model_selection import train_test_split
x_train, x_test, y_train, y_test = train_test_split(X, Y, test_size=0.2)

print(x_train.shape)
print(y_train.shape)

print(x_test.shape)
print(y_test.shape)

```


```python
from keras.models import Sequential
from keras.layers.core import Dense

# create model
model = Sequential()
model.add(Dense(32, input_shape=(6,), activation='relu'))
model.add(Dense(64, activation='relu'))
model.add(Dense(32, activation='relu'))
model.add(Dense(2, activation='softmax'))
# Compile model
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])

# Fit the model
model.fit(X, Y, epochs=150, batch_size=10)

# evaluate the model on the original data
scores = model.evaluate(X, Y)
print("\n%s: %.2f%%" % (model.metrics_names[1], scores[1]*100))
```

    Using TensorFlow backend.


    Epoch 1/150
    891/891 [==============================] - 1s - loss: 0.7727 - acc: 0.6554     
    Epoch 2/150
    891/891 [==============================] - 0s - loss: 0.6595 - acc: 0.6801     
    Epoch 3/150
    891/891 [==============================] - 0s - loss: 0.6423 - acc: 0.6958     
    Epoch 4/150
    891/891 [==============================] - 0s - loss: 0.6607 - acc: 0.6667     
    Epoch 5/150
    891/891 [==============================] - 0s - loss: 0.6420 - acc: 0.6835     
    Epoch 6/150
    891/891 [==============================] - 0s - loss: 0.5530 - acc: 0.7363     
    Epoch 7/150
    891/891 [==============================] - 0s - loss: 0.5664 - acc: 0.7419     
    Epoch 8/150
    891/891 [==============================] - 0s - loss: 0.5861 - acc: 0.7250     
    Epoch 9/150
    891/891 [==============================] - 0s - loss: 0.5732 - acc: 0.7351     
    Epoch 10/150
    891/891 [==============================] - 0s - loss: 0.5481 - acc: 0.7632     
    Epoch 11/150
    891/891 [==============================] - 0s - loss: 0.5623 - acc: 0.7531     
    Epoch 12/150
    891/891 [==============================] - 0s - loss: 0.4869 - acc: 0.7767     
    Epoch 13/150
    891/891 [==============================] - 0s - loss: 0.5079 - acc: 0.7688     
    Epoch 14/150
    891/891 [==============================] - 0s - loss: 0.4578 - acc: 0.7957     
    Epoch 15/150
    891/891 [==============================] - 0s - loss: 0.4549 - acc: 0.7980     
    Epoch 16/150
    891/891 [==============================] - 0s - loss: 0.5497 - acc: 0.7654     
    Epoch 17/150
    891/891 [==============================] - 0s - loss: 0.4686 - acc: 0.7957     
    Epoch 18/150
    891/891 [==============================] - 0s - loss: 0.4916 - acc: 0.7935     
    Epoch 19/150
    891/891 [==============================] - 0s - loss: 0.4753 - acc: 0.7912     
    Epoch 20/150
    891/891 [==============================] - 0s - loss: 0.4621 - acc: 0.7980     
    Epoch 21/150
    891/891 [==============================] - 0s - loss: 0.4571 - acc: 0.7924     
    Epoch 22/150
    891/891 [==============================] - 0s - loss: 0.5650 - acc: 0.7677     
    Epoch 23/150
    891/891 [==============================] - 0s - loss: 0.5007 - acc: 0.7980     
    Epoch 24/150
    891/891 [==============================] - 0s - loss: 0.4552 - acc: 0.7890     
    Epoch 25/150
    891/891 [==============================] - 0s - loss: 0.4737 - acc: 0.8036     
    Epoch 26/150
    891/891 [==============================] - 0s - loss: 0.4518 - acc: 0.7991     
    Epoch 27/150
    891/891 [==============================] - 0s - loss: 0.4360 - acc: 0.8047     
    Epoch 28/150
    891/891 [==============================] - 0s - loss: 0.5005 - acc: 0.7856     
    Epoch 29/150
    891/891 [==============================] - 0s - loss: 0.4638 - acc: 0.8126     
    Epoch 30/150
    891/891 [==============================] - 0s - loss: 0.4486 - acc: 0.7935     
    Epoch 31/150
    891/891 [==============================] - 0s - loss: 0.4573 - acc: 0.8025     
    Epoch 32/150
    891/891 [==============================] - 0s - loss: 0.4375 - acc: 0.8171     
    Epoch 33/150
    891/891 [==============================] - 0s - loss: 0.4484 - acc: 0.7969     
    Epoch 34/150
    891/891 [==============================] - 0s - loss: 0.4674 - acc: 0.8092     
    Epoch 35/150
    891/891 [==============================] - 0s - loss: 0.4406 - acc: 0.8182     
    Epoch 36/150
    891/891 [==============================] - 0s - loss: 0.4439 - acc: 0.8159     
    Epoch 37/150
    891/891 [==============================] - 0s - loss: 0.4302 - acc: 0.8137     
    Epoch 38/150
    891/891 [==============================] - 0s - loss: 0.4250 - acc: 0.8137     
    Epoch 39/150
    891/891 [==============================] - 0s - loss: 0.4256 - acc: 0.8260     
    Epoch 40/150
    891/891 [==============================] - 0s - loss: 0.4295 - acc: 0.8137     
    Epoch 41/150
    891/891 [==============================] - 0s - loss: 0.4365 - acc: 0.8002     
    Epoch 42/150
    891/891 [==============================] - 0s - loss: 0.4518 - acc: 0.8148     
    Epoch 43/150
    891/891 [==============================] - 0s - loss: 0.4309 - acc: 0.8204     
    Epoch 44/150
    891/891 [==============================] - 0s - loss: 0.4325 - acc: 0.8159     
    Epoch 45/150
    891/891 [==============================] - 0s - loss: 0.4472 - acc: 0.8171     
    Epoch 46/150
    891/891 [==============================] - 0s - loss: 0.4166 - acc: 0.8227     
    Epoch 47/150
    891/891 [==============================] - 0s - loss: 0.4326 - acc: 0.8193     
    Epoch 48/150
    891/891 [==============================] - 0s - loss: 0.4309 - acc: 0.8013     
    Epoch 49/150
    891/891 [==============================] - 0s - loss: 0.4102 - acc: 0.8249     
    Epoch 50/150
    891/891 [==============================] - 0s - loss: 0.4178 - acc: 0.8126     
    Epoch 51/150
    891/891 [==============================] - 0s - loss: 0.4295 - acc: 0.8070     
    Epoch 52/150
    891/891 [==============================] - 0s - loss: 0.4077 - acc: 0.8272     
    Epoch 53/150
    891/891 [==============================] - 0s - loss: 0.4833 - acc: 0.7946     
    Epoch 54/150
    891/891 [==============================] - 0s - loss: 0.4146 - acc: 0.8260     
    Epoch 55/150
    891/891 [==============================] - 0s - loss: 0.4177 - acc: 0.8103     
    Epoch 56/150
    891/891 [==============================] - 0s - loss: 0.4208 - acc: 0.8114     
    Epoch 57/150
    891/891 [==============================] - 0s - loss: 0.4222 - acc: 0.7991     
    Epoch 58/150
    891/891 [==============================] - 0s - loss: 0.4087 - acc: 0.8260     
    Epoch 59/150
    891/891 [==============================] - 0s - loss: 0.4213 - acc: 0.8025     
    Epoch 60/150
    891/891 [==============================] - 0s - loss: 0.4160 - acc: 0.8103     
    Epoch 61/150
    891/891 [==============================] - 0s - loss: 0.4200 - acc: 0.8171     
    Epoch 62/150
    891/891 [==============================] - 0s - loss: 0.4185 - acc: 0.8249     
    Epoch 63/150
    891/891 [==============================] - 0s - loss: 0.4169 - acc: 0.8204     
    Epoch 64/150
    891/891 [==============================] - 0s - loss: 0.3999 - acc: 0.8294     
    Epoch 65/150
    891/891 [==============================] - 0s - loss: 0.4260 - acc: 0.8070     
    Epoch 66/150
    891/891 [==============================] - 0s - loss: 0.4014 - acc: 0.8182     
    Epoch 67/150
    891/891 [==============================] - 0s - loss: 0.4019 - acc: 0.8272     
    Epoch 68/150
    891/891 [==============================] - 0s - loss: 0.4153 - acc: 0.8215     
    Epoch 69/150
    891/891 [==============================] - 0s - loss: 0.4117 - acc: 0.8227     
    Epoch 70/150
    891/891 [==============================] - 0s - loss: 0.4013 - acc: 0.8316     
    Epoch 71/150
    891/891 [==============================] - 0s - loss: 0.4145 - acc: 0.8215     
    Epoch 72/150
    891/891 [==============================] - 0s - loss: 0.4090 - acc: 0.8294     
    Epoch 73/150
    891/891 [==============================] - 0s - loss: 0.4090 - acc: 0.8159     
    Epoch 74/150
    891/891 [==============================] - 0s - loss: 0.4253 - acc: 0.8058     
    Epoch 75/150
    891/891 [==============================] - 0s - loss: 0.3995 - acc: 0.8238     
    Epoch 76/150
    891/891 [==============================] - 0s - loss: 0.4162 - acc: 0.8103     
    Epoch 77/150
    891/891 [==============================] - 0s - loss: 0.4082 - acc: 0.8182     
    Epoch 78/150
    891/891 [==============================] - 0s - loss: 0.4042 - acc: 0.8249     
    Epoch 79/150
    891/891 [==============================] - 0s - loss: 0.4009 - acc: 0.8182     
    Epoch 80/150
    891/891 [==============================] - 0s - loss: 0.3913 - acc: 0.8339     
    Epoch 81/150
    891/891 [==============================] - 0s - loss: 0.4023 - acc: 0.8159     
    Epoch 82/150
    891/891 [==============================] - 0s - loss: 0.4052 - acc: 0.8182     
    Epoch 83/150
    891/891 [==============================] - 0s - loss: 0.4085 - acc: 0.8227     
    Epoch 84/150
    891/891 [==============================] - 0s - loss: 0.3954 - acc: 0.8294     
    Epoch 85/150
    891/891 [==============================] - 0s - loss: 0.4027 - acc: 0.8249     
    Epoch 86/150
    891/891 [==============================] - 0s - loss: 0.3986 - acc: 0.8238     
    Epoch 87/150
    891/891 [==============================] - 0s - loss: 0.3898 - acc: 0.8305     
    Epoch 88/150
    891/891 [==============================] - 0s - loss: 0.4051 - acc: 0.8092     
    Epoch 89/150
    891/891 [==============================] - 0s - loss: 0.3994 - acc: 0.8193     
    Epoch 90/150
    891/891 [==============================] - 0s - loss: 0.3996 - acc: 0.8204     
    Epoch 91/150
    891/891 [==============================] - 0s - loss: 0.3855 - acc: 0.8373     
    Epoch 92/150
    891/891 [==============================] - 0s - loss: 0.3889 - acc: 0.8339     
    Epoch 93/150
    891/891 [==============================] - 0s - loss: 0.3847 - acc: 0.8260     
    Epoch 94/150
    891/891 [==============================] - 0s - loss: 0.3901 - acc: 0.8215     
    Epoch 95/150
    891/891 [==============================] - 0s - loss: 0.3857 - acc: 0.8305     
    Epoch 96/150
    891/891 [==============================] - 0s - loss: 0.3908 - acc: 0.8249     
    Epoch 97/150
    891/891 [==============================] - 0s - loss: 0.3821 - acc: 0.8339     
    Epoch 98/150
    891/891 [==============================] - 0s - loss: 0.3964 - acc: 0.8227     
    Epoch 99/150
    891/891 [==============================] - 0s - loss: 0.3902 - acc: 0.8249     
    Epoch 100/150
    891/891 [==============================] - 0s - loss: 0.3883 - acc: 0.8238     
    Epoch 101/150
    891/891 [==============================] - 0s - loss: 0.3887 - acc: 0.8283     
    Epoch 102/150
    891/891 [==============================] - 0s - loss: 0.3829 - acc: 0.8227     
    Epoch 103/150
    891/891 [==============================] - 0s - loss: 0.4121 - acc: 0.8193     
    Epoch 104/150
    891/891 [==============================] - 0s - loss: 0.3863 - acc: 0.8350     
    Epoch 105/150
    891/891 [==============================] - 0s - loss: 0.3880 - acc: 0.8215     
    Epoch 106/150
    891/891 [==============================] - 0s - loss: 0.3857 - acc: 0.8328     
    Epoch 107/150
    891/891 [==============================] - 0s - loss: 0.3848 - acc: 0.8339     
    Epoch 108/150
    891/891 [==============================] - 0s - loss: 0.3901 - acc: 0.8294     
    Epoch 109/150
    891/891 [==============================] - 0s - loss: 0.3842 - acc: 0.8316     
    Epoch 110/150
    891/891 [==============================] - 0s - loss: 0.3876 - acc: 0.8294     
    Epoch 111/150
    891/891 [==============================] - 0s - loss: 0.3825 - acc: 0.8171     
    Epoch 112/150
    891/891 [==============================] - 0s - loss: 0.3742 - acc: 0.8384     
    Epoch 113/150
    891/891 [==============================] - 0s - loss: 0.3832 - acc: 0.8350     
    Epoch 114/150
    891/891 [==============================] - 0s - loss: 0.3874 - acc: 0.8294     
    Epoch 115/150
    891/891 [==============================] - 0s - loss: 0.3810 - acc: 0.8171     
    Epoch 116/150
    891/891 [==============================] - 0s - loss: 0.4017 - acc: 0.8227     
    Epoch 117/150
    891/891 [==============================] - 0s - loss: 0.3769 - acc: 0.8440     
    Epoch 118/150
    891/891 [==============================] - 0s - loss: 0.3770 - acc: 0.8418     
    Epoch 119/150
    891/891 [==============================] - 0s - loss: 0.3773 - acc: 0.8339     
    Epoch 120/150
    891/891 [==============================] - 0s - loss: 0.3873 - acc: 0.8283     
    Epoch 121/150
    891/891 [==============================] - 0s - loss: 0.3827 - acc: 0.8316     
    Epoch 122/150
    891/891 [==============================] - 0s - loss: 0.3779 - acc: 0.8373     
    Epoch 123/150
    891/891 [==============================] - 0s - loss: 0.3798 - acc: 0.8373     
    Epoch 124/150
    891/891 [==============================] - 0s - loss: 0.3808 - acc: 0.8294     
    Epoch 125/150
    891/891 [==============================] - 0s - loss: 0.3894 - acc: 0.8260     
    Epoch 126/150
    891/891 [==============================] - 0s - loss: 0.3737 - acc: 0.8350     
    Epoch 127/150
    891/891 [==============================] - 0s - loss: 0.3704 - acc: 0.8384     
    Epoch 128/150
    891/891 [==============================] - 0s - loss: 0.3740 - acc: 0.8272     
    Epoch 129/150
    891/891 [==============================] - 0s - loss: 0.3775 - acc: 0.8305     
    Epoch 130/150
    891/891 [==============================] - 0s - loss: 0.3689 - acc: 0.8384     
    Epoch 131/150
    891/891 [==============================] - 0s - loss: 0.3681 - acc: 0.8418     
    Epoch 132/150
    891/891 [==============================] - 0s - loss: 0.3717 - acc: 0.8339     
    Epoch 133/150
    891/891 [==============================] - 0s - loss: 0.3656 - acc: 0.8316     
    Epoch 134/150
    891/891 [==============================] - 0s - loss: 0.3701 - acc: 0.8373     
    Epoch 135/150
    891/891 [==============================] - 0s - loss: 0.3747 - acc: 0.8316     
    Epoch 136/150
    891/891 [==============================] - 0s - loss: 0.3685 - acc: 0.8260     
    Epoch 137/150
    891/891 [==============================] - 0s - loss: 0.3645 - acc: 0.8429     
    Epoch 138/150
    891/891 [==============================] - 0s - loss: 0.3722 - acc: 0.8395     
    Epoch 139/150
    891/891 [==============================] - 0s - loss: 0.3681 - acc: 0.8395     
    Epoch 140/150
    891/891 [==============================] - 0s - loss: 0.3652 - acc: 0.8418     
    Epoch 141/150
    891/891 [==============================] - 0s - loss: 0.3747 - acc: 0.8294     - ETA: 0s - loss: 0.4255 - ac
    Epoch 142/150
    891/891 [==============================] - 0s - loss: 0.3679 - acc: 0.8384     
    Epoch 143/150
    891/891 [==============================] - 0s - loss: 0.3684 - acc: 0.8305     
    Epoch 144/150
    891/891 [==============================] - 0s - loss: 0.3702 - acc: 0.8373     
    Epoch 145/150
    891/891 [==============================] - 0s - loss: 0.3944 - acc: 0.8227     
    Epoch 146/150
    891/891 [==============================] - 0s - loss: 0.3774 - acc: 0.8272     
    Epoch 147/150
    891/891 [==============================] - 0s - loss: 0.3743 - acc: 0.8429     
    Epoch 148/150
    891/891 [==============================] - 0s - loss: 0.3710 - acc: 0.8429     
    Epoch 149/150
    891/891 [==============================] - 0s - loss: 0.3772 - acc: 0.8361     
    Epoch 150/150
    891/891 [==============================] - 0s - loss: 0.3677 - acc: 0.8350     
     32/891 [>.............................] - ETA: 0s
    acc: 84.51%



```python
from IPython.display import SVG
from keras.utils.vis_utils import model_to_dot
SVG(model_to_dot(model).create(prog='dot', format='svg'))
model.save('titanic.h5')
```


```python
print(Y[1])
print(model.predict(np.reshape(X[1], (1, 6))))
print(model.predict_classes(np.reshape(X[1], (1, 6))))
```

    [ 1.  0.]
    [[ 0.88813961  0.11186044]]
    1/1 [==============================] - 0s
    [0]



```python
df_testing=pd.read_csv('data/test.csv')
display(df_testing.head())

```


<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>892</td>
      <td>3</td>
      <td>Kelly, Mr. James</td>
      <td>male</td>
      <td>34.5</td>
      <td>0</td>
      <td>0</td>
      <td>330911</td>
      <td>7.8292</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>1</th>
      <td>893</td>
      <td>3</td>
      <td>Wilkes, Mrs. James (Ellen Needs)</td>
      <td>female</td>
      <td>47.0</td>
      <td>1</td>
      <td>0</td>
      <td>363272</td>
      <td>7.0000</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>2</th>
      <td>894</td>
      <td>2</td>
      <td>Myles, Mr. Thomas Francis</td>
      <td>male</td>
      <td>62.0</td>
      <td>0</td>
      <td>0</td>
      <td>240276</td>
      <td>9.6875</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>3</th>
      <td>895</td>
      <td>3</td>
      <td>Wirz, Mr. Albert</td>
      <td>male</td>
      <td>27.0</td>
      <td>0</td>
      <td>0</td>
      <td>315154</td>
      <td>8.6625</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>896</td>
      <td>3</td>
      <td>Hirvonen, Mrs. Alexander (Helga E Lindqvist)</td>
      <td>female</td>
      <td>22.0</td>
      <td>1</td>
      <td>1</td>
      <td>3101298</td>
      <td>12.2875</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>



```python
print(df_testing.info())
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 418 entries, 0 to 417
    Data columns (total 11 columns):
    PassengerId    418 non-null int64
    Pclass         418 non-null int64
    Name           418 non-null object
    Sex            418 non-null object
    Age            332 non-null float64
    SibSp          418 non-null int64
    Parch          418 non-null int64
    Ticket         418 non-null object
    Fare           417 non-null float64
    Cabin          91 non-null object
    Embarked       418 non-null object
    dtypes: float64(2), int64(4), object(5)
    memory usage: 36.0+ KB
    None



```python
#the input data
test_data=[]

#get passenger id
test_passenger_id = np.array(df_testing['PassengerId'])
test_data.append(test_passenger_id)

#get the passenger class from the data
test_passenger_class=np.array(df_testing['Pclass'])
test_data.append(test_passenger_class)

#convert sex to numeric value i.e., 0 for male and 1 for female 
test_passenger_sex=np.array([0 if x=='male' else 1 for x in df_testing['Sex']])
test_data.append(test_passenger_sex)

#some ages are NAN use average age in place of nan
test_passenger_age=np.array([df_testing['Age'].mean() if pd.isnull(x) else x for x in df_testing['Age']])
test_data.append(test_passenger_age)

#get the passenger SibSp from the data
test_passenger_sibsp=np.array(df_testing['SibSp'])
test_data.append(test_passenger_sibsp)

#get the passenger Parch from the data
test_passenger_parch=np.array(df_testing['Parch'])
test_data.append(test_passenger_parch)

#get the passenger Fare from the data
test_passenger_fare=np.array(df_testing['Fare'])
test_data.append(test_passenger_fare)

test_final_data=np.transpose(np.array(test_data))
print(test_final_data)

#shuffle the input data
#np.random.shuffle(final_data)
#print(final_data)

test_passenger_id=test_final_data[:, 0].astype(int)
#print(test_passenger_id)

test_passenger_data= test_final_data[:, 1:7]
#print(test_passenger_data.shape)

file = open('submission.csv','w')  
file.write("PassengerId,Survived\n")


for i in range(0, len(test_passenger_data)):
               id=test_passenger_id[i]
               prediction=model.predict_classes(np.reshape(X[i], (1, 6)), verbose=False)
               file.write("{},{}\n".format(str(id), str(prediction[0])))
                
file.close() 




```

    [[  8.92000000e+02   3.00000000e+00   0.00000000e+00 ...,   0.00000000e+00
        0.00000000e+00   7.82920000e+00]
     [  8.93000000e+02   3.00000000e+00   1.00000000e+00 ...,   1.00000000e+00
        0.00000000e+00   7.00000000e+00]
     [  8.94000000e+02   2.00000000e+00   0.00000000e+00 ...,   0.00000000e+00
        0.00000000e+00   9.68750000e+00]
     ..., 
     [  1.30700000e+03   3.00000000e+00   0.00000000e+00 ...,   0.00000000e+00
        0.00000000e+00   7.25000000e+00]
     [  1.30800000e+03   3.00000000e+00   0.00000000e+00 ...,   0.00000000e+00
        0.00000000e+00   8.05000000e+00]
     [  1.30900000e+03   3.00000000e+00   0.00000000e+00 ...,   1.00000000e+00
        1.00000000e+00   2.23583000e+01]]



```python

```


```python

```
