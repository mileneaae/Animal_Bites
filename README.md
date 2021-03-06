# Data analysis project - Animal Bites :dog: :cat: :rabbit: <br>
### By [Milene Amâncio Alves Eigenheer](https://www.linkedin.com/in/mileneaae)<br>

## :bookmark_tabs: Description<br>
This is a brief report about [animal bites data](https://www.kaggle.com/rtatman/animal-bites) from the Louisville Metro Department of Public Health and Wellness. The file <b>[Milene_Eigenheer_Animal_bites_Report.pdf](https://github.com/mileneaae/Animal_Bites/blob/main/Milene_Eigenheer_Animal_bites_Report.pdf)</b> is a short report where I identified interesting insights, explained it, and discussed areas of further research.<br>


### :computer: R packages used<br>
:heavy_check_mark: [data.table](https://cran.r-project.org/web/packages/data.table/data.table.pdf)<br>
:heavy_check_mark: [Tydiverse](https://www.tidyverse.org/)<br>
:heavy_check_mark: [Lubridate](https://cran.r-project.org/web/packages/lubridate/lubridate.pdf)<br>
:heavy_check_mark: [ggthemes](https://cran.r-project.org/web/packages/ggthemes/ggthemes.pdf)<br>
:heavy_check_mark: [RColorBrewer](https://cran.r-project.org/web/packages/RColorBrewer/RColorBrewer.pdf)<br>


### :bulb: Spatial analysis<br>
All the spatial analysis were conducted at QGis 3.16.5 (2021). The steps were:<br>
:heavy_check_mark: Importing vector of the limits of the counties in Kentucky<br>
:heavy_check_mark: Subsetting it to Louisville and save as another shapefile<br>
:heavy_check_mark: Importing the new file (Louisville limits)<br>
:heavy_check_mark: Importing the .csv file about intensity of bites per location (“spatial2.csv”) and save it as shapefile<br>
:heavy_check_mark: Intersecting the points with the limits of the city to get only points inside the city<br>
:heavy_check_mark: Plotting the new points and classify it by size according to the number of incidents by location<br>
:heavy_check_mark: Map layout<br>

### Feel free to use and share it :green_heart: