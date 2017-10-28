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

A brief look into the typical values of [thermal conductivity](https://en.wikipedia.org/wiki/Thermal_conductivity) reveals why concrete and asphalt are serious contributors, this being amplified by the dense spatial presence of roads and rooftops.

<table class="tg">
  <tr>
    <th class="tg-baqh" colspan="2">Thermal Conductivity [W/(m K)]</th>
  </tr>
  <tr>
    <td class="tg-baqh">Material</td>
    <td class="tg-baqh">Value</td>
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




