# Data analyzes project - Animal Bites <br>
### By [Milene Amâncio Alves Eigenheer](https://www.linkedin.com/in/mileneaae)<br>

## Description<br>
This is a brief report about the animal bites data from the perspective of the Louisville Metro Department of Public Health and Wellness. In the <b>Milene_Eigenheer_Animal_bites_Report.pdf</b> you can find a short report where I identified interesting insights, explained it, and discussed areas of further research.<br>
<br>

<br>

### R packages used<br>
<ul>
<li> [data.table](https://cran.r-project.org/web/packages/data.table/data.table.pdf)</li>
<li> [Tydiverse](https://www.tidyverse.org/)</li>
<li> [Lubridate](https://cran.r-project.org/web/packages/lubridate/lubridate.pdf)</li>
<li> [ggthemes](https://cran.r-project.org/web/packages/ggthemes/ggthemes.pdf)</li>
<li> [RColorBrewer](https://cran.r-project.org/web/packages/RColorBrewer/RColorBrewer.pdf)</li>
</ul><br>

--------------------------------------<br>
### Spatial analyses<br>
All the spatial analyses were conducted at QGis 3.16.5 (2021). The steps were:
<ul>
<li> Importing vector of the limits of the counties in Kentucky</li>
<li> Subsetting it to Louisville and save as another shapefile</li>
<li> Importing the new file (Louisville limits)</li>
<li> Importing the .csv file about intensity of bites per location (“spatial2.csv”) and save it as shapefile</li>
<li> Intersecting the points with the limits of the city to get only points inside the city</li>
<li> Plotting the new points and classify it by size according to the number of incidents by location</li>
<li> Map layout
</ul><br>

----------------------------------<br>
### Feel free to use and share it :green_heart: