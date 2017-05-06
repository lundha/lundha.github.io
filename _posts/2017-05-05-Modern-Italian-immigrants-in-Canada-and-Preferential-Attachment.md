
#### **Where do modern Italian immigrants go to live in Canada? Is their choice affected by preferential attachment?**
##### *A Data Science project carried out with Jupyter and R*
---

<img src="/images/Picture3.png" width="180" height="120"> 

I am *Italian*, and I know how traditionalists we are. Wherever we go, we look for Italian food, Italian clothes, Italian cars, and... *someone speaking Italian*. We share this feature with all the other Mediterranean peoples. Our Country is well known for the food, its nice weather, its culture, history, and music. But many ignore the fact that Italy has historically been harsh towards its sons and daughters, repeatedly forcing them to migrate in search for a better life. The [Italian Diaspora](https://en.wikipedia.org/wiki/Italian_diaspora) has had two main runs, first from 1860-1920, and then from 1945-1970. In 1913 alone, a mind-boggling 872,598 people fled. Many of them liked **Canada** and the promise of a better life it offered. I have always been fascinated with stories of people leaving the sad *double f* behind (family and famine), selling whatever they had, buying the ticket, and sailing across the Atlantic Ocean. Parents, relatives, acquintances, or the unknown for the unlucky ones, lied ahead. For many, that was a one-way journey driven by the need for a better future. But first, they had to do a stopover in Halifax, at [Pier 21](https://www.pier21.ca/home/).


![Migrants](/images/canadian-museum-of-immigration.jpg)

##### [Source](http://fantasticpixcool.com/canadian+museum+of+immigration+at+pier+21)
    

These days, not much seems to have changed. We are far more educated than our predecessors, possibly overeducated, but here we are again. Italy is currently fording its way through a muddy river of unemployment and economic crisis, partly due to catastrophic political choices and the evergreen *corruption*. This has caused upwards of 173,000 Italians to leave the Country from 2012-2013, 36.2% of which were aged 18-34. That is a conspicuous slice of its future that our Country is giving away, and the trend seems to be on the rise. Canada was, and still is, a favorite destination.

Italians used to settle in places where other Italians had settled, in an attempt to mitigate the shock. This, together with the many family-based sponsorships, made sure that the existing Italian communities grew larger. This phenomenon reminds me of the [preferential attachment](https://en.wikipedia.org/wiki/Preferential_attachment), which is studied in fields such as network theory [4]. According to it, new nodes are connected to pre-existing nodes in a network based on the number of connections that the latter already have. That's like saying that, at a party, you are much more willing to shake hands with and introduce yourself to those who are interacting with the highest number of people in the room. Which means, *you trust or are more attracted by* the most socially active people, as opposed to the lonely souls. 

A process like this, as [Albert-László Barabási](https://en.wikipedia.org/wiki/Albert-L%C3%A1szl%C3%B3_Barab%C3%A1si) described it in his works, results in large components growing larger, eventually evolving into a scale-free system. When the size of the components are plotted against their frequency, a [power law behavior](https://en.wikipedia.org/wiki/Power_law) emerges, where a few communities are very frequently preferred, the large majority being the road less traveled.

<img src="/images/Picture4.png" width="480" height="400">

My idea is to test whether the modern Italian immigrants - those admitted into Canada by means of a Permanent Residency during the period 2006-2015 - targeted the Canadian cities with the highest percentage of Italian residents, or whether they used other factors to select their intended destination.

To identify the places where Italians have historically settled, let's consider all Canadian cities with a population of Italian descent of at least 2,000 people. The total population data are available through [1], whereas the data regarding Italians come from [2]. Latitude and longitude values can be looked up in [3]. 


```R
library(maps)
library(mapdata)
data<-read.csv("C:\\Users\\Francesco\\Desktop\\Data_Science_portfolio\\Italians_in_Canada.csv")
newdata<-data[order(-data$Percentage),]
newdata
```

Dimensions | Megapixels
---|---
1,920 x 1,080 | 2.1MP
3,264 x 2,448 | 8MP
4,288 x 3,216 | 14MP

<table>
<thead><tr><th></th><th scope=col>ID</th><th scope=col>City</th><th scope=col>Province</th><th scope=col>Italians</th><th scope=col>Population</th><th scope=col>Percentage</th><th scope=col>Lat</th><th scope=col>Lon</th></tr></thead>
<tbody>
	<tr><th scope=row>14</th><td>14              </td><td>Bolton          </td><td>Ontario         </td><td>  3030          </td><td>  25954         </td><td>11.7            </td><td>43.52           </td><td> -79.44         </td></tr>
	<tr><th scope=row>5</th><td> 5              </td><td>St. Catharines  </td><td>Ontario         </td><td>  9915          </td><td> 133113         </td><td> 7.5            </td><td>43.11           </td><td> -79.14         </td></tr>
	<tr><th scope=row>2</th><td> 2              </td><td>Montreal        </td><td>Quebec          </td><td>108435          </td><td>1704694         </td><td> 6.3            </td><td>45.30           </td><td> -73.34         </td></tr>
	<tr><th scope=row>1</th><td> 1              </td><td>Toronto         </td><td>Ontario         </td><td>153285          </td><td>2731571         </td><td> 5.6            </td><td>43.42           </td><td> -79.24         </td></tr>
	<tr><th scope=row>11</th><td>11              </td><td>Sault Ste. Marie</td><td>Ontario         </td><td>  3295          </td><td>  73368         </td><td> 4.5            </td><td>46.32           </td><td> -84.21         </td></tr>
	<tr><th scope=row>7</th><td> 7              </td><td>Windsor         </td><td>Ontario         </td><td>  8515          </td><td> 217188         </td><td> 3.9            </td><td>42.17           </td><td> -83.00         </td></tr>
	<tr><th scope=row>3</th><td> 3              </td><td>Hamilton        </td><td>Ontario         </td><td> 18130          </td><td> 536917         </td><td> 3.4            </td><td>43.15           </td><td> -79.52         </td></tr>
	<tr><th scope=row>15</th><td>15              </td><td>Thunder Bay     </td><td>Ontario         </td><td>  2940          </td><td> 108359         </td><td> 2.7            </td><td>48.22           </td><td> -89.14         </td></tr>
	<tr><th scope=row>4</th><td> 4              </td><td>Vancouver       </td><td>British Columbia</td><td> 15430          </td><td> 631486         </td><td> 2.4            </td><td>49.15           </td><td>-123.60         </td></tr>
	<tr><th scope=row>12</th><td>12              </td><td>Oshawa          </td><td>Ontario         </td><td>  3290          </td><td> 149607         </td><td> 2.2            </td><td>43.54           </td><td> -78.51         </td></tr>
	<tr><th scope=row>17</th><td>17              </td><td>Guelph          </td><td>Ontario         </td><td>  2350          </td><td> 131794         </td><td> 1.8            </td><td>43.33           </td><td> -80.15         </td></tr>
	<tr><th scope=row>16</th><td>16              </td><td>Sudbury         </td><td>Ontario         </td><td>  2480          </td><td> 161531         </td><td> 1.5            </td><td>46.29           </td><td> -81.00         </td></tr>
	<tr><th scope=row>6</th><td> 6              </td><td>Ottawa          </td><td>Ontario         </td><td>  8595          </td><td> 934243         </td><td> 0.9            </td><td>45.25           </td><td> -75.41         </td></tr>
	<tr><th scope=row>13</th><td>13              </td><td>London          </td><td>Ontario         </td><td>  3140          </td><td> 383822         </td><td> 0.8            </td><td>42.98           </td><td> -81.25         </td></tr>
	<tr><th scope=row>9</th><td> 9              </td><td>Edmonton        </td><td>Alberta         </td><td>  4920          </td><td> 932546         </td><td> 0.5            </td><td>53.32           </td><td>-113.30         </td></tr>
	<tr><th scope=row>10</th><td>10              </td><td>Winnipeg        </td><td>Manitoba        </td><td>  3800          </td><td> 705244         </td><td> 0.5            </td><td>49.53           </td><td> -97.08         </td></tr>
	<tr><th scope=row>8</th><td> 8              </td><td>Calgary         </td><td>Alberta         </td><td>  5210          </td><td>1239220         </td><td> 0.4            </td><td>51.03           </td><td>-114.04         </td></tr>
</tbody>
</table>



**Table 1**

    

When plotted with circles, it is really evident where Italians traditionally live in Canada.


```R
options(repr.plot.width=7, repr.plot.height=5)
map("worldHires","Canada", xlim=c(-141,-53), ylim=c(40,85), col="gray90", fill=TRUE)
points(data$Lon, data$Lat, pch=19, col=rgb(255/255,0/255,0/255,alpha=0.5), cex=data$Percentage/2) 
par(xpd=TRUE) #Places legend outside the plot
legend(x=-60,y=85,title="Italians (%)",pt.cex=newdata$Percentage/2,bty="n",pch=19,legend=newdata$Percentage,text.col="black",
       col=rgb(255/255,0/255,0/255,alpha=0.5),pt.bg=rgb(255/255,0/255,0/255,alpha=0.5))
```

![Italians-in-Canada](/images/output_11_1.png)
**Figure 1**


The Ontario-Quebec macroregion is the most densely populated in Canada, combining for 21.82 million people as of 2014. This macroregion is home to the largest Italian groups, located primarily along the Windsor-Montreal corridor. Below is a snapshot centered on the Ontario-Quebec area:

### Ontario and Quebec


```R
ont <- newdata[newdata$Province == "Ontario" | newdata$Province == "Quebec", ] #|=or
ont
```


<table>
<thead><tr><th></th><th scope=col>ID</th><th scope=col>City</th><th scope=col>Province</th><th scope=col>Italians</th><th scope=col>Population</th><th scope=col>Percentage</th><th scope=col>Lat</th><th scope=col>Lon</th></tr></thead>
<tbody>
	<tr><th scope=row>14</th><td>14              </td><td>Bolton          </td><td>Ontario         </td><td>  3030          </td><td>  25954         </td><td>11.7            </td><td>43.52           </td><td>-79.44          </td></tr>
	<tr><th scope=row>5</th><td> 5              </td><td>St. Catharines  </td><td>Ontario         </td><td>  9915          </td><td> 133113         </td><td> 7.5            </td><td>43.11           </td><td>-79.14          </td></tr>
	<tr><th scope=row>2</th><td> 2              </td><td>Montreal        </td><td>Quebec          </td><td>108435          </td><td>1704694         </td><td> 6.3            </td><td>45.30           </td><td>-73.34          </td></tr>
	<tr><th scope=row>1</th><td> 1              </td><td>Toronto         </td><td>Ontario         </td><td>153285          </td><td>2731571         </td><td> 5.6            </td><td>43.42           </td><td>-79.24          </td></tr>
	<tr><th scope=row>11</th><td>11              </td><td>Sault Ste. Marie</td><td>Ontario         </td><td>  3295          </td><td>  73368         </td><td> 4.5            </td><td>46.32           </td><td>-84.21          </td></tr>
	<tr><th scope=row>7</th><td> 7              </td><td>Windsor         </td><td>Ontario         </td><td>  8515          </td><td> 217188         </td><td> 3.9            </td><td>42.17           </td><td>-83.00          </td></tr>
	<tr><th scope=row>3</th><td> 3              </td><td>Hamilton        </td><td>Ontario         </td><td> 18130          </td><td> 536917         </td><td> 3.4            </td><td>43.15           </td><td>-79.52          </td></tr>
	<tr><th scope=row>15</th><td>15              </td><td>Thunder Bay     </td><td>Ontario         </td><td>  2940          </td><td> 108359         </td><td> 2.7            </td><td>48.22           </td><td>-89.14          </td></tr>
	<tr><th scope=row>12</th><td>12              </td><td>Oshawa          </td><td>Ontario         </td><td>  3290          </td><td> 149607         </td><td> 2.2            </td><td>43.54           </td><td>-78.51          </td></tr>
	<tr><th scope=row>17</th><td>17              </td><td>Guelph          </td><td>Ontario         </td><td>  2350          </td><td> 131794         </td><td> 1.8            </td><td>43.33           </td><td>-80.15          </td></tr>
	<tr><th scope=row>16</th><td>16              </td><td>Sudbury         </td><td>Ontario         </td><td>  2480          </td><td> 161531         </td><td> 1.5            </td><td>46.29           </td><td>-81.00          </td></tr>
	<tr><th scope=row>6</th><td> 6              </td><td>Ottawa          </td><td>Ontario         </td><td>  8595          </td><td> 934243         </td><td> 0.9            </td><td>45.25           </td><td>-75.41          </td></tr>
	<tr><th scope=row>13</th><td>13              </td><td>London          </td><td>Ontario         </td><td>  3140          </td><td> 383822         </td><td> 0.8            </td><td>42.98           </td><td>-81.25          </td></tr>
</tbody>
</table>

**Table 2**

    

```R
options(repr.plot.width=7, repr.plot.height=5)
map("worldHires","Canada", xlim=c(-91.5,-65), ylim=c(40,57), col="gray90", fill=TRUE)
points(data$Lon, data$Lat, pch=19, col=rgb(255/255,0/255,0/255,alpha=0.4), cex=data$Percentage/2)
text(x=-89.14, y=48.22-0.8, labels="Thunder Bay", cex= 0.8,font=2)
text(x=-73.34+1, y=45.30-0.8, labels="Montreal", cex= 0.8,font=2)
text(x=-75.41-1, y=45.25+0.5, labels="Ottawa", cex= 0.8,font=2)
text(x=-83-2, y=42.17, labels="Windsor", cex= 0.8,font=2)
text(x=-84.21-1, y=46.32-0.8, labels="Sault Ste. Marie", cex= 0.8,font=2)
text(x=-81.00, y=46.29+0.5, labels="Sudbury", cex= 0.8,font=2)
text(x=-79.44+0.7, y=43.52+1, labels="Bolton", cex= 0.8,font=2)
text(x=-81.25-1.8, y=42.98+0.2, labels="London", cex= 0.8,font=2)
text(x=-80.15-1.8, y=43.33+0.2, labels="Guelph", cex= 0.8,font=2)
text(x=-79.44+3, y=43.11+0.5, labels="Toronto", cex= 0.8,font=2)
text(x=-79.44+3, y=43.11+0.1, labels="Hamilton", cex= 0.8,font=2)
text(x=-79.44+3, y=42.81, labels="St. Catharines", cex= 0.8,font=2)
text(x=-79.44+3, y=42.41, labels="Oshawa", cex= 0.8,font=2)
text(x=-81.00, y=46.29+3, labels="ONTARIO", cex= 0.8,font=3)
text(x=-73.34+1, y=45.30+5, labels="QUEBEC", cex= 0.8,font=3)
par(xpd=TRUE) #Places legend outside the plot
legend(x=-65,y=56,title="Italians (%)",pt.cex=c(1,2,2.5,3.5,6),bty="n",pch=19,legend=c("<1","2","5","8",">10"),
       text.col="black",col=rgb(255/255,0/255,0/255,alpha=0.4),pt.bg=rgb(255/255,0/255,0/255,alpha=0.4))
```

![Ontario-Quebec](/images/output_15_1.png)
**Figure 2**



As visible, the Italian population in this area is mainly living in the Toronto-Hamilton-St. Catharines-Oshawa-Bolton conurbation, which in 2014 had 3,577,162 residents. Of these, the Italians were a slice as large as 5.25%. Away from this hotbed, notable Italian presence can be observed in Montreal (6.3%), Sault Ste. Marie (4.5%), and Windsor (3.9%). 


```R
#Plotting
cols <- ifelse(ont$City == "Toronto" | ont$City == "Hamilton" | ont$City == "Oshawa" | 
               ont$City == "St. Catharines" | ont$City == "Bolton", "darkred","grey")
mybar<-barplot(ont$Percentage,names.arg=ont$City,main="Percentage of Italian Residents by City",ylab="%",
        ylim=c(0,12),cex.names=0.70,las=2,col=cols,)
lines(x = mybar, y = ont$Percentage,col="blue",lwd=3,lty="dashed")
legend(12, 11, legend=c("Macroregion","Other","Power law"),fill=c("darkred", "grey","blue"),cex=0.88)
mtext("Ontario and Quebec")
```

![Powerlaw](/images/output_17_1.png)
**Figure 3**



Now, it is clear that there always been a trend for Italians to go to Ontario (or Quebec, to a lesser extent). Only a few endeavoured to settle on the West Coast (Italians constitute 2.4% of Vancouver's population), and even less reached the Prairies (the Italians in Calgary, Edmonton, and Winnipeg do not exceed 0.5% of the total number of residents in each city, with Regina and Saskatoon not even making the cut).

Given this situation, I thought two questions were legitimate: 
1. **Where do modern Italian immigrants go to live, in Canada?** 
2. **Are they biased by the presence of other Italians upon selecting their destination?**

These questions are obviously referred to Permanent Residents (PRs) only, thus excluding temporary residents, students, and tourists. At any rate, this is also a good opportunity to detect the presence of the previously described [preferential attachment](https://en.wikipedia.org/wiki/Preferential_attachment). 

The Italian PRs newly admitted into Canada might decide that they want to settle in places with an Italian presence, hoping that this would facilitate the integration process. So, question number 2 becomes:

   **Are modern Italian immigrants (unknowingly) affected by preferential attachment?**
   
This question is all the more legitimate given the presence of that dashed blue line in Figure 3, which might suggest the presence of a power law behavior in the historical trend of Italian immigration to Canada. This, as Albert-László Barabási said, is the signature of preferential attachment.

To find the answer I am relying on the [Permanent Residents Datasets](http://open.canada.ca/data/en/dataset/ad975a26-df23-456a-8ada-756191a23695?page=2) made available by the Government of Canada. Particularly useful is the [Admissions of Permanent Residents by Province/Territory and Census Metropolitan Area (CMA) of Intended Destination and Country of Citizenship](http://www.cic.gc.ca/opendata-donneesouvertes/data/IRCC_PRadmiss_0017_E.xls) file. This open dataset reports the number of total PRs admitted into Canada from 2006-2016, divided by Country of origin and intended destination. I want to answer questions number 1 and 2 by computing the percentage of Italian PRs in the period 2006-2015 in each city in Table 2, and compare these values with those in **Figure 3**.

Unfortunately, the most remarkable part of the bar plot in Figure 3 - represented by the city of [Bolton, Ontario](https://en.wikipedia.org/wiki/Bolton,_Ontario) - is not plottable, as in the dataset it is part of the Toronto Census Metropolitan Area, and as such its PRs data are not available. This will make things somewhat different from Figure 3.

Table 3 shows the number of PRs issued to Italian immigrants in the period 2006-2015:


```R
itprs<-read.csv("C:\\Users\\Francesco\\Desktop\\Data_Science_portfolio\\Italian_PR_admissions.csv",header=TRUE,check.names=FALSE)
itprs<-itprs[order(itprs$City),]
itprs
```


<table>
<thead><tr><th></th><th scope=col>City</th><th scope=col>Province</th><th scope=col>2006</th><th scope=col>2007</th><th scope=col>2008</th><th scope=col>2009</th><th scope=col>2010</th><th scope=col>2011</th><th scope=col>2012</th><th scope=col>2013</th><th scope=col>2014</th><th scope=col>2015</th><th scope=col>Total</th></tr></thead>
<tbody>
	<tr><th scope=row>1</th><td>Bolton          </td><td>Ontario         </td><td> NA             </td><td> NA             </td><td> NA             </td><td> NA             </td><td> NA             </td><td> NA             </td><td> NA             </td><td> NA             </td><td> NA             </td><td> NA             </td><td>  NA            </td></tr>
	<tr><th scope=row>10</th><td>Guelph          </td><td>Ontario         </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>   0            </td></tr>
	<tr><th scope=row>7</th><td>Hamilton        </td><td>Ontario         </td><td>  5             </td><td>  5             </td><td>  5             </td><td> 15             </td><td>  5             </td><td> 10             </td><td>  0             </td><td>  5             </td><td>  0             </td><td> 15             </td><td>  55            </td></tr>
	<tr><th scope=row>13</th><td>London          </td><td>Ontario         </td><td>  0             </td><td>  0             </td><td>  5             </td><td>  5             </td><td>  5             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  15            </td></tr>
	<tr><th scope=row>3</th><td>Montreal        </td><td>Quebec          </td><td> 65             </td><td> 75             </td><td> 80             </td><td> 75             </td><td> 75             </td><td> 60             </td><td> 75             </td><td>110             </td><td>110             </td><td>125             </td><td> 850            </td></tr>
	<tr><th scope=row>9</th><td>Oshawa          </td><td>Ontario         </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  5             </td><td>  5             </td><td>  10            </td></tr>
	<tr><th scope=row>12</th><td>Ottawa          </td><td>Ontario         </td><td>  5             </td><td> 15             </td><td> 10             </td><td> 25             </td><td> 15             </td><td> 20             </td><td> 10             </td><td> 15             </td><td> 25             </td><td> 20             </td><td> 160            </td></tr>
	<tr><th scope=row>5</th><td>Sault Ste. Marie</td><td>Ontario         </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>   0            </td></tr>
	<tr><th scope=row>2</th><td>St. Catharines  </td><td>Ontario         </td><td>  0             </td><td>  5             </td><td>  5             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  5             </td><td>  0             </td><td>  15            </td></tr>
	<tr><th scope=row>11</th><td>Sudbury         </td><td>Ontario         </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  5             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>   5            </td></tr>
	<tr><th scope=row>8</th><td>Thunder Bay     </td><td>Ontario         </td><td>  0             </td><td>  0             </td><td>  5             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>   5            </td></tr>
	<tr><th scope=row>4</th><td>Toronto         </td><td>Ontario         </td><td>130             </td><td>100             </td><td>110             </td><td>150             </td><td>150             </td><td>125             </td><td>130             </td><td>165             </td><td>170             </td><td>215             </td><td>1445            </td></tr>
	<tr><th scope=row>6</th><td>Windsor         </td><td>Ontario         </td><td>  0             </td><td>  5             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  0             </td><td>  5             </td><td>  5             </td><td>  5             </td><td>  20            </td></tr>
</tbody>
</table>



**Table 3**

    

The total number of new PRs in the cities in Table 2 over the 2006-2015 period is derived from the same dataset as above:


```R
prs<-read.csv("C:\\Users\\Francesco\\Desktop\\Data_Science_portfolio\\Total_PR_admissions.csv",header=TRUE,check.names=FALSE)
prs<-prs[order(prs$City),]
prs
```


<table>
<thead><tr><th></th><th scope=col>City</th><th scope=col>Province</th><th scope=col>2006</th><th scope=col>2007</th><th scope=col>2008</th><th scope=col>2009</th><th scope=col>2010</th><th scope=col>2011</th><th scope=col>2012</th><th scope=col>2013</th><th scope=col>2014</th><th scope=col>2015</th><th scope=col>Total</th></tr></thead>
<tbody>
	<tr><th scope=row>1</th><td>Bolton          </td><td>Ontario         </td><td>   NA           </td><td>   NA           </td><td>   NA           </td><td>   NA           </td><td>   NA           </td><td>   NA           </td><td>   NA           </td><td>   NA           </td><td>   NA           </td><td>   NA           </td><td>    NA          </td></tr>
	<tr><th scope=row>10</th><td>Guelph          </td><td>Ontario         </td><td>  775           </td><td>  710           </td><td>  760           </td><td>  635           </td><td>  615           </td><td>  525           </td><td>  610           </td><td>  620           </td><td>  610           </td><td>  685           </td><td>  6545          </td></tr>
	<tr><th scope=row>7</th><td>Hamilton        </td><td>Ontario         </td><td> 3990           </td><td> 3645           </td><td> 3760           </td><td> 3705           </td><td> 3980           </td><td> 3255           </td><td> 4065           </td><td> 3225           </td><td> 3105           </td><td> 3020           </td><td> 35750          </td></tr>
	<tr><th scope=row>13</th><td>London          </td><td>Ontario         </td><td> 2980           </td><td> 2470           </td><td> 2335           </td><td> 2480           </td><td> 2940           </td><td> 2275           </td><td> 1865           </td><td> 2060           </td><td> 2000           </td><td> 1985           </td><td> 23390          </td></tr>
	<tr><th scope=row>3</th><td>Montreal        </td><td>Quebec          </td><td>35550           </td><td>35755           </td><td>36040           </td><td>39155           </td><td>42295           </td><td>40290           </td><td>42735           </td><td>40575           </td><td>39995           </td><td>39365           </td><td>391755          </td></tr>
	<tr><th scope=row>9</th><td>Oshawa          </td><td>Ontario         </td><td>  745           </td><td>  865           </td><td>  735           </td><td>  800           </td><td>  760           </td><td>  770           </td><td>  720           </td><td>  835           </td><td>  795           </td><td>  640           </td><td>  7665          </td></tr>
	<tr><th scope=row>12</th><td>Ottawa          </td><td>Ontario         </td><td> 6310           </td><td> 5830           </td><td> 6315           </td><td> 6830           </td><td> 7210           </td><td> 6450           </td><td> 6110           </td><td> 6040           </td><td> 5255           </td><td> 6245           </td><td> 62595          </td></tr>
	<tr><th scope=row>5</th><td>Sault Ste. Marie</td><td>Ontario         </td><td>   60           </td><td>   65           </td><td>   70           </td><td>   60           </td><td>   95           </td><td>   50           </td><td>   55           </td><td>   50           </td><td>   55           </td><td>   70           </td><td>   630          </td></tr>
	<tr><th scope=row>2</th><td>St. Catharines  </td><td>Ontario         </td><td> 1590           </td><td> 1380           </td><td> 1235           </td><td> 1115           </td><td> 1255           </td><td> 1155           </td><td>  965           </td><td>  980           </td><td>  915           </td><td>  920           </td><td> 11510          </td></tr>
	<tr><th scope=row>11</th><td>Sudbury         </td><td>Ontario         </td><td>  130           </td><td>  135           </td><td>  135           </td><td>  145           </td><td>  115           </td><td>  145           </td><td>  150           </td><td>  170           </td><td>  290           </td><td>  185           </td><td>  1600          </td></tr>
	<tr><th scope=row>8</th><td>Thunder Bay     </td><td>Ontario         </td><td>  180           </td><td>  145           </td><td>  140           </td><td>  120           </td><td>  150           </td><td>  115           </td><td>   95           </td><td>  105           </td><td>  130           </td><td>  150           </td><td>  1330          </td></tr>
	<tr><th scope=row>4</th><td>Toronto         </td><td>Ontario         </td><td>99015           </td><td>86890           </td><td>86665           </td><td>82400           </td><td>91855           </td><td>77535           </td><td>77030           </td><td>81465           </td><td>75650           </td><td>82110           </td><td>840615          </td></tr>
	<tr><th scope=row>6</th><td>Windsor         </td><td>Ontario         </td><td> 2840           </td><td> 2265           </td><td> 2015           </td><td> 1970           </td><td> 1875           </td><td> 1710           </td><td> 1240           </td><td> 1690           </td><td> 1455           </td><td> 1810           </td><td> 18870          </td></tr>
</tbody>
</table>



**Table 4**

    

The percentage of Italian PRs with respect to the totals in Table 4 is easy to compute:


```R
itprs$PRs_Percentage<-round(itprs$Total/prs$Total,digits=3)*100
df<-data.frame("City"=ont$City,"Historical_Percentage"=ont$Percentage,
               "Current_Percentage"=itprs$PRs_Percentage,check.names=FALSE)
df<-df[order(-df$Historical_Percentage),]
df
```


<table>
<thead><tr><th scope=col>City</th><th scope=col>Historical_Percentage</th><th scope=col>Current_Percentage</th></tr></thead>
<tbody>
	<tr><td>Bolton          </td><td>11.7            </td><td> NA             </td></tr>
	<tr><td>St. Catharines  </td><td> 7.5            </td><td>0.0             </td></tr>
	<tr><td>Montreal        </td><td> 6.3            </td><td>0.2             </td></tr>
	<tr><td>Toronto         </td><td> 5.6            </td><td>0.1             </td></tr>
	<tr><td>Sault Ste. Marie</td><td> 4.5            </td><td>0.2             </td></tr>
	<tr><td>Windsor         </td><td> 3.9            </td><td>0.1             </td></tr>
	<tr><td>Hamilton        </td><td> 3.4            </td><td>0.3             </td></tr>
	<tr><td>Thunder Bay     </td><td> 2.7            </td><td>0.0             </td></tr>
	<tr><td>Oshawa          </td><td> 2.2            </td><td>0.1             </td></tr>
	<tr><td>Guelph          </td><td> 1.8            </td><td>0.3             </td></tr>
	<tr><td>Sudbury         </td><td> 1.5            </td><td>0.4             </td></tr>
	<tr><td>Ottawa          </td><td> 0.9            </td><td>0.2             </td></tr>
	<tr><td>London          </td><td> 0.8            </td><td>0.1             </td></tr>
</tbody>
</table>


**Table 5**

    

Now, back to question 2: do modern Italians settle in places in Canada where the Italian presence is historically notable?


```R
names <- c("Bolton", "St. Catharines", "Montreal", "Toronto", "Sault Ste. Marie",
           "Windsor","Hamilton","Thunder Bay","Oshawa","Guelph","Sudbury","Ottawa","London")
barplot(cbind(df$Historical_Percentage,df$Current_Percentage), main="Historical vs Current Percentage of Italian Immigrants", 
        ylab="%", beside=TRUE, col=rep(c('darkblue', 'red'), each=13),ylim=c(0,12),names.arg=c(names,names),las=2,cex.names=0.7)
legend(13, 11.5, c("Historical","Current"), cex=0.88, fill=c("darkblue","red"))
```


![Historical-Current](/images/output_26_1.png)
**Figure 4**



By looking at Figure 4, we can conclude that:
**no, new Italians seem to be unaffected by preferential attachment**.

This means that they ignore the places where Italians have traditionally moved to, and instead use other criteria to select their destination. So, this is one big suriprise for me! 

The (tiny) numbers of new Italian immigrants no longer choose Bolton or St. Catharines as their top two favorite destinations. Bolton is included in the Toronto figure, so it is possible to conclude that its appeal has almost disappeared over the decades. New Italians, instead, head mostly to Sudbury, Hamilton, Guelph. Even Montreal and Toronto are the road less traveled these days, and this can be due to a number of factors, probably due to be house affordability and living costs.

To check whether the new Italians use *more affordable housing* as a relocation criterion, it is probably worth checking the average Shelter-To-Income-Ratio ([STIR](http://www12.statcan.gc.ca/nhs-enm/2011/ref/dict/households-menage028-eng.cfm)), which is the percentage of total before-tax household income spent on shelter. So, a higher STIR means a more expensive houshold. The following data ara available through the http://cmhc.beyond2020.com website, via the "Lauch Table Viewer" option. The unemployment rate values come from http://www.statcan.gc.ca/tables-tableaux/sum-som/l01/cst01/lfss03h-eng.htm.


```R
stir<-read.csv("C:\\Users\\Francesco\\Desktop\\Data_Science_portfolio\\Average_STIR_Canada.csv",header=TRUE,check.names=FALSE)
stir<-stir[order(stir$City),]
stir
```


<table>
<thead><tr><th></th><th scope=col>City</th><th scope=col>Average_STIR_2011_CMA</th><th scope=col>Unemployment_March2017</th></tr></thead>
<tbody>
	<tr><th scope=row>1</th><td>Bolton          </td><td>  NA            </td><td> NA             </td></tr>
	<tr><th scope=row>10</th><td>Guelph          </td><td>22.2            </td><td>5.4             </td></tr>
	<tr><th scope=row>7</th><td>Hamilton        </td><td>22.5            </td><td>5.9             </td></tr>
	<tr><th scope=row>13</th><td>London          </td><td>22.6            </td><td>6.0             </td></tr>
	<tr><th scope=row>3</th><td>Montreal        </td><td>23.2            </td><td>6.6             </td></tr>
	<tr><th scope=row>9</th><td>Oshawa          </td><td>22.5            </td><td>6.0             </td></tr>
	<tr><th scope=row>12</th><td>Ottawa          </td><td>21.0            </td><td>5.1             </td></tr>
	<tr><th scope=row>5</th><td>Sault Ste. Marie</td><td>19.7            </td><td>8.1             </td></tr>
	<tr><th scope=row>2</th><td>St. Catharines  </td><td>22.4            </td><td>6.4             </td></tr>
	<tr><th scope=row>11</th><td>Sudbury         </td><td>21.0            </td><td>7.4             </td></tr>
	<tr><th scope=row>8</th><td>Thunder Bay     </td><td>19.3            </td><td>5.8             </td></tr>
	<tr><th scope=row>4</th><td>Toronto         </td><td>24.7            </td><td>7.1             </td></tr>
	<tr><th scope=row>6</th><td>Windsor         </td><td>21.3            </td><td>8.8             </td></tr>
</tbody>
</table>


**Table 6**

    

Comparing Table 6 with Figure 4 reveals that Toronto and Montreal probably are not good choices due to the high STIR percentage, which includes all types of households. Toronto also has the worse unemployment rate of the two. On the other hand, Thunder Bay is likely receiving less attention due to its distance from the Golden Horseshoe area. Moving to Sudbury and Greater Sudbury seem to be a risk - given the third highest unemployment rate - that new Italians seem to be willing to take. All in all, the Ottawa-Gatineau CMA seems to be offering the better conditions.  

But which aspect drives the relocation choice of new Italians the most? Do they move more to cities with low STIRs, or to places with low unemployment rates? Let's take a look at two regression graphs: new Italian PRs vs STIR, and new Italian PRs vs Unemployment.


```R
x1=stir$Average_STIR_2011_CMA
x2=stir$Unemployment_March2017
y=itprs$Total
par(mfrow=c(1,2))
plot(x1, y, xlab="Average STIR (%)",ylab="new Italian PRs",main="Average STIR vs Italian PRs",col="red",pch=16,cex.main=0.9) 
grid()
plot(x2, y, xlab="Unemployment Rate (%)",ylab="new Italian PRs",main="Unemployment Rate vs Italian PRs",
     col="blue",pch=17,cex.main=0.9)   
grid()
```
    

![Scatter](/images/output_32_1.png)
**Figure 5**


Figure 5 reveals that new Italians don't supposedly look at things like STIR and unemployment rate upon selecting their destination in Canada, and that's because the PRs issued to new Italians **are not negatively correlated with STIR or the unemployment rate**. Toronto and Montreal are, expectedly, the most common choices given the opportunities they offer in terms of job market. It is also the case that in these two cities the STIR is higher than the unemployment rate. This could be good news, apparently, but one has to consider how many of the available jobs effectively allow people to save money and live a life of fulfillment. Italians in Canada have certainly stopped following the nationality-based preferential attachment, and have embraced the move to the largest cities. In a way, however, they are still following the preferential attachment pattern, only that nowadays they are attracted by better employment opportunities rather than the reassuring presence of compatriots. 

---

### Sources
1. Population data: https://en.wikipedia.org
2. Italian residents (*in Italian*): http://www.italiansinfuga.com/2013/08/08/dove-abitano-gli-italiani-in-canada/
3. Latitude and Longitude: https://en.wikipedia.org
4. Barabási, A.-L.; R. Albert (1999). "Emergence of scaling in random networks". Science. 286 (5439): 509–512
