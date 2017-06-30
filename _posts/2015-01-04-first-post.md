---
layout: post
title: Classifier to estimate the surviving chance of Titanic passengers
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

Here are some samples extracted from the dataset:

There are 2 classes in our task 'not survived' (class 0) and 'survived' (class 1), and the passengers data have 8 features.

|PassengerId|Survived|Pclass|Name|Sex|Age|SibSp|Parch|Ticket|Fare|Cabin|Embarked|
|-----------|--------|------|----|---|---|-----|-----|------|----|-----|--------|
|1|0|3|Braund, Mr. Owen Harris|male|22.0|1|0|A/5 21171|7.2500|NaN|S|
|2|1|1|Cumings, Mrs. John Bradley (Florence Briggs Th...|female|38.0|1|0|PC 17599|71.2833|C85|C|


```python
import pandas as pd
import numpy as np
```


```python
df_train=pd.read_csv('data/train.csv')


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
||
|PassengerId|
|Survived|
|Pclass|
|Name|
|Sex|
|Age|
|SibSp|
|Parch|
|Ticket|
|Fare|
|Cabin|
|Embarked|
    </tr>
  </thead>
  <tbody>
    <tr>
      
      
    </tr>
    <tr>
|1|

    </tr>
    <tr>
|2|
|3
|1
|3
|Heikkinen, Miss. Laina
|female
|26.0
|0
|0
|STON/O2. 3101282
|7.9250
|NaN
|S
    </tr>
    <tr>
|3|
|4
|1
|1
|Futrelle, Mrs. Jacques Heath (Lily May Peel)
|female
|35.0
|1
|0
|113803
|53.1000
|C123
|S
    </tr>
    <tr>
|4|
|5
|0
|3
|Allen, Mr. William Henry
|male
|35.0
|0
|0
|373450
|8.0500
|NaN
|S
    </tr>
    <tr>
|5|
|6
|0
|3
|Moran, Mr. James
|male
|NaN
|0
|0
|330877
|8.4583
|NaN
|Q
    </tr>
    <tr>
|6|
|7
|0
|1
|McCarthy, Mr. Timothy J
|male
|54.0
|0
|0
|17463
|51.8625
|E46
|S
    </tr>
    <tr>
|7|
|8
|0
|3
|Palsson, Master. Gosta Leonard
|male
|2.0
|3
|1
|349909
|21.0750
|NaN
|S
    </tr>
    <tr>
|8|
|9
|1
|3
|Johnson, Mrs. Oscar W (Elisabeth Vilhelmina Berg)
|female
|27.0
|0
|2
|347742
|11.1333
|NaN
|S
    </tr>
    <tr>
|9|
|10
|1
|2
|Nasser, Mrs. Nicholas (Adele Achem)
|female
|14.0
|1
|0
|237736
|30.0708
|NaN
|C
    </tr>
    <tr>
|10|
|11
|1
|3
|Sandstrom, Miss. Marguerite Rut
|female
|4.0
|1
|1
|PP 9549
|16.7000
|G6
|S
    </tr>
    <tr>
|11|
|12
|1
|1
|Bonnell, Miss. Elizabeth
|female
|58.0
|0
|0
|113783
|26.5500
|C103
|S
    </tr>
    <tr>
|12|
|13
|0
|3
|Saundercock, Mr. William Henry
|male
|20.0
|0
|0
|A/5. 2151
|8.0500
|NaN
|S
    </tr>
    <tr>
|13|
|14
|0
|3
|Andersson, Mr. Anders Johan
|male
|39.0
|1
|5
|347082
|31.2750
|NaN
|S
    </tr>
    <tr>
|14|
|15
|0
|3
|Vestrom, Miss. Hulda Amanda Adolfina
|female
|14.0
|0
|0
|350406
|7.8542
|NaN
|S
    </tr>
    <tr>
|15|
|16
|1
|2
|Hewlett, Mrs. (Mary D Kingcome)
|female
|55.0
|0
|0
|248706
|16.0000
|NaN
|S
    </tr>
    <tr>
|16|
|17
|0
|3
|Rice, Master. Eugene
|male
|2.0
|4
|1
|382652
|29.1250
|NaN
|Q
    </tr>
    <tr>
|17|
|18
|1
|2
|Williams, Mr. Charles Eugene
|male
|NaN
|0
|0
|244373
|13.0000
|NaN
|S
    </tr>
    <tr>
|18|
|19
|0
|3
|Vander Planke, Mrs. Julius (Emelia Maria Vande...
|female
|31.0
|1
|0
|345763
|18.0000
|NaN
|S
    </tr>
    <tr>
|19|
|20
|1
|3
|Masselmani, Mrs. Fatima
|female
|NaN
|0
|0
|2649
|7.2250
|NaN
|C
    </tr>
    <tr>
|20|
|21
|0
|2
|Fynney, Mr. Joseph J
|male
|35.0
|0
|0
|239865
|26.0000
|NaN
|S
    </tr>
    <tr>
|21|
|22
|1
|2
|Beesley, Mr. Lawrence
|male
|34.0
|0
|0
|248698
|13.0000
|D56
|S
    </tr>
    <tr>
|22|
|23
|1
|3
|McGowan, Miss. Anna "Annie"
|female
|15.0
|0
|0
|330923
|8.0292
|NaN
|Q
    </tr>
    <tr>
|23|
|24
|1
|1
|Sloper, Mr. William Thompson
|male
|28.0
|0
|0
|113788
|35.5000
|A6
|S
    </tr>
    <tr>
|24|
|25
|0
|3
|Palsson, Miss. Torborg Danira
|female
|8.0
|3
|1
|349909
|21.0750
|NaN
|S
    </tr>
    <tr>
|25|
|26
|1
|3
|Asplund, Mrs. Carl Oscar (Selma Augusta Emilia...
|female
|38.0
|1
|5
|347077
|31.3875
|NaN
|S
    </tr>
    <tr>
|26|
|27
|0
|3
|Emir, Mr. Farred Chehab
|male
|NaN
|0
|0
|2631
|7.2250
|NaN
|C
    </tr>
    <tr>
|27|
|28
|0
|1
|Fortune, Mr. Charles Alexander
|male
|19.0
|3
|2
|19950
|263.0000
|C23 C25 C27
|S
    </tr>
    <tr>
|28|
|29
|1
|3
|O'Dwyer, Miss. Ellen "Nellie"
|female
|NaN
|0
|0
|330959
|7.8792
|NaN
|Q
    </tr>
    <tr>
|29|
|30
|0
|3
|Todoroff, Mr. Lalio
|male
|NaN
|0
|0
|349216
|7.8958
|NaN
|S
    </tr>
    <tr>
|...|
|...
|...
|...
|...
|...
|...
|...
|...
|...
|...
|...
|...
    </tr>
    <tr>
|861|
|862
|0
|2
|Giles, Mr. Frederick Edward
|male
|21.0
|1
|0
|28134
|11.5000
|NaN
|S
    </tr>
    <tr>
|862|
|863
|1
|1
|Swift, Mrs. Frederick Joel (Margaret Welles Ba...
|female
|48.0
|0
|0
|17466
|25.9292
|D17
|S
    </tr>
    <tr>
|863|
|864
|0
|3
|Sage, Miss. Dorothy Edith "Dolly"
|female
|NaN
|8
|2
|CA. 2343
|69.5500
|NaN
|S
    </tr>
    <tr>
|864|
|865
|0
|2
|Gill, Mr. John William
|male
|24.0
|0
|0
|233866
|13.0000
|NaN
|S
    </tr>
    <tr>
|865|
|866
|1
|2
|Bystrom, Mrs. (Karolina)
|female
|42.0
|0
|0
|236852
|13.0000
|NaN
|S
    </tr>
    <tr>
|866|
|867
|1
|2
|Duran y More, Miss. Asuncion
|female
|27.0
|1
|0
|SC/PARIS 2149
|13.8583
|NaN
|C
    </tr>
    <tr>
|867|
|868
|0
|1
|Roebling, Mr. Washington Augustus II
|male
|31.0
|0
|0
|PC 17590
|50.4958
|A24
|S
    </tr>
    <tr>
|868|
|869
|0
|3
|van Melkebeke, Mr. Philemon
|male
|NaN
|0
|0
|345777
|9.5000
|NaN
|S
    </tr>
    <tr>
|869|
|870
|1
|3
|Johnson, Master. Harold Theodor
|male
|4.0
|1
|1
|347742
|11.1333
|NaN
|S
    </tr>
    <tr>
|870|
|871
|0
|3
|Balkic, Mr. Cerin
|male
|26.0
|0
|0
|349248
|7.8958
|NaN
|S
    </tr>
    <tr>
|871|
|872
|1
|1
|Beckwith, Mrs. Richard Leonard (Sallie Monypeny)
|female
|47.0
|1
|1
|11751
|52.5542
|D35
|S
    </tr>
    <tr>
|872|
|873
|0
|1
|Carlsson, Mr. Frans Olof
|male
|33.0
|0
|0
|695
|5.0000
|B51 B53 B55
|S
    </tr>
    <tr>
|873|
|874
|0
|3
|Vander Cruyssen, Mr. Victor
|male
|47.0
|0
|0
|345765
|9.0000
|NaN
|S
    </tr>
    <tr>
|874|
|875
|1
|2
|Abelson, Mrs. Samuel (Hannah Wizosky)
|female
|28.0
|1
|0
|P/PP 3381
|24.0000
|NaN
|C
    </tr>
    <tr>
|875|
|876
|1
|3
|Najib, Miss. Adele Kiamie "Jane"
|female
|15.0
|0
|0
|2667
|7.2250
|NaN
|C
    </tr>
    <tr>
|876|
|877
|0
|3
|Gustafsson, Mr. Alfred Ossian
|male
|20.0
|0
|0
|7534
|9.8458
|NaN
|S
    </tr>
    <tr>
|877|
|878
|0
|3
|Petroff, Mr. Nedelio
|male
|19.0
|0
|0
|349212
|7.8958
|NaN
|S
    </tr>
    <tr>
|878|
|879
|0
|3
|Laleff, Mr. Kristo
|male
|NaN
|0
|0
|349217
|7.8958
|NaN
|S
    </tr>
    <tr>
|879|
|880
|1
|1
|Potter, Mrs. Thomas Jr (Lily Alexenia Wilson)
|female
|56.0
|0
|1
|11767
|83.1583
|C50
|C
    </tr>
    <tr>
|880|
|881
|1
|2
|Shelley, Mrs. William (Imanita Parrish Hall)
|female
|25.0
|0
|1
|230433
|26.0000
|NaN
|S
    </tr>
    <tr>
|881|
|882
|0
|3
|Markun, Mr. Johann
|male
|33.0
|0
|0
|349257
|7.8958
|NaN
|S
    </tr>
    <tr>
|882|
|883
|0
|3
|Dahlberg, Miss. Gerda Ulrika
|female
|22.0
|0
|0
|7552
|10.5167
|NaN
|S
    </tr>
    <tr>
|883|
|884
|0
|2
|Banfield, Mr. Frederick James
|male
|28.0
|0
|0
|C.A./SOTON 34068
|10.5000
|NaN
|S
    </tr>
    <tr>
|884|
|885
|0
|3
|Sutehall, Mr. Henry Jr
|male
|25.0
|0
|0
|SOTON/OQ 392076
|7.0500
|NaN
|S
    </tr>
    <tr>
|885|
|886
|0
|3
|Rice, Mrs. William (Margaret Norton)
|female
|39.0
|0
|5
|382652
|29.1250
|NaN
|Q
    </tr>
    <tr>
|886|
|887
|0
|2
|Montvila, Rev. Juozas
|male
|27.0
|0
|0
|211536
|13.0000
|NaN
|S
    </tr>
    <tr>
|887|
|888
|1
|1
|Graham, Miss. Margaret Edith
|female
|19.0
|0
|0
|112053
|30.0000
|B42
|S
    </tr>
    <tr>
|888|
|889
|0
|3
|Johnston, Miss. Catherine Helen "Carrie"
|female
|NaN
|1
|2
|W./C. 6607
|23.4500
|NaN
|S
    </tr>
    <tr>
|889|
|890
|1
|1
|Behr, Mr. Karl Howell
|male
|26.0
|0
|0
|111369
|30.0000
|C148
|C
    </tr>
    <tr>
|890|
|891
|0
|3
|Dooley, Mr. Patrick
|male
|32.0
|0
|0
|370376
|7.7500
|NaN
|Q
    </tr>
  </tbody>
</table>
<p>891 rows × 12 columns</p>
</div>