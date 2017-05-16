#### **Where are the dirtiest restaurants and hotels in the United Kingdom? How many of them failed to achieve maximum food hygiene ratings?**
##### A Data Science project carried out with Jupyter and Python | May 11, 2017
---

<img src="/images/Ratings.png" width="370" height="210"> 


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

This section is about:
  1. Scraping the [Food Standards Agency html page](http://ratings.food.gov.uk/open-data/) to extract the file names to be downloaded;
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
                  
                
Once we have the zip folders, we need to extract all the files in a folder with the same name as the zip folder. A quick manipulation of the *Regions_and_files.csv* file will reveal how many businesses are subjected to the standards in each part of the UK:


    #Create dataframe with recap data and bar plot
    import pandas
    import matplotlib.pyplot as plt
    
    df = pandas.read_csv("Regions_and_files.csv")
    df.groupby('Region')['Businesses'].sum()
    df2 = pandas.Series.to_frame(df.groupby('Region')['Businesses'].sum())
    ax = df2.plot(kind='bar',title='Number of Businesses per Region in the UK, 2017',zorder=3)
    ax.grid(zorder=0,linestyle='--',linewidth=0.5) 
    plt.xticks(fontsize=8)  
    plt.tight_layout()
    
    
<img src="/images/Barplot.png" width="600" height="450">

**Figure 1** 

Surprisingly, [Greater London](https://en.wikipedia.org/wiki/Greater_London) is not the one with the highest number of businesses. These are instead located in the [South East](https://en.wikipedia.org/wiki/South_East_England#Demographics) (67,401 different places scattered across cities such as Southampton, Oxford, Portsmouth, Canterbury, and many others). London comes next at 64,897. The least represented region is Northern Ireland, with 16,135 businesses.

Each file in the Scotland folder [has the hygiene descriptor under the *RatingValue* tag](http://ratings.food.gov.uk/OpenDataFiles/FHRS760en-GB.xml), whereas the files for the rest of the UK [have the hygiene score under the *Hygiene* tag](http://ratings.food.gov.uk/OpenDataFiles/FHRS250en-GB.xml). So, we need to loop through each file and get hold of these values. 


    import xml.etree.ElementTree as ET
    
    #Dictionary with ratings
    ratings = {} #UK except Scotland
    scotland = {} #Different rating system

    tot_businesses = 0 #Count the number of businesses in the UK except Scotland

    #Parsing the xml files in their individual directories
    path = r'C:\Users\MyName\MyFolder\Files' #Change as appropriate
    for root, dirs, files in os.walk(path,topdown=True):
        gb_vals = [] #Stores the ratings for each region (=root)

        #Check Scotland, as it has a different rating scheme
        if root=='C:\\Users\\MyName\\MyFolder\\Files\\Scotland': #Change as appropriate
            #Scottish ratings are categorical
            passed = 0
            passed_safe = 0
            improve = 0
            for file in files: #For each City/Area in Scotland
                if not file.endswith('.xml'): continue
                fullname = os.path.join(root, file)
                print fullname[65:]
                tree = ET.parse(fullname)
                treeroot = tree.getroot()    
                for each in treeroot.findall('.//EstablishmentDetail'): 
                    councilarea = each.find('.//LocalAuthorityName')
                    council = councilarea.text
                for each in treeroot.findall('.//EstablishmentDetail'):
                    rating = each.find('.//RatingValue')
                    #Skip the rare situations of exempt businesses or similar
                    if rating.text=='Exempt' or rating.text=='Awaiting publication' or rating.text=='Awaiting inspection' or rating is None:
                        continue
                    else:
                        if rating.text=='Pass and Eat Safe':
                            passed_safe+=1
                        if rating.text=='Pass':
                            passed+=1
                        if rating.text=='Improvement Required':
                            improve+=1
                perc1 = round(100*improve/(improve+passed_safe+passed),3) #Percentage of negative businesses
                scotland[council] = perc1 #Saving values in the dictionary             

        else: #All the other Regions
            for file in files: #For each City/Area in the Region
                count = 0 #Count number of non excellent businesses
                if not file.endswith('.xml'): continue
                fullname = os.path.join(root, file)
                print fullname[65:]
                tree = ET.parse(fullname)
                treeroot = tree.getroot()
                for each in treeroot.findall('.//Header'):
                    businesses = each.find('.//ItemCount')
                    tot_businesses = tot_businesses + int(businesses.text)
                for each in treeroot.findall('.//Scores'):
                    rating = each.find('.//Hygiene') 
                    if rating is None:
                        continue
                    if int(rating.text)>=20: #Percentage of negative ranks
                        count+=1
                perc2 = round(count/int(businesses.text),3)        
                gb_vals.append(perc2) #Get the rating value

            region_name = str(root[65:]) #Getting the name of the Region as a text string
            ratings[region_name] = gb_vals #Saving values in the dictionary           

    #Average ratings per region, UK except Scotland
    avg_gb = {k:round(100*sum(i)/len(i),3) for k,i in ratings.items() if len(i)>0}



### Plotting data

Now it's time to plot the data acquired from the xml files. The geographer's soul that's in me calls for them to be put on a nice map. Instead of using the Python package [Basemap](https://matplotlib.org/basemap/users/examples.html), I want to load and display some [shapefiles](https://en.wikipedia.org/wiki/Shapefile), because they allow me to select the appropriate administrative boundaries to show. Since this is a separate analysis, there are two dedicated shapefiles: <a href="/Files/Scotland.zip" target="_blank">one for Scotland</a> and <a href="/Files/UK_except_Scotland.zip" target="_blank">one for the rest of the UK</a>.

Drawing inspiration [from here](http://stackoverflow.com/questions/25505674/python-matplotlib-add-colorbar?answertab=active#tab-top), we can now plot a [cloropleth map](https://en.wikipedia.org/wiki/Choropleth_map) showing the areas of the UK and Scotland where the percentage of poor food hygiene ratings is highest. The definition of "poor" in both cases refers to non-complaint businesses:

1. For Scotland, we can use the dedicated "Improvement Required" rating tag and count their frequency for each council area. This will give us the percentage of non-compliant businesses in each council area: 

<div>
$$
\begin{align*}
  & \ Non-compliant_{Scotland}  = 100* \frac{\sum_{} Improvement Required}{\sum_{} Businesses}
\end{align*}
$$
</div>

2. For the rest of the UK, we can assume that non-compliant businesses are given a score of at least 20. We count such occurrences for and average their number to get the percentage of non-compliant businesses for each region:

<div>
$$
\begin{align*}
  & \ Non-compliant_{UK}  = 100* \frac{\sum_{} scores ≥20}{\sum_{} Businesses}
\end{align*}
$$
</div>

    #Plotting
    import cartopy.crs as ccrs
    import matplotlib.pyplot as plt
    import matplotlib as mpl
    import cartopy.io.shapereader as shpreader

    #Scotland
    #Set projection and colormap
    fig, ax = plt.subplots(subplot_kw={'projection': ccrs.OSGB()})
    norm = mpl.colors.Normalize(vmin=min(scotland.values()), vmax=max(scotland.values()))
    cmap = plt.cm.RdYlBu_r

    #Loop over the shapes
    for n, country in enumerate(shpreader.Reader(r'C:\Users\MyName\MyFolder\Scotland\Scotland_with_council_areas.shp').records()):

        ax.add_geometries(country.geometry, ccrs.OSGB(),
                          facecolor=cmap(norm(country.attributes['Hygiene'])),
                          label=country.attributes['geo_label'],edgecolor='black',linewidth=0.3)

    #Image polishing
    ax.set_title('Non-compliant businesses in Scotland, %')
    ax.set_extent([5513.0004, 470323.0000, 530252.8000, 1220301.5006], ccrs.OSGB())
    plt.xlabel("Easting [m]")
    plt.ylabel("Northing [m]")

    #Colorbar
    cax = fig.add_axes([0.8, 0.15, 0.03, 0.7]) #Position
    cb = mpl.colorbar.ColorbarBase(cax, cmap=cmap, norm=norm, spacing='proportional')
    cb.set_label('%')

<img src="/images/Non-compliant_Scotland.png" width="600" height="450">

**Figure 2**

In Scotland, it looks like hat the worst place to go out for lunch or dinner is Aberdeen, where 16.52% of the 1,764 targeted businesses are non-complaint as of May 2017. In Eastern Scotland there seems to be a cluster of bad practice: in fact, Aberdeenshire comes right behind with 13.81% non-compliant businesses, followed by the Angus council area (12.25%). Based on this analysis, this is an area where Food Standards Scotland should probably reinforce its standards better. The parts of Scotland closer to England offer a much better landscape in terms of food standards, with Dumfries and Galloway showing a 7.72% of non-compliant businesses - a 53% reduction compared to the city of Aberdeen.

    #Rest of the UK
    #Set projection and colormap
    fig, ax = plt.subplots(subplot_kw={'projection': ccrs.OSGB()})
    norm = mpl.colors.Normalize(vmin=min(avg_gb.values()), vmax=max(avg_gb.values()))
    cmap = plt.cm.RdYlBu_r

    #Loop over the shapes
    for n, country in enumerate(shpreader.Reader(r'C:\Users\Francesco\Desktop\Data_Science_portfolio\20170511\UK_except_Scotland\UK_except_Scotland.shp').records()):

        ax.add_geometries(country.geometry, ccrs.OSGB(),
                          facecolor=cmap(norm(country.attributes['Hygiene'])),
                          label=country.attributes['NAME'],edgecolor='black',linewidth=0.3)

    #Image polishing
    ax.set_title('Non-compliant businesses in the UK, %')
    ax.set_extent([-69.1254, 655988.9963, 5333.6025, 657599.5034], ccrs.OSGB())
    plt.xlabel("Easting [m]")
    plt.ylabel("Northing [m]")

    #Colorbar
    cax = fig.add_axes([0.85, 0.15, 0.03, 0.7]) #Position
    cb = mpl.colorbar.ColorbarBase(cax, cmap=cmap, norm=norm, spacing='proportional')
    cb.set_label('%')

<img src="/images/Non-compliant_restUK.png" width="600" height="450">

**Figure 3**

In the rest of the United Kingdom and in Northern Ireland, the numbers are different because of the different rating scheme. However, it looks like that the overall non-compliance comes with very low numbers. Northern Ireland itself seems to be the safest place to be served food, where an average of 0.009% of businesses has an hygiene rating of at least 20. The second-best performing region is the North East, where the average non-compliant businesses are 0.108%. However, unlike in Scotland, no spatial pattern can be recognizable, and it is worth mentioning Greater London as the worst region. There, an average of 0.418% of all businesses are non-compliant with food hygiene regulations.

Of course this analysis is informative, but coarse. What if we zoom in on large cities? Again, due to the different rating systems, we need to run two separate analyses. Let's consider the largest cities in Scotland (Glasgow, Edinburgh, Aberdeen, Dundee, and Paisley), and those in the rest of the UK (Birmingham, Leeds, Sheffield, Bradford, Liverpool, Manchester, Cardiff, Belfast, and Newcastle upon Tyne). For Greater London, we already know that an average 0.418% of all businesses are non-complaint, so we can consider this value admissible. For the other cities, a quick look at the [source page](view-source:http://ratings.food.gov.uk/open-data/) of the Food Standards Agency website will allow us to identify the associated xml file:

    #Scotland
    sco={'Glasgow':'FHRS776en-GB.xml','Edinburgh':'FHRS773en-GB.xml','Aberdeen':'FHRS760en-GB.xml',
         'Dundee':'FHRS772en-GB.xml','Paisley':'FHRS788en-GB.xml'}

    #Rest of the UK
    rest_of_uk={'Birmingham':'FHRS402en-GB.xml','Leeds':'FHRS413en-GB.xml','Sheffield':'FHRS425en-GB.xml',
                'Bradford':'FHRS404en-GB.xml','Liverpool':'FHRS414en-GB.xml','Manchester':'FHRS415en-GB.xml',
                'Cardiff':'FHRS556en-GB.xml','Belfast':'FHRS807en-GB.xml','Newcastle':'FHRS416en-GB.xml'}

Now we can search the folder with all the xml files and compute the percentage of non-compliant businesses for each city:

    #Scotland
    cities_sco={}
    path = r'C:\Users\MyName\MyFolder\Files\Scotland' #Change as appropriate
    for root, dirs, files in os.walk(path,topdown=True):
        for city, xmlfile in sco.iteritems():
            for file in files:
                passed = 0
                passed_safe = 0
                improve = 0
                if file==city:
                    fullname = os.path.join(root, file)
                    tree=ET.parse(fullname)
                    treeroot = tree.getroot()
                    for each in treeroot.findall('.//EstablishmentDetail'):
                        rating = each.find('.//RatingValue')
                        #Skip the rare situations of exempt businesses or similar
                        if rating.text=='Exempt' or rating.text=='Awaiting publication' or rating.text=='Awaiting inspection' or rating is None:
                            continue
                        else:
                            if rating.text=='Pass and Eat Safe':
                                passed_safe+=1
                            if rating.text=='Pass':
                                passed+=1
                            if rating.text=='Improvement Required':
                                improve+=1
                    perc1 = round(100*improve/(improve+passed_safe+passed),3)
                    cities_sco[city]=perc1
                    
And we do the same for the rest of the UK:

    #Rest of the UK
    cities_gb={}
    path = r'C:\Users\MyName\MyFolder\Files' #Change as appropriate
    for root, dirs, files in os.walk(path,topdown=True):
        for city, xmlfile in rest_of_uk.iteritems():
            for file in files:
                count=0
                if file==xmlfile:
                    fullname = os.path.join(root, file)
                    tree=ET.parse(fullname)
                    treeroot = tree.getroot()
                    for each in treeroot.findall('.//Header'):
                        businesses = each.find('.//ItemCount')
                        tot_businesses = tot_businesses + int(businesses.text)
                    for each in treeroot.findall('.//Scores'):
                        rating = each.find('.//Hygiene')
                        #Skip the rare situations of exempt businesses or similar
                        if rating is None:
                            continue
                        if int(rating.text)>=20:
                                count+=1
                    perc2 = round(100*count/int(businesses.text),3)
                    cities_gb[city]=perc2
         
Now we turn the two dictionaries *cities_sco* and *cities_gb* into dataframes and plot them:

    import pandas as pd
    cities_sco_df=pd.DataFrame(cities_sco.items(), columns=['City', 'Non-compliant businesses (%)'])
    cities_gb_df=pd.DataFrame(cities_gb.items(), columns=['City', 'Non-compliant businesses (%)'])

