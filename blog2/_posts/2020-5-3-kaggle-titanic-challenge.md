---
layout: post
title: Kaggle Titanic Challenge
images:
  - url: /images/titanic2.jpg
---

<img src="/images/titanic-challenge.jpg" width="800" height="500"/>


### Kaggle Titanic Challenge


### Data dictionary


<div class="table-wrapper" markdown="block">

| Variable  |       Definition  |    Key        |
| ------------- | ------------- | -------------- 
| Survived      | Survival     |    0 = No, 1 = Yes            | 
| PassengerId      | PassengerID     |         | 
| Pclass      | Ticket class     |  1 = 1st, 2 = 2nd, 3 = 3rd              | 
| Sex   | Sex           |     | 
| Age   |   Age in years            |     | 
| SibSp   | # of siblings / spouses aboard the Titanic|              | 
| Parch          |    # of parents / children aboard the Titanic | | 
| Ticket         |Ticket number|     | 
| Fare          | Passenger fare  |            | 
| Cabin            | Cabin number  |            | 
| Embarked            | Port of Embarkation  |    C = Cherbourg, Q = Queenstown, S = Southampton         | 

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


### Descriptive analysis of the data finds the mean age, number of passengers in each class and the gender distribution on board

{% highlight python3 %}
mean_age = df_total["Age"].mean()

print(df_total["Pclass"].value_counts(normalize=True))
print(df_total["Embarked"].value_counts(normalize=True))
print(df_total["Sex"].value_counts(normalize=True))
{% endhighlight %}

Mean age was 29.9. 69.9% of the passangers embarked in Southampton, whereas 20.7% and 9.4% embarked 
in Cherbourg and Queenstown, respectively. Further, 64.4% of the passengers were men, while 35.6% of the passengers were women.
The ticket class distribution can be used as a proxy for the socio-economic distribution, and it was as shown in the table:


| Variable          |    1st class    |    2nd class       |   3rd class       |
| ------------- |   -------             | -----------        | -----------      |
| Pclass          | 24.7%   |    21.1%                     |     54.2%          |  


### Linear regression 


Linear regression is a statistical model that examines the linear relationship between
two(Simple) or more(Multiple) variables.

 {% highlight python3 %}
import statsmodels.api as sm
{% endhighlight %}

Isolating the target variable 

{% highlight python3 %}
df_survived = pd.DataFrame(df_train, columns=["Survived"])
{% endhighlight %}


Perform an easy clean of the data set by only including columns with integers or replacing strings with factors
where it is possible.

{% highlight python3 %}
df_test = df_test.drop('Name', 1)

df_test['Sex'].replace('male', 1,inplace=True)
df_test['Sex'].replace('female', 0,inplace=True)

{% endhighlight %}


{% highlight python3 %}

model = sm.GLM(df_survived, df_train_wo_survived, family=sm.families.Binomial()).fit()

df_prediction = pd.DataFrame(model.predict(df_test).fillna(0))
df_prediction.columns = ["prediction"]

df_prediction["prediction"] = np.where(df_prediction["prediction"]>0.5, 1, 0)


print(df_prediction["prediction"].value_counts(normalize=True))


{% endhighlight %}


The distribution of survivors from the training set was

| Survived          |    %    | 
| ------------- | ------- | 
| Yes          | 61.6%     |  
| No         | 38.4%     |  

From the prediction

| Predicted survived  |    %    | 
| ------------- | ------- | 
| Yes          | 65.3%     |  
| No         | 34.7%     | 


Comparing with the the actual number of survivors from the test set gave an accuracy of 
0.7676. With the predicted survivors, the following plot give the probability of surviving given the ticket class.

<img src="/images/surVpclass.png"/>


