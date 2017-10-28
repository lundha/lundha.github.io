#### **Smart Cities: the use of sensors to help tackle the Urban Heat Island effect**
##### A Data-Science-for-Smart-Cities project carried out with Python | October 24, 2017
---


#### Problem Statement: the Urban Heat Island Effect (UHI)

<img src="/images/Atlanta_thermal.jpg" width="380" height="280"> 


The image above was collected in May 1997 by NASA ([source](https://commons.wikimedia.org/w/index.php?curid=6026139)) 
to show the distribution of temperatures in downtown Atlanta. The majority of the white areas happen to be located on roofs, and 
a significant contribution is given by the streets (in red). This phenomenon, known as [Urban Heat Island effect](https://en.wikipedia.org/wiki/Urban_heat_island), is all but new, having first been observed in the early 19th Century. It indicates an increase in temperatures around the city core with respect to the rural areas surrounding it, and it is caused by the introduction of dark surfaces - [asphalt](https://en.wikipedia.org/wiki/Asphalt), mainly - that absorb the incident solar radiation. This leads to an increase in temperature on the surface of dark objects, which in turn produces a spike in the thermal radiation they emit back into the air. 
Rooftops and roads are particularly good heat absorpters, due to the presence of [asphaltene](https://en.wikipedia.org/wiki/Asphaltene). 

<img src="/images/asphalt.jpg" width="300" height="240"> 

A look into the typical values of [thermal conductivity](https://en.wikipedia.org/wiki/Thermal_conductivity) quantifies the extent to which concrete and asphalt are serious contributors, their contribution being amplified by the dense spatial presence of roads and rooftops.

<table class="tg">
  <tr>
    <th class="tg-baqh" colspan="2">Thermal Conductivity [W/(m K)]</th>
  </tr>
  <tr>
    <th class="tg-baqh">Material</th>
    <th class="tg-baqh">Value</th>
  </tr>
  <tr>
    <td class="tg-baqh">Asphalt</td>
    <td class="tg-baqh">0.75</td>
  </tr>
  <tr>
    <td class="tg-baqh">Bricks</td>
    <td class="tg-baqh">0.47</td>
  </tr>
  <tr>
    <td class="tg-baqh">Concrete, medium</td>
    <td class="tg-baqh">0.4 - 0.7</td>
  </tr>
  <tr>
    <td class="tg-baqh">Concrete, dense</td>
    <td class="tg-baqh">1.0 - 1.8</td>
  </tr>
  <tr>
    <td class="tg-baqh">Soil with organic matter</td>
    <td class="tg-baqh">0.15 - 2.0</td>
  </tr>
  <tr>
    <td class="tg-baqh">Wood</td>
    <td class="tg-baqh">0.07 - 0.17</td>
  </tr>
</table>


As pointed out by the [NASA Earth Observatory](https://earthobservatory.nasa.gov/Features/UrbanRain/urbanrain2.php), the UHI effect triggers a negative climatic effect. Urban landscapes may experience temperatures between 3.5°C and 4.5°C higher than the surrounding landscapes, resulting in warmer and unstable layers of air that can hold more moisture. [Research has demonstrated that this increases the frequency of more extreme rainfall events in the summer](https://www.atmos-chem-phys.net/17/5439/2017/acp-17-5439-2017.pdf) in the Northern Hemisphere. As a result, the risk of floods may increase.

#### The Los Angeles solution

<img src="/images/LA.jpg" width="380" height="280"> 

The Mayor of Los Angeles has recently committed to painting some of the city streets in a light shade of grey (pictured above, courtesy of the [Bureau of Street Services](https://bss.lacity.org)) in an attempt to cut the UHI effect. As reported by the [press](http://www.digitaljournal.com/news/environment/los-angeles-paining-streets-white-to-combat-urban-warming/article/502092), the ambition is to decrease urban temperatures by as much as 1.67°C over the next two decades. The plan is to use a specific coating that, in a pilot experiment, has led to a decrease in road surface temperature of 4.5°C. Apparently, this solution could potentially offset the projected increase reported by NASA and exceed the ambition of the Bureau of Street Services.
However, the financial side of things is not equally promising, as the cost of the specific coating to be used is estimated to be around [$40,000 per mile](http://www.mercurynews.com/2017/05/22/cool-pavement-to-cut-urban-street-heat-gets-first-california-tryout-in-canoga-park/). Moreover, the Bureau of Street Services is responsile for maintaining [6,500 centerline miles of streets and 800 centerline miles of alleys](https://www.google.co.uk/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwj-3M7NrpPXAhXQaVAKHYBgCI4QFggqMAE&url=http%3A%2F%2Fbss.lacity.org%2Fstate_streets%2Fstateofthestreets.htm&usg=AOvVaw3aIaOEQBr27krhVrplNCSV), accounting for the largest street system in the United States at the municipal level. Having to allocate $260 million to coat the streets (while neglecting the alleys) is probably haunting the dreams of city officials. The answer may lie in a **decision-making tool to identify areas to prioritise, allowing to create a strategic investment plan**.

#### How Data from Smart Infrastructures can foster Smart Decisions

<img src="/images/sensors.jpg" width="300" height="280"> 

Manufacturers and suppliers of sensors have already started supplying devices ([picture above](https://www.campbellsci.eu/road-surface-sensors)) that can measure **road surface temperatures** at very short intervals of time. These datasets are an ideal component of open datasets that cities could make available.
This project was carried out using the open data made available by the [City of Seattle Open Data Portal](https://data.seattle.gov/Transportation/Road-Weather-Information-Stations/egc4-d24i), which supplies data for 10 bridges and road intersections (pictured below) at intervals of 15 minutes. Ideally, sensors should be placed at each intersection and on each bridge. However, the cost of installing dense networks and the lack of awareness by institutions are likely the main reasons why extended datasets are very difficult to find.

<img src="/images/Sensor_locations.png" width="610" height="360"> 
