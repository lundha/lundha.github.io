---
layout: post
title: Second post
images:
  - url: /images/mining.jpg
---


<img src="/images/mining.jpg"/>


## Exploratory data analysis of Premier League season 19/20

### Using Pythons pandas to explore o/u 2.5 goals stats

Reading the csv data and creating a pandas data frame to store it in

```python

pl_stats19_20 = pd.read_csv (r'/Users/Lund/pl-analysis/data/pl1920.txt')
df = pd.DataFrame(pl_stats19_20)

```
Starting by selecting the columns which we are going to use for our data analysis.
This will be to start with:



| Variable      | Description |
| ------------- |:-------------:| 
| HomeTeam      | Home Team | 
| AwayTeam      | Away Team      | 
|  FTHG and HG  | Full Time Home Team Goals      | 
| FTAG and AG   | Full Time Away Team Goals           | 
| FTR and Res   | Full Time Result (H=Home Win, D=Draw, A=Away Win) | 
| HTHG          | Half Time Home Team Goals      | 
| HTAG          | Half Time Away Team Goals      | 
| HTR           | Half Time Result (H=Home Win, D=Draw, A=Away Win)           | 
| HS            | Home Team Shots      | 
| AS            | Away Team Shots     | 
| HST           | Home Team Shots on Target          | 
| AST           | Away Team Shots on Target | 
| HHW           | Home Team Hit Woodwork     |   
| AHW           | Away Team Hit Woodwork      | 
| HC            | Home Team Corners      | 
| AC            | Away Team Corners      | 
| HF           |Home Team Fouls Committed           | 
| AF           | Away Team Fouls Committed | 
| HFKC           | Home Team Free Kicks Conceded      |   
| AFKC          |  Away Team Free Kicks Conceded      |   
| HO            | Home Team Offsides      | 
| AO            | Away Team Offsides      | 
| HY           | Home Team Yellow Cards          | 
| AY           | Away Team Yellow Cards | 
| HR           |  Home Team Red Cards    |   
| AR           | Away Team Red Cards      |
| HBP           | Home Team Bookings Points (10 = yellow, 25 = red)      |   
| ABP           | Away Team Bookings Points (10 = yellow, 25 = red)      |      

Thus, this is the columns 3:10 and 14:34

```python
df.drop(df.columns[0:3], axis = 1, inplace = True)


```

