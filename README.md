# Data analysis project - Animal Bites :dog: :cat: :rabbit: <br>
### By [Milene Amâncio Alves Eigenheer](https://www.linkedin.com/in/mileneaae)<br>

## :bookmark_tabs: Description<br>
This is a brief report about the animal bites data from the perspective of the Louisville Metro Department of Public Health and Wellness. <br>
The file <b>Milene_Eigenheer_Animal_bites_Report.pdf</b> is a short report where I identified interesting insights, explained it, and discussed areas of further research.<br>

<br>


### :computer: R packages used
<br>
<ul>
<li> :heavy_check_mark: [data.table](https://cran.r-project.org/web/packages/data.table/data.table.pdf)</li>
<li> :heavy_check_mark: [Tydiverse](https://www.tidyverse.org/)</li>
<li> :heavy_check_mark: [Lubridate](https://cran.r-project.org/web/packages/lubridate/lubridate.pdf)</li>
<li> :heavy_check_mark: [ggthemes](https://cran.r-project.org/web/packages/ggthemes/ggthemes.pdf)</li>
<li> :heavy_check_mark: [RColorBrewer](https://cran.r-project.org/web/packages/RColorBrewer/RColorBrewer.pdf)</li>
</ul>
<br>

<br>

### :nerd_face: Spatial analysis<br>
All the spatial analysis were conducted at QGis 3.16.5 (2021). The steps were:
<ul>
<li> :heavy_check_mark: Importing vector of the limits of the counties in Kentucky</li>
<li> :heavy_check_mark: Subsetting it to Louisville and save as another shapefile</li>
<li> :heavy_check_mark: Importing the new file (Louisville limits)</li>
<li> :heavy_check_mark: Importing the .csv file about intensity of bites per location (“spatial2.csv”) and save it as shapefile</li>
<li> :heavy_check_mark: Intersecting the points with the limits of the city to get only points inside the city</li>
<li> :heavy_check_mark: Plotting the new points and classify it by size according to the number of incidents by location</li>
<li> :heavy_check_mark: Map layout
</ul>
<br>

### Feel free to use and share it :green_heart: