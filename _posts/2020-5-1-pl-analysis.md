---
layout: post
title: Second post
images:
  - url: /images/mining.jpg
---


<img src="/images/mining.jpg"/>


## Exploratory data analysis of Premier League season 19/20

#### Using Pythons pandas to explore o/u 2.5 goals statistics

Reading the csv data and creating a pandas data frame to store it in

{% highlight python3 %}


pl_stats19_20 = pd.read_csv (r'/Users/Lund/pl-analysis/data/pl1920.txt')
df = pd.DataFrame(pl_stats19_20)

{% endhighlight %}

Starting by selecting the columns which we are going to use for our data analysis.
This will be to start with:




| Variable  | Description |
| ------------- | ------------- | 
| HomeTeam      | Home Team                     | 
| AwayTeam      | Away Team                     | 
| FTHG and HG   | Full Time Home Team Goals     | 
| FTAG and AG   | Full Time Away Team Goals     | 
| FTR and Res   | Full Time Result              | 
| HTHG          | Half Time Home Team Goals     | 
| HTAG          | Half Time Away Team Goals     | 
| HTR           | Half Time Result              | 
| HS            | Home Team Shots               | 
| AS            | Away Team Shots               | 
| HST           | Home Team Shots on Target     | 
| AST           | Away Team Shots on Target     | 
| HHW           | Home Team Hit Woodwork        |   
| AHW           | Away Team Hit Woodwork        | 
| HC            | Home Team Corners             |   
| AC            | Away Team Corners             | 
| HF            | Home Team Fouls Committed     | 
| AF            | Away Team Fouls Committed     | 
| HFKC          | Home Team Free Kicks Conceded |   
| AFKC          | Away Team Free Kicks Conceded |   
| HO            | Home Team Offsides            | 
| AO            | Away Team Offsides            | 
| HY            | Home Team Yellow Cards        | 
| AY            | Away Team Yellow Cards        | 
| HR            |  Home Team Red Cards          |   
| AR            | Away Team Red Cards           |     
| B365>2.5      | Bet365 over 2.5 goals         | 
|B365<2.5       |  Bet365 under 2.5 goals       |   
| P>2.5         | Pinnacle over 2.5 goals       |
| P<2.5         | Pinnacle under 2.5 goals      |   
| Max>2.5       | Market maximum over 2.5 goals |
| Max<2.5       | Market maximum under 2.5 goals|
| Avg>2.5       | Market average over 2.5 goals |
| Avg<2.5       | Market average under 2.5 goals|


Using numpy to select the desired columns to work with, where the 

{% highlight python3 %}
df = df.iloc[:, np.r_[3:11, 12:24, 48:56]]
{% endhighlight %}

#### Over/under and odds statistics 

Questions is

- How many % of matches ended up over 2.5 goals
- How many did the odds correspond to the statistics(i.e odds of 2.00 give 0.5 probability)

{% highlight python3 %}
df['TG'] = df['FTHG'] + df['FTAG']
df['TG>2.5'] = np.where(df['TG']>2.5, 1, 0
{% endhighlight %}

{% highlight python3 %}
print(df['TG>2.5'].value_counts(normalize=True))
{% endhighlight %}

Gave the percentages 


- Over 2.5 = 52.43%
- Under 2.5 = 47.57%

Further, the odds of over 2.5 goals was over 2 

{% highlight python3 %}
print(df['Odds>=2'].value_counts(normalize=True))
{% endhighlight %}

Gave the percentages


- Over or equal 2 = 22.92%
- Under 2 = 77.08%


### Goal expectancy and attack strength vs defense strength




