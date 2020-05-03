---
layout: post
title: Kaggle Titanic Challenge
images:
  - url: /images/titanic2.jpg
---

### Kaggle Titanic Challenge

<img src="/images/titanic-challenge.jpg"/>

### Data dictionary


<div class="table-wrapper" markdown="block">

| Variable  |       Definition  |    Key        |
| ------------- | ------------- | -------------- 
| survival      | Survival     |    0 = No, 1 = Yes            | 
| pclass      | Ticket class     |  1 = 1st, 2 = 2nd, 3 = 3rd              | 
| sex   | Sex           |     | 
| Age   |   Age in years            |     | 
| sibsp   | # of siblings / spouses aboard the Titanic|              | 
| parch          |    # of parents / children aboard the Titanic | | 
| ticket         |Ticket number|     | 
| fare          | Passenger fare  |            | 
| cabin            | Cabin number  |            | 
| embarked            | Port of Embarkation  |    C = Cherbourg, Q = Queenstown, S = Southampton         | 

</div>

Read train and test data. Train data consists of 890 rows of passenger info, whereas test data consists of 417.
   
| Data          |    #    |    %       |
| ------------- | ------- | ------------ 
| Train          | 890     |      68.1      |
| Test         | 417     |      31.9      |

{% highlight python3 %}
t_train = pd.read_csv (r'/Users/Lund/Documents/kaggle-titanic-challenge/data/train.csv')
t_test = pd.read_csv (r'/Users/Lund/Documents/kaggle-titanic-challenge/data/test.csv')
{% endhighlight %}

