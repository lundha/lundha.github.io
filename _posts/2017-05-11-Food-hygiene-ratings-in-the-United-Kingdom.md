#### **Where are the dirtiest restaurants and hotels in the United Kingdom? How many of them failed to achieve maximum food hygiene ratings?**
##### A Data Science project carried out with Jupyter and Python | May 11, 2017
---

<img src="/images/Ratings.png" width="320" height="190"> 

Eating safe food is not always a given. While sitting at the table of a restaurant, I have seen a fair number of people being utterly disgusted by the sight of a dirty fork, a greased knife, or a glass (unexpectedly) embellished by lipstick stains. [Bill Bryson](https://en.wikipedia.org/wiki/Bill_Bryson) would be glad to know that sometimes all of these had devilishly gathered *at the same table*. The reaction is always the same, because in one way or another, we are all concerned with hygiene when we visit a restaurant, a hotel, a pub, or any other place (especially if we have to pay).

In the United Kingdom, as well as in most Countries, this matter is taken seriously by the Government. In 2001, following the proposal of a [detailed report](https://www.gov.uk/government/uploads/system/uploads/attachment_data/file/265718/fsa.pdf), the [Food Standards Agency](https://en.wikipedia.org/wiki/Food_Standards_Agency) was formed, the purpose being to screen all businesses that serve food. The idea was to make sure they complied with a set of hygiene standards, so that food always remained safe to eat.

In Scotland, food hygiene standards are overseen by [Food Standards Scotland](https://en.wikipedia.org/wiki/Food_Standards_Scotland), while in the rest of the United Kingdom they are enforced by the [Food Standards Agency](https://en.wikipedia.org/wiki/Food_Standards_Agency). 

<img src="/images/Hygiene_Agencies.png" width="300" height="80">

###### Images found [here](https://en.wikipedia.org/wiki/Food_Standards_Agency) and [here](https://en.wikipedia.org/wiki/Food_Standards_Scotland).



As of May 2017, there are 47,477 businesses to be screened in Scotland, and 469,897 in the rest of the United Kingdom, included Northern Ireland. This is a huge cohort, and the task is accomplished using *two different* schemes: the **Food Hygiene Information Scheme (FHIS)** in Scotland and the **Food Hygiene Rating Scheme (FHRS)** in the rest of the United Kingdom. The first uses *categorical* descriptors that are directly assigned to each business (Table 1), while the second adopts a *points scheme* that assigns high scores to poor hygiene conditions (Table 2).

<table>
<thead><tr><th scope="col" <td colspan=2>Scotland (FHIS)</th></tr></thead>
<tbody>
<tr><th scope="row">1</th><th>Pass and Eat Safe</th>  
<tr><th scope="row">2</th><th>Pass</th> 
<tr><th scope="row">3</th><th>Improvement Required</th>
<tr><th scope="row">4</th><th>Awaiting Publication</th>
<tr><th scope="row">5</th><th>Awaiting Inspection</th> 
</tbody>
</table>

**Table 1 - Ratings for Scotland**

<table>
<thead>
<tr><th scope="col" <td colspan=3>Rest of the UK (FHRS)</th></tr>
<tr><th scope="col" <td colspan=2>Hygiene score</th><th scope="col" <td colspan=1>Hygiene rating</th></tr>
</thead>
<tbody>
<tr><th scope="row">1</th><th>0-15</th><th>5</th></tr>  
<tr><th scope="row">2</th><th>20</th><th>4</th></tr> 
<tr><th scope="row">3</th><th>25-30</th><th>3</th></tr>
<tr><th scope="row">4</th><th>35-40</th><th>2</th></tr>
<tr><th scope="row">5</th><th>45-50</th><th>1</th></tr> 
<tr><th scope="row">6</th><th>>50</th><th>0</th></tr>
</tbody>
</table>

**Table 2 - Ratings for the rest of the UK**




