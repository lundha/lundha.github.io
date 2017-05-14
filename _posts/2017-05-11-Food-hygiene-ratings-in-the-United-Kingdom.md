#### **Where are the dirtiest restaurants and hotels in the United Kingdom? How many of them failed to achieve maximum food hygiene ratings?**
##### A Data Science project carried out with Jupyter and Python | May 11, 2017
---

<img src="/images/Ratings.png" width="320" height="190"> 

### Intro

Eating safe food is not always a given. While sitting at the table of a restaurant, I have seen a fair number of people being utterly disgusted by the sight of a dirty fork, a greased knife, or a glass (unexpectedly) embellished by lipstick stains. [Bill Bryson](https://en.wikipedia.org/wiki/Bill_Bryson) would be glad to know that sometimes all of these had devilishly gathered *at the same table*. The reaction is always the same, because in one way or another, we are all concerned with hygiene when we visit a restaurant, a hotel, a pub, or any other place (especially if we have to pay).

In the United Kingdom, as well as in most Countries, this matter is taken seriously by the Government. In 2001, following the proposal of a [detailed report](https://www.gov.uk/government/uploads/system/uploads/attachment_data/file/265718/fsa.pdf), the [Food Standards Agency](https://en.wikipedia.org/wiki/Food_Standards_Agency) was formed, the purpose being to screen all businesses that serve food. The idea was to make sure they complied with a set of hygiene standards, so that food always remained safe to eat.

In Scotland, food hygiene standards are overseen by [Food Standards Scotland](https://en.wikipedia.org/wiki/Food_Standards_Scotland), while in the rest of the United Kingdom they are enforced by the [Food Standards Agency](https://en.wikipedia.org/wiki/Food_Standards_Agency). 

<img src="/images/Hygiene_Agencies.png" width="300" height="80">

###### Images found [here](https://en.wikipedia.org/wiki/Food_Standards_Agency) and [here](https://en.wikipedia.org/wiki/Food_Standards_Scotland).



As of May 2017, there are 47,477 businesses to be screened in Scotland, and 469,897 in the rest of the United Kingdom, including Northern Ireland. This is a huge cohort, and the task is accomplished using *two different* schemes: the **Food Hygiene Information Scheme (FHIS)** in Scotland and the **Food Hygiene Rating Scheme (FHRS)** in the rest of the United Kingdom. The first uses *categorical* descriptors that are directly assigned to each business (Table 1), while the second adopts a *points scheme* that assigns high scores to poor hygiene conditions (Table 2).



<table class="tg">
  <tr>
    <th class="tg-baqh" colspan="2">Scotland (FHIS)</th>
  </tr>
  <tr>
    <td class="tg-baqh">1</td>
    <td class="tg-baqh">Pass and Eat Safe</td>
  </tr>
  <tr>
    <td class="tg-baqh">2</td>
    <td class="tg-baqh">Pass</td>
  </tr>
  <tr>
    <td class="tg-baqh">3</td>
    <td class="tg-baqh">Improvement Required</td>
  </tr>
  <tr>
    <td class="tg-baqh">4</td>
    <td class="tg-baqh">Awaiting Publication</td>
  </tr>
  <tr>
    <td class="tg-baqh">5</td>
    <td class="tg-baqh">Awaiting Inspection</td>
  </tr>
</table>



**Table 1 - Ratings for Scotland**

<table class="tg">
  <tr>
    <th class="tg-baqh" colspan="2">Rest of the UK (FHRS)</th>
  </tr>
  <tr>
    <td class="tg-baqh">Hygiene score</td>
    <td class="tg-baqh">Hygiene rating</td>
  </tr>
  <tr>
    <td class="tg-baqh">0-15</td>
    <td class="tg-baqh">5</td>
  </tr>
  <tr>
    <td class="tg-baqh">20</td>
    <td class="tg-baqh">4</td>
  </tr>
  <tr>
    <td class="tg-baqh">25-30</td>
    <td class="tg-baqh">3</td>
  </tr>
  <tr>
    <td class="tg-baqh">35-40</td>
    <td class="tg-baqh">2</td>
  </tr>
  <tr>
    <td class="tg-baqh">45-50</td>
    <td class="tg-baqh">1</td>
  </tr>
  <tr>
    <td class="tg-baqh">&gt;50</td>
    <td class="tg-baqh">0</td>
  </tr>
</table>


**Table 2 - Ratings for the rest of the UK**


### Parsing xml files from the Food Standards Agency website

To figure out which areas of the UK host the dirtiest businesses food-wise, it is necessary to parse the [Food Standards Agency website](http://ratings.food.gov.uk/open-data/), where data are publicly available to download. These come in the form of [xml](https://en.wikipedia.org/wiki/XML) files that are grouped by region and local authority (Scotland is included). Given the different rating system in place for Scotland, a separate analysis is required, and the results will not be adjusted to those of the rest of the UK.

This is about:
1. Scraping the Food Standards Agency html page to extract the file names to be downloaded;
2. For each region on the html page, downloading a zip folder containing only the relevant xml files.

Python makes it easy to do both. We can scrap websites with the [BeautifulSoup](http://www.pythonforbeginners.com/beautifulsoup/beautifulsoup-4-python) package. All we need to do is define an url, and we can use [urllib2](https://docs.python.org/2/howto/urllib2.html) for this. Then, the download can be performed using a list of file names produced parsing the html page, as BeautifulSoup needs to know which files we need. In the end, a zip folder will be created for each region on the html page, with each folder containing only the relevant xml files.

    from __future__ import division
    import urllib2
    from bs4 import BeautifulSoup
    import os 
    import zipfile
    
    #Set project folder
    myfolder = r'C:\Users\MyName\MyFolder' #Change as appropriate
    os.chdir(myfolder) #Get inside the project folder

    #Set the url of a website
    url = 'http://ratings.food.gov.uk/open-data/'
    f = urllib2.urlopen(url)
    mainpage = f.read()
    
    #Get the list of file names to download
    soup = BeautifulSoup(mainpage, 'html.parser')
    tablewrapper = soup.find(id='openDataStatic')
    regions = []
    filenames = {} #Dictionary with {Region:[list of file names]}
    with open('Regions_and_files.csv', 'w') as f:
        f.write("Region,City,File,Businesses"+'\n')
        for h2 in soup.find_all('h2')[6:]:
            temp = [] #Stores the links for each region
            region = h2.text.strip() #Get the text of each h2 without the white spaces
            regions.append(str(region))
            table = h2.next_sibling.next_sibling
            for tr in table.find_all('tr')[1:]: # Skip headers
                tds = tr.find_all('td')
                if len(tds)==0:
                    continue
                else:
                    a = tr.find_all('a')
                    link = str(a)[10:67]
                    span = tr.find_all('span')
                    businesses = int(str(span[3].text).replace(',', '')) #Number of businesses for each local authority
                    if "cy" not in link:   #Avoiding files in Welsh
                        temp.append(link)
                        f.write("%s,%s,%s,%s" % \
                                      (region,str(tds[0].text)[1:-1], link, businesses)+'\n')
            filenames[region] = temp
            
    #Parsing the html page and saving the xml files to the appropriate folder based on the region
    for key, value in filenames.iteritems():
        zipname = str(key)+".zip" #str(key) contains the name of the region
        with zipfile.ZipFile(zipname, "w") as code:
            for url in filenames[key]:
                f = urllib2.urlopen(url)
                data = f.read()
                xmlfilename = url.rsplit('/', 1)[-1]
                code.writestr(xmlfilename, data)
                
Once we have the zip folders, we need to extract all the files in a folder with the same name as the zip folder. Each file in the Scotland folder [has the hygiene descriptor under the *RatingValue* tag](http://ratings.food.gov.uk/OpenDataFiles/FHRS760en-GB.xml), whereas the files for the rest of the UK [have the hygiene score under the *Hygiene* tag](http://ratings.food.gov.uk/OpenDataFiles/FHRS250en-GB.xml). So, we need to loop through each file and get hold of these values. 


