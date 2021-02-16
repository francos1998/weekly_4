---
title: 'Weekly Exercises #4'
author: "Franco Salinas"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
```

```
## ── Attaching packages ────────────────────────────── tidyverse 1.3.0 ──
```

```
## ✓ ggplot2 3.3.2     ✓ purrr   0.3.4
## ✓ tibble  3.0.3     ✓ dplyr   1.0.2
## ✓ tidyr   1.1.2     ✓ stringr 1.4.0
## ✓ readr   1.3.1     ✓ forcats 0.5.0
```

```
## ── Conflicts ───────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(lubridate)     # for date manipulation
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```r
library(openintro)     # for the abbr2state() function
```

```
## Loading required package: airports
```

```
## Loading required package: cherryblossom
```

```
## Loading required package: usdata
```

```r
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
```

```
## 
## Attaching package: 'maps'
```

```
## The following object is masked from 'package:purrr':
## 
##     map
```

```r
library(ggmap)         # for mapping points on maps
```

```
## Google's Terms of Service: https://cloud.google.com/maps-platform/terms/.
```

```
## Please cite ggmap if you use it! See citation("ggmap") for details.
```

```r
library(gplots)        # for col2hex() function
```

```
## 
## Attaching package: 'gplots'
```

```
## The following object is masked from 'package:stats':
## 
##     lowess
```

```r
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
```

```
## Linking to GEOS 3.8.1, GDAL 3.1.1, PROJ 6.3.1
```

```r
library(leaflet)       # for highly customizable mapping
library(carData)       # for Minneapolis police stops data
library(ggthemes)      # for more themes (including theme_map())
theme_set(theme_minimal())
```


```r
# Starbucks locations
Starbucks <- read_csv("https://www.macalester.edu/~ajohns24/Data/Starbucks.csv")
```

```
## Parsed with column specification:
## cols(
##   Brand = col_character(),
##   `Store Number` = col_character(),
##   `Store Name` = col_character(),
##   `Ownership Type` = col_character(),
##   `Street Address` = col_character(),
##   City = col_character(),
##   `State/Province` = col_character(),
##   Country = col_character(),
##   Postcode = col_character(),
##   `Phone Number` = col_character(),
##   Timezone = col_character(),
##   Longitude = col_double(),
##   Latitude = col_double()
## )
```

```r
starbucks_us_by_state <- Starbucks %>% 
  filter(Country == "US") %>% 
  count(`State/Province`) %>% 
  mutate(state_name = str_to_lower(abbr2state(`State/Province`))) 

# Lisa's favorite St. Paul places - example for you to create your own data
favorite_stp_by_lisa <- tibble(
  place = c("Home", "Macalester College", "Adams Spanish Immersion", 
            "Spirit Gymnastics", "Bama & Bapa", "Now Bikes",
            "Dance Spectrum", "Pizza Luce", "Brunson's"),
  long = c(-93.1405743, -93.1712321, -93.1451796, 
           -93.1650563, -93.1542883, -93.1696608, 
           -93.1393172, -93.1524256, -93.0753863),
  lat = c(44.950576, 44.9378965, 44.9237914,
          44.9654609, 44.9295072, 44.9436813, 
          44.9399922, 44.9468848, 44.9700727)
  )

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

```
## Parsed with column specification:
## cols(
##   date = col_date(format = ""),
##   state = col_character(),
##   fips = col_character(),
##   cases = col_double(),
##   deaths = col_double()
## )
```

## Put your homework on GitHub!

If you were not able to get set up on GitHub last week, go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) and get set up first. Then, do the following (if you get stuck on a step, don't worry, I will help! You can always get started on the homework and we can figure out the GitHub piece later):

* Create a repository on GitHub, giving it a nice name so you know it is for the 4th weekly exercise assignment (follow the instructions in the document/video).  
* Copy the repo name so you can clone it to your computer. In R Studio, go to file --> New project --> Version control --> Git and follow the instructions from the document/video.  
* Download the code from this document and save it in the repository folder/project on your computer.  
* In R Studio, you should then see the .Rmd file in the upper right corner in the Git tab (along with the .Rproj file and probably .gitignore).  
* Check all the boxes of the files in the Git tab under Stage and choose commit.  
* In the commit window, write a commit message, something like "Initial upload" would be appropriate, and commit the files.  
* Either click the green up arrow in the commit window or close the commit window and click the green up arrow in the Git tab to push your changes to GitHub.  
* Refresh your GitHub page (online) and make sure the new documents have been pushed out.  
* Back in R Studio, knit the .Rmd file. When you do that, you should have two (as long as you didn't make any changes to the .Rmd file, in which case you might have three) files show up in the Git tab - an .html file and an .md file. The .md file is something we haven't seen before and is here because I included `keep_md: TRUE` in the YAML heading. The .md file is a markdown (NOT R Markdown) file that is an interim step to creating the html file. They are displayed fairly nicely in GitHub, so we want to keep it and look at it there. Click the boxes next to these two files, commit changes (remember to include a commit message), and push them (green up arrow).  
* As you work through your homework, save and commit often, push changes occasionally (maybe after you feel finished with an exercise?), and go check to see what the .md file looks like on GitHub.  
* If you have issues, let me know! This is new to many of you and may not be intuitive at first. But, I promise, you'll get the hang of it! 


## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.


## Warm-up exercises from tutorial

These exercises will reiterate what you learned in the "Mapping data with R" tutorial. If you haven't gone through the tutorial yet, you should do that first.

### Starbucks locations (`ggmap`)

  1. Add the `Starbucks` locations to a world map. Add an aesthetic to the world map that sets the color of the points according to the ownership type. What, if anything, can you deduce from this visualization?  


```r
world <- get_stamenmap(
  bbox = c(left = -180, bottom = -57, right = 179, top = 82.1),
  maptype = "terrain",
  zoom = 2
)
```

```
## Source : http://tile.stamen.com/terrain/2/0/0.png
```

```
## Source : http://tile.stamen.com/terrain/2/1/0.png
```

```
## Source : http://tile.stamen.com/terrain/2/2/0.png
```

```
## Source : http://tile.stamen.com/terrain/2/3/0.png
```

```
## Source : http://tile.stamen.com/terrain/2/0/1.png
```

```
## Source : http://tile.stamen.com/terrain/2/1/1.png
```

```
## Source : http://tile.stamen.com/terrain/2/2/1.png
```

```
## Source : http://tile.stamen.com/terrain/2/3/1.png
```

```
## Source : http://tile.stamen.com/terrain/2/0/2.png
```

```
## Source : http://tile.stamen.com/terrain/2/1/2.png
```

```
## Source : http://tile.stamen.com/terrain/2/2/2.png
```

```
## Source : http://tile.stamen.com/terrain/2/3/2.png
```

```r
ggmap(world)+
  geom_point(data = Starbucks,
             aes(x = Longitude,
                 y = Latitude, 
                 color = `Ownership Type`),
             alpha = .3,
             size = .1)+
  theme_map()+
  theme(legend.background = element_blank())
```

```
## Warning: Removed 1 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

>I can deduce that most Starbucks are company owned or Licensed. I also can deduce that joint venture Starbucks are located only in Europe and Asia. 


  2. Construct a new map of Starbucks locations in the Twin Cities metro area (approximately the 5 county metro area).  

```r
twins <- get_stamenmap(
  bbox = c(left = -93.80, bottom = 44.6, right = -92.3, top = 45.2),
  maptype = "terrain",
  zoom = 10
)
```

```
## Source : http://tile.stamen.com/terrain/10/245/367.png
```

```
## Source : http://tile.stamen.com/terrain/10/246/367.png
```

```
## Source : http://tile.stamen.com/terrain/10/247/367.png
```

```
## Source : http://tile.stamen.com/terrain/10/248/367.png
```

```
## Source : http://tile.stamen.com/terrain/10/249/367.png
```

```
## Source : http://tile.stamen.com/terrain/10/245/368.png
```

```
## Source : http://tile.stamen.com/terrain/10/246/368.png
```

```
## Source : http://tile.stamen.com/terrain/10/247/368.png
```

```
## Source : http://tile.stamen.com/terrain/10/248/368.png
```

```
## Source : http://tile.stamen.com/terrain/10/249/368.png
```

```
## Source : http://tile.stamen.com/terrain/10/245/369.png
```

```
## Source : http://tile.stamen.com/terrain/10/246/369.png
```

```
## Source : http://tile.stamen.com/terrain/10/247/369.png
```

```
## Source : http://tile.stamen.com/terrain/10/248/369.png
```

```
## Source : http://tile.stamen.com/terrain/10/249/369.png
```

```r
ggmap(twins)+
  geom_point(data = Starbucks,
             aes(x = Longitude,
                 y = Latitude, 
                 color = `Ownership Type`),
             alpha = .8,
             size = .6)+
  theme_map()+
  theme(legend.background = element_blank())
```

```
## Warning: Removed 25455 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

  3. In the Twin Cities plot, play with the zoom number. What does it do?  (just describe what it does - don't actually include more than one map).  
  
>It changes the quality of the map's graphics.

  4. Try a couple different map types (see `get_stamenmap()` in help and look at `maptype`). Include a map with one of the other map types.  


```r
twins <- get_stamenmap(
  bbox = c(left = -93.80, bottom = 44.6, right = -92.3, top = 45.2),
  maptype = "toner-background",
  zoom = 10
)
```

```
## Source : http://tile.stamen.com/toner-background/10/245/367.png
```

```
## Source : http://tile.stamen.com/toner-background/10/246/367.png
```

```
## Source : http://tile.stamen.com/toner-background/10/247/367.png
```

```
## Source : http://tile.stamen.com/toner-background/10/248/367.png
```

```
## Source : http://tile.stamen.com/toner-background/10/249/367.png
```

```
## Source : http://tile.stamen.com/toner-background/10/245/368.png
```

```
## Source : http://tile.stamen.com/toner-background/10/246/368.png
```

```
## Source : http://tile.stamen.com/toner-background/10/247/368.png
```

```
## Source : http://tile.stamen.com/toner-background/10/248/368.png
```

```
## Source : http://tile.stamen.com/toner-background/10/249/368.png
```

```
## Source : http://tile.stamen.com/toner-background/10/245/369.png
```

```
## Source : http://tile.stamen.com/toner-background/10/246/369.png
```

```
## Source : http://tile.stamen.com/toner-background/10/247/369.png
```

```
## Source : http://tile.stamen.com/toner-background/10/248/369.png
```

```
## Source : http://tile.stamen.com/toner-background/10/249/369.png
```

```r
ggmap(twins)+
  geom_point(data = Starbucks,
             aes(x = Longitude,
                 y = Latitude, 
                 color = `Ownership Type`),
             alpha = .8,
             size = .6)+
  theme_map()+
  theme(legend.background = element_blank())
```

```
## Warning: Removed 25455 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

  5. Add a point to the map that indicates Macalester College and label it appropriately. There are many ways you can do think, but I think it's easiest with the `annotate()` function (see `ggplot2` cheatsheet).


```r
twins <- get_stamenmap(
  bbox = c(left = -93.80, bottom = 44.6, right = -92.3, top = 45.2),
  maptype = "toner-background",
  zoom = 10
)
ggmap(twins)+
  geom_point(data = Starbucks,
             aes(x = Longitude,
                 y = Latitude, 
                 color = `Ownership Type`),
             alpha = .8,
             size = .6)+
  theme_map()+
  theme(legend.background = element_blank())+
  annotate(geom = "point", 
           x = -93.17216,
           y = 44.94085,
           xmin = -93.17216, 
           xend = -93.16924, 
           ymin = 44.93976, 
           yend = 44.94085, 
           color = "blue",
           size = 4)+
  annotate(geom = "text",
           x = -93.17216,
           y = 44.899,
           xmin = -93.17216,
           xmax = -93.16924,
           ymin = 44.93976,
           ymax = 44.94085,
           label = "Macalester College",
           color = "Blue")
```

```
## Warning: Ignoring unknown aesthetics: xmin, xend, ymin, yend
```

```
## Warning: Ignoring unknown aesthetics: xmin, xmax, ymin, ymax
```

```
## Warning: Removed 25455 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

### Choropleth maps with Starbucks data (`geom_map()`)

The example I showed in the tutorial did not account for population of each state in the map. In the code below, a new variable is created, `starbucks_per_10000`, that gives the number of Starbucks per 10,000 people. It is in the `starbucks_with_2018_pop_est` dataset.


```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))
```

```
## Parsed with column specification:
## cols(
##   state = col_character(),
##   est_pop_2018 = col_double()
## )
```

```r
starbucks_with_2018_pop_est <-
  starbucks_us_by_state %>% 
  left_join(census_pop_est_2018,
            by = c("state_name" = "state")) %>% 
  mutate(starbucks_per_10000 = (n/est_pop_2018)*10000)
```

  6. **`dplyr` review**: Look through the code above and describe what each line of code does.
  

```r
#Names a data set
#Read the data from a link
#separate de observations into the dot and the name of county as variables
#Take de dot variable away
#change the format of the observation names so that they are lower case
#name the new data frame
#Join the Starbucks by state data set with the census population data, by matching the Starbucks observations by state name with the population data
#Create a new variable that contains the number of Starbucks per 
#10 000 people in each state. 
```

  7. Create a choropleth map that shows the number of Starbucks per 10,000 people on a map of the US. Use a new fill color, add points for all Starbucks in the US (except Hawaii and Alaska), add an informative title for the plot, and include a caption that says who created the plot (you!). Make a conclusion about what you observe.
  

```r
states_map <- map_data("state")
starbucks_with_2018_pop_est %>% 
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = state_name,
               fill = starbucks_per_10000)) +
  expand_limits(x = states_map$long, y = states_map$lat) + 
  theme_map()+
  labs(title = "Starbucks per 10000 people",
       fill = "",
       caption = "Franco Salinas")+
  theme(legend.background = element_blank())
```

![](04_exercises_files/figure-html/unnamed-chunk-7-1.png)<!-- -->
  
>I observe that the west has a higher ratio than the east. 


### A few of your favorite things (`leaflet`)

  8. In this exercise, you are going to create a single map of some of your favorite places! The end result will be one map that satisfies the criteria below. 

  * Create a data set using the `tibble()` function that has 10-15 rows of your favorite places. The columns will be the name of the location, the latitude, the longitude, and a column that indicates if it is in your top 3 favorite locations or not. For an example of how to use `tibble()`, look at the `favorite_stp_by_lisa` I created in the data R code chunk at the beginning.  

  * Create a `leaflet` map that uses circles to indicate your favorite places. Label them with the name of the place. Choose the base map you like best. Color your 3 favorite places differently than the ones that are not in your top 3 (HINT: `colorFactor()`). Add a legend that explains what the colors mean.  
  
  * Connect all your locations together with a line in a meaningful way (you may need to order them differently in the original data).  
  
  * If there are other variables you want to add that could enhance your plot, do that now.  
  

```r
favorite_stp_by_franco <- tibble(
  place = c("Home",
            "Macalester College",
            "Hola Arepa", 
            "Vertical Endeavors",
            "Moto-i",
            "My friend's place",
            "Simplicitea",
            "River",
            "Shish",
            "IceHouse"),
  long = c(-93.16661, 
           -93.1712321, 
           -93.27594, 
           -93.27703, 
           -93.28694,
           -93.16484,
           -93.16820,
           -93.19679,
           -93.16917, 
           -93.27661),
  lat = c(44.94131, 
          44.9378965, 
          44.93988,
          44.95669,
          44.94922,
          44.94331, 
          44.93368,
          44.94208,
          44.94048, 
          44.95690),
  top3 = c("no","no","yes","yes","no","no","yes","no","no","no")
  )
```


```r
pal <- colorFactor(c("green","blue"), 
                     domain = favorite_stp_by_franco$top3)
pal(favorite_stp_by_franco$top3)
```

```
##  [1] "#00FF00" "#00FF00" "#0000FF" "#0000FF" "#00FF00" "#00FF00" "#0000FF"
##  [8] "#00FF00" "#00FF00" "#00FF00"
```

```r
leaflet(data = favorite_stp_by_franco) %>% #base plot
  addProviderTiles(providers$CartoDB.Voyager) %>% 
  addCircles(color = ~pal(top3),
             opacity = 1, 
             label = ~place) %>% 
  addLegend(position = "topright",
            pal = pal,
            opacity = 1,
            values = ~top3) %>% 
  addPolylines(lng = ~long, 
               lat = ~lat, 
               color = col2hex("darkred"))
```

```
## Assuming "long" and "lat" are longitude and latitude, respectively
```

<!--html_preserve--><div id="htmlwidget-67426b81d2b566152c6a" style="width:672px;height:480px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-67426b81d2b566152c6a">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addProviderTiles","args":["CartoDB.Voyager",null,null,{"errorTileUrl":"","noWrap":false,"detectRetina":false}]},{"method":"addCircles","args":[[44.94131,44.9378965,44.93988,44.95669,44.94922,44.94331,44.93368,44.94208,44.94048,44.9569],[-93.16661,-93.1712321,-93.27594,-93.27703,-93.28694,-93.16484,-93.1682,-93.19679,-93.16917,-93.27661],10,null,null,{"interactive":true,"className":"","stroke":true,"color":["#00FF00","#00FF00","#0000FF","#0000FF","#00FF00","#00FF00","#0000FF","#00FF00","#00FF00","#00FF00"],"weight":5,"opacity":1,"fill":true,"fillColor":["#00FF00","#00FF00","#0000FF","#0000FF","#00FF00","#00FF00","#0000FF","#00FF00","#00FF00","#00FF00"],"fillOpacity":0.2},null,null,["Home","Macalester College","Hola Arepa","Vertical Endeavors","Moto-i","My friend's place","Simplicitea","River","Shish","IceHouse"],{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null,null]},{"method":"addLegend","args":[{"colors":["#00FF00","#0000FF"],"labels":["no","yes"],"na_color":null,"na_label":"NA","opacity":1,"position":"topright","type":"factor","title":"top3","extra":null,"layerId":null,"className":"info legend","group":null}]},{"method":"addPolylines","args":[[[[{"lng":[-93.16661,-93.1712321,-93.27594,-93.27703,-93.28694,-93.16484,-93.1682,-93.19679,-93.16917,-93.27661],"lat":[44.94131,44.9378965,44.93988,44.95669,44.94922,44.94331,44.93368,44.94208,44.94048,44.9569]}]]],null,null,{"interactive":true,"className":"","stroke":true,"color":"#8B0000","weight":5,"opacity":0.5,"fill":false,"fillColor":"#8B0000","fillOpacity":0.2,"smoothFactor":1,"noClip":false},null,null,null,{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]}],"limits":{"lat":[44.93368,44.9569],"lng":[-93.28694,-93.16484]}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

  
## Revisiting old datasets

This section will revisit some datasets we have used previously and bring in a mapping component. 

### Bicycle-Use Patterns

The data come from Washington, DC and cover the last quarter of 2014.

Two data tables are available:

- `Trips` contains records of individual rentals
- `Stations` gives the locations of the bike rental stations

Here is the code to read in the data. We do this a little differently than usualy, which is why it is included here rather than at the top of this file. To avoid repeatedly re-reading the files, start the data import chunk with `{r cache = TRUE}` rather than the usual `{r}`. This code reads in the large dataset right away.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

```
## Parsed with column specification:
## cols(
##   name = col_character(),
##   lat = col_double(),
##   long = col_double(),
##   nbBikes = col_double(),
##   nbEmptyDocks = col_double()
## )
```

  9. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. This time, plot the points on top of a map. Use any of the mapping tools you'd like.
  

```r
departures_station <- 
  Trips %>% 
  left_join(Stations, by = c("sstation" = "name")) %>% 
  group_by(lat, long) %>% 
  summarize(freq_stations = n())
```

```
## `summarise()` regrouping output by 'lat' (override with `.groups` argument)
```

```r
departures_station
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["lat"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["long"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["freq_stations"],"name":[3],"type":["int"],"align":["right"]}],"data":[{"1":"38.80111","2":"-77.06895","3":"186"},{"1":"38.80268","2":"-77.06356","3":"673"},{"1":"38.80312","2":"-77.04036","3":"427"},{"1":"38.80438","2":"-77.06087","3":"192"},{"1":"38.80472","2":"-77.04336","3":"742"},{"1":"38.80532","2":"-77.04988","3":"669"},{"1":"38.80565","2":"-77.05293","3":"363"},{"1":"38.80577","2":"-77.06072","3":"1423"},{"1":"38.81074","2":"-77.04466","3":"527"},{"1":"38.81146","2":"-77.05028","3":"236"},{"1":"38.81458","2":"-77.05281","3":"1629"},{"1":"38.82006","2":"-77.05762","3":"467"},{"1":"38.82093","2":"-77.05310","3":"313"},{"1":"38.82175","2":"-77.04749","3":"362"},{"1":"38.82595","2":"-77.05854","3":"447"},{"1":"38.83308","2":"-77.05982","3":"277"},{"1":"38.83411","2":"-77.08732","3":"104"},{"1":"38.83521","2":"-77.09430","3":"11"},{"1":"38.83767","2":"-77.09482","3":"64"},{"1":"38.83991","2":"-77.08708","3":"67"},{"1":"38.84232","2":"-77.08956","3":"359"},{"1":"38.84260","2":"-77.05020","3":"667"},{"1":"38.84322","2":"-76.99939","3":"52"},{"1":"38.84401","2":"-77.05054","3":"380"},{"1":"38.84440","2":"-77.08593","3":"99"},{"1":"38.84471","2":"-76.98782","3":"85"},{"1":"38.84622","2":"-77.06928","3":"143"},{"1":"38.84798","2":"-77.07510","3":"158"},{"1":"38.84844","2":"-77.05152","3":"1227"},{"1":"38.84845","2":"-77.08492","3":"59"},{"1":"38.85069","2":"-77.05152","3":"324"},{"1":"38.85225","2":"-77.10502","3":"86"},{"1":"38.85330","2":"-77.04980","3":"971"},{"1":"38.85469","2":"-77.10055","3":"14"},{"1":"38.85610","2":"-77.05120","3":"332"},{"1":"38.85632","2":"-77.11153","3":"123"},{"1":"38.85640","2":"-77.04920","3":"756"},{"1":"38.85725","2":"-77.05332","3":"403"},{"1":"38.85730","2":"-77.05110","3":"2468"},{"1":"38.85780","2":"-77.08673","3":"67"},{"1":"38.85787","2":"-77.05949","3":"579"},{"1":"38.85925","2":"-77.06328","3":"197"},{"1":"38.86010","2":"-76.96720","3":"77"},{"1":"38.86017","2":"-77.04959","3":"679"},{"1":"38.86079","2":"-77.09586","3":"23"},{"1":"38.86230","2":"-77.05994","3":"1351"},{"1":"38.86248","2":"-77.08660","3":"369"},{"1":"38.86267","2":"-76.99464","3":"412"},{"1":"38.86290","2":"-77.05280","3":"656"},{"1":"38.86370","2":"-77.06330","3":"838"},{"1":"38.86383","2":"-77.08032","3":"496"},{"1":"38.86390","2":"-76.99004","3":"190"},{"1":"38.86470","2":"-77.04867","3":"186"},{"1":"38.86559","2":"-76.95210","3":"11"},{"1":"38.86578","2":"-76.97840","3":"104"},{"1":"38.86646","2":"-77.04826","3":"131"},{"1":"38.86647","2":"-77.07613","3":"99"},{"1":"38.86661","2":"-76.98524","3":"180"},{"1":"38.86726","2":"-77.07232","3":"246"},{"1":"38.86737","2":"-76.98804","3":"252"},{"1":"38.86920","2":"-76.95990","3":"38"},{"1":"38.86942","2":"-77.09560","3":"331"},{"1":"38.86944","2":"-77.10450","3":"457"},{"1":"38.87182","2":"-77.10791","3":"627"},{"1":"38.87306","2":"-76.97101","3":"151"},{"1":"38.87376","2":"-77.08923","3":"161"},{"1":"38.87430","2":"-77.00570","3":"580"},{"1":"38.87501","2":"-77.00240","3":"1520"},{"1":"38.87630","2":"-77.00370","3":"1606"},{"1":"38.87639","2":"-77.10774","3":"668"},{"1":"38.87670","2":"-77.01780","3":"4023"},{"1":"38.87674","2":"-76.99447","3":"473"},{"1":"38.87675","2":"-77.02127","3":"944"},{"1":"38.87800","2":"-76.96070","3":"9"},{"1":"38.87843","2":"-77.03023","3":"746"},{"1":"38.87861","2":"-77.00600","3":"3100"},{"1":"38.87887","2":"-77.12070","3":"503"},{"1":"38.87920","2":"-76.99530","3":"1285"},{"1":"38.87982","2":"-77.03741","3":"5160"},{"1":"38.88015","2":"-77.10767","3":"1043"},{"1":"38.88030","2":"-76.98620","3":"2124"},{"1":"38.88070","2":"-77.08596","3":"479"},{"1":"38.88083","2":"-77.09113","3":"232"},{"1":"38.88104","2":"-77.11177","3":"1558"},{"1":"38.88119","2":"-77.00183","3":"2249"},{"1":"38.88150","2":"-77.10396","3":"405"},{"1":"38.88263","2":"-77.10937","3":"732"},{"1":"38.88279","2":"-77.10315","3":"1614"},{"1":"38.88392","2":"-77.11682","3":"434"},{"1":"38.88397","2":"-77.10783","3":"576"},{"1":"38.88400","2":"-76.99540","3":"6982"},{"1":"38.88400","2":"-76.98610","3":"2221"},{"1":"38.88408","2":"-76.95746","3":"13"},{"1":"38.88412","2":"-77.04657","3":"2341"},{"1":"38.88412","2":"-77.01744","3":"3184"},{"1":"38.88462","2":"-77.10108","3":"619"},{"1":"38.88473","2":"-77.09349","3":"592"},{"1":"38.88496","2":"-77.08777","3":"416"},{"1":"38.88510","2":"-77.00230","3":"3290"},{"1":"38.88580","2":"-77.09775","3":"761"},{"1":"38.88627","2":"-77.02224","3":"3784"},{"1":"38.88695","2":"-76.99681","3":"3583"},{"1":"38.88724","2":"-77.02823","3":"3150"},{"1":"38.88730","2":"-77.01894","3":"4742"},{"1":"38.88731","2":"-77.01397","3":"3413"},{"1":"38.88731","2":"-77.02576","3":"1499"},{"1":"38.88732","2":"-76.98357","3":"2173"},{"1":"38.88738","2":"-77.00195","3":"3492"},{"1":"38.88786","2":"-77.09488","3":"1277"},{"1":"38.88810","2":"-77.09308","3":"602"},{"1":"38.88825","2":"-77.04943","3":"9984"},{"1":"38.88855","2":"-77.03243","3":"8207"},{"1":"38.88877","2":"-77.02858","3":"5120"},{"1":"38.88900","2":"-77.09250","3":"770"},{"1":"38.88936","2":"-77.07729","3":"496"},{"1":"38.88960","2":"-76.97690","3":"1504"},{"1":"38.88991","2":"-76.98333","3":"1861"},{"1":"38.88992","2":"-77.07130","3":"1388"},{"1":"38.88994","2":"-76.93723","3":"121"},{"1":"38.88996","2":"-77.00035","3":"3553"},{"1":"38.88999","2":"-76.99519","3":"2474"},{"1":"38.89040","2":"-77.08890","3":"887"},{"1":"38.89054","2":"-77.08095","3":"666"},{"1":"38.89170","2":"-77.08460","3":"1589"},{"1":"38.89216","2":"-77.07937","3":"478"},{"1":"38.89227","2":"-77.01392","3":"3022"},{"1":"38.89230","2":"-77.04360","3":"3056"},{"1":"38.89246","2":"-77.04657","3":"2335"},{"1":"38.89303","2":"-77.02601","3":"3182"},{"1":"38.89324","2":"-77.08604","3":"593"},{"1":"38.89365","2":"-77.07670","3":"822"},{"1":"38.89400","2":"-76.94797","3":"89"},{"1":"38.89451","2":"-77.03162","3":"4035"},{"1":"38.89457","2":"-77.01994","3":"3176"},{"1":"38.89460","2":"-77.07231","3":"1580"},{"1":"38.89472","2":"-77.04513","3":"1106"},{"1":"38.89476","2":"-76.99711","3":"3807"},{"1":"38.89483","2":"-76.98763","3":"3330"},{"1":"38.89485","2":"-77.02324","3":"3191"},{"1":"38.89492","2":"-77.04659","3":"2165"},{"1":"38.89494","2":"-77.09169","3":"565"},{"1":"38.89518","2":"-77.05485","3":"853"},{"1":"38.89520","2":"-77.04360","3":"1180"},{"1":"38.89534","2":"-77.01611","3":"2094"},{"1":"38.89591","2":"-77.02606","3":"2701"},{"1":"38.89593","2":"-77.10525","3":"28"},{"1":"38.89593","2":"-77.08901","3":"1198"},{"1":"38.89601","2":"-77.07811","3":"526"},{"1":"38.89630","2":"-77.04500","3":"2868"},{"1":"38.89692","2":"-77.08650","3":"718"},{"1":"38.89696","2":"-77.00493","3":"15876"},{"1":"38.89706","2":"-76.94745","3":"96"},{"1":"38.89720","2":"-76.98358","3":"1574"},{"1":"38.89722","2":"-77.01935","3":"3357"},{"1":"38.89727","2":"-76.99475","3":"2921"},{"1":"38.89731","2":"-77.07099","3":"2621"},{"1":"38.89732","2":"-77.02232","3":"2850"},{"1":"38.89745","2":"-77.00989","3":"6196"},{"1":"38.89761","2":"-77.08085","3":"785"},{"1":"38.89786","2":"-77.02697","3":"3071"},{"1":"38.89807","2":"-77.03182","3":"3655"},{"1":"38.89836","2":"-77.02787","3":"4321"},{"1":"38.89840","2":"-77.02428","3":"2239"},{"1":"38.89841","2":"-77.03962","3":"3588"},{"1":"38.89854","2":"-76.93186","3":"61"},{"1":"38.89898","2":"-77.07832","3":"94"},{"1":"38.89910","2":"-77.03370","3":"4784"},{"1":"38.89941","2":"-77.01529","3":"2431"},{"1":"38.89963","2":"-77.03169","3":"2836"},{"1":"38.89968","2":"-77.04154","3":"3722"},{"1":"38.89970","2":"-77.02309","3":"5748"},{"1":"38.89970","2":"-77.00891","3":"1902"},{"1":"38.89997","2":"-76.99835","3":"2738"},{"1":"38.89998","2":"-76.99138","3":"2362"},{"1":"38.90028","2":"-77.02982","3":"3288"},{"1":"38.90030","2":"-77.04290","3":"2530"},{"1":"38.90030","2":"-76.98820","3":"3295"},{"1":"38.90036","2":"-77.01211","3":"2034"},{"1":"38.90041","2":"-77.00195","3":"4983"},{"1":"38.90041","2":"-76.98287","3":"1375"},{"1":"38.90080","2":"-77.04700","3":"3537"},{"1":"38.90088","2":"-77.04891","3":"4855"},{"1":"38.90093","2":"-77.01868","3":"5680"},{"1":"38.90138","2":"-76.94188","3":"35"},{"1":"38.90154","2":"-77.04656","3":"2332"},{"1":"38.90176","2":"-77.05108","3":"2729"},{"1":"38.90200","2":"-77.03353","3":"3121"},{"1":"38.90206","2":"-77.03832","3":"3891"},{"1":"38.90220","2":"-77.04337","3":"2265"},{"1":"38.90222","2":"-77.05922","3":"2728"},{"1":"38.90268","2":"-77.02674","3":"3958"},{"1":"38.90276","2":"-77.03863","3":"5196"},{"1":"38.90304","2":"-77.01903","3":"5743"},{"1":"38.90341","2":"-77.04365","3":"2805"},{"1":"38.90358","2":"-77.06779","3":"1416"},{"1":"38.90358","2":"-77.04479","3":"2197"},{"1":"38.90373","2":"-76.98721","3":"1368"},{"1":"38.90375","2":"-77.06269","3":"3324"},{"1":"38.90381","2":"-77.03493","3":"2884"},{"1":"38.90382","2":"-77.02840","3":"3693"},{"1":"38.90383","2":"-77.05348","3":"4738"},{"1":"38.90474","2":"-77.04161","3":"3622"},{"1":"38.90509","2":"-76.99410","3":"1607"},{"1":"38.90513","2":"-77.05689","3":"4380"},{"1":"38.90534","2":"-77.04677","3":"5314"},{"1":"38.90561","2":"-77.02714","3":"5654"},{"1":"38.90570","2":"-77.00560","3":"5760"},{"1":"38.90571","2":"-77.00304","3":"3652"},{"1":"38.90572","2":"-77.02226","3":"5181"},{"1":"38.90590","2":"-77.03250","3":"9597"},{"1":"38.90660","2":"-77.05152","3":"3028"},{"1":"38.90660","2":"-77.03879","3":"4849"},{"1":"38.90706","2":"-77.01523","3":"2418"},{"1":"38.90774","2":"-77.07165","3":"3681"},{"1":"38.90801","2":"-76.99698","3":"284"},{"1":"38.90814","2":"-77.03836","3":"5822"},{"1":"38.90847","2":"-76.93310","3":"29"},{"1":"38.90849","2":"-77.06359","3":"2517"},{"1":"38.90860","2":"-77.03230","3":"5646"},{"1":"38.90890","2":"-77.04478","3":"4725"},{"1":"38.90985","2":"-77.03444","3":"9123"},{"1":"38.91010","2":"-77.04440","3":"13056"},{"1":"38.91097","2":"-77.00495","3":"1994"},{"1":"38.91210","2":"-77.03870","3":"7918"},{"1":"38.91260","2":"-77.01350","3":"3658"},{"1":"38.91265","2":"-77.04183","3":"1543"},{"1":"38.91266","2":"-77.01767","3":"2403"},{"1":"38.91268","2":"-77.03168","3":"5928"},{"1":"38.91272","2":"-77.02215","3":"5009"},{"1":"38.91540","2":"-77.04460","3":"6076"},{"1":"38.91542","2":"-77.01229","3":"3547"},{"1":"38.91550","2":"-77.02220","3":"4889"},{"1":"38.91554","2":"-77.03818","3":"9077"},{"1":"38.91644","2":"-77.06820","3":"1657"},{"1":"38.91679","2":"-77.02814","3":"4968"},{"1":"38.91720","2":"-77.02590","3":"3315"},{"1":"38.91760","2":"-77.03210","3":"8445"},{"1":"38.91762","2":"-77.01597","3":"2405"},{"1":"38.91776","2":"-77.04062","3":"4031"},{"1":"38.91815","2":"-77.00475","3":"1400"},{"1":"38.91881","2":"-77.04157","3":"4390"},{"1":"38.91908","2":"-77.00065","3":"1069"},{"1":"38.92039","2":"-77.02567","3":"1556"},{"1":"38.92067","2":"-77.04368","3":"3657"},{"1":"38.92068","2":"-76.99588","3":"456"},{"1":"38.92107","2":"-77.03189","3":"4597"},{"1":"38.92258","2":"-77.07033","3":"1338"},{"1":"38.92265","2":"-77.07727","3":"2145"},{"1":"38.92292","2":"-77.04258","3":"6373"},{"1":"38.92320","2":"-77.04764","3":"1553"},{"1":"38.92333","2":"-77.03520","3":"3769"},{"1":"38.92358","2":"-77.05005","3":"5381"},{"1":"38.92490","2":"-77.02220","3":"1322"},{"1":"38.92609","2":"-77.03654","3":"5468"},{"1":"38.92680","2":"-77.03220","3":"5324"},{"1":"38.92710","2":"-76.97892","3":"194"},{"1":"38.92750","2":"-76.99719","3":"495"},{"1":"38.92787","2":"-77.04336","3":"798"},{"1":"38.92816","2":"-77.02344","3":"2578"},{"1":"38.92864","2":"-76.99095","3":"394"},{"1":"38.92874","2":"-77.01246","3":"760"},{"1":"38.92946","2":"-77.02782","3":"4602"},{"1":"38.93028","2":"-77.05560","3":"1849"},{"1":"38.93080","2":"-77.03150","3":"6052"},{"1":"38.93190","2":"-77.03880","3":"4496"},{"1":"38.93251","2":"-76.99289","3":"726"},{"1":"38.93367","2":"-76.99102","3":"336"},{"1":"38.93427","2":"-77.05798","3":"1783"},{"1":"38.93460","2":"-76.99550","3":"561"},{"1":"38.93475","2":"-77.07465","3":"1217"},{"1":"38.93604","2":"-77.02465","3":"3353"},{"1":"38.93750","2":"-77.03280","3":"2368"},{"1":"38.93874","2":"-77.08717","3":"1312"},{"1":"38.94115","2":"-77.06204","3":"983"},{"1":"38.94180","2":"-77.02510","3":"1360"},{"1":"38.94202","2":"-77.03265","3":"1114"},{"1":"38.94384","2":"-77.07708","3":"1220"},{"1":"38.94455","2":"-77.06390","3":"1372"},{"1":"38.94761","2":"-77.07938","3":"2189"},{"1":"38.94777","2":"-77.03282","3":"572"},{"1":"38.94966","2":"-77.02733","3":"562"},{"1":"38.95237","2":"-77.00272","3":"175"},{"1":"38.95481","2":"-77.08243","3":"225"},{"1":"38.95502","2":"-77.06996","3":"1079"},{"1":"38.95643","2":"-77.03295","3":"806"},{"1":"38.95660","2":"-77.01981","3":"320"},{"1":"38.96115","2":"-77.08866","3":"214"},{"1":"38.96176","2":"-77.08600","3":"584"},{"1":"38.96497","2":"-77.07595","3":"210"},{"1":"38.96499","2":"-77.10338","3":"290"},{"1":"38.97500","2":"-77.01121","3":"374"},{"1":"38.97522","2":"-77.01686","3":"1585"},{"1":"38.97709","2":"-77.09459","3":"334"},{"1":"38.97793","2":"-77.00647","3":"555"},{"1":"38.98110","2":"-77.09743","3":"397"},{"1":"38.98128","2":"-77.01134","3":"238"},{"1":"38.98246","2":"-77.09199","3":"211"},{"1":"38.98353","2":"-77.09537","3":"316"},{"1":"38.98363","2":"-77.00631","3":"130"},{"1":"38.98469","2":"-77.09454","3":"896"},{"1":"38.98674","2":"-77.00003","3":"206"},{"1":"38.98700","2":"-77.02942","3":"81"},{"1":"38.98856","2":"-77.09654","3":"272"},{"1":"38.98954","2":"-77.09803","3":"345"},{"1":"38.98972","2":"-77.02385","3":"252"},{"1":"38.99025","2":"-77.02935","3":"240"},{"1":"38.99064","2":"-77.10024","3":"186"},{"1":"38.99238","2":"-77.10010","3":"261"},{"1":"38.99268","2":"-77.02946","3":"140"},{"1":"38.99521","2":"-77.02918","3":"143"},{"1":"38.99765","2":"-77.03450","3":"22"},{"1":"38.99939","2":"-77.03155","3":"103"},{"1":"38.99963","2":"-77.10965","3":"204"},{"1":"39.00058","2":"-77.00149","3":"34"},{"1":"39.07633","2":"-77.14138","3":"46"},{"1":"39.08371","2":"-77.14944","3":"45"},{"1":"39.08413","2":"-77.15129","3":"56"},{"1":"39.08438","2":"-77.14687","3":"78"},{"1":"39.08539","2":"-77.14580","3":"94"},{"1":"39.09378","2":"-77.20250","3":"10"},{"1":"39.09410","2":"-77.13295","3":"8"},{"1":"39.09477","2":"-77.14521","3":"61"},{"1":"39.09566","2":"-77.15905","3":"49"},{"1":"39.09631","2":"-77.19267","3":"46"},{"1":"39.09764","2":"-77.19664","3":"2"},{"1":"39.09938","2":"-77.18801","3":"11"},{"1":"39.10210","2":"-77.20032","3":"8"},{"1":"39.10221","2":"-77.17709","3":"22"},{"1":"39.10309","2":"-77.19644","3":"39"},{"1":"39.10771","2":"-77.15207","3":"96"},{"1":"39.11031","2":"-77.18267","3":"97"},{"1":"39.11469","2":"-77.17149","3":"155"},{"1":"39.11977","2":"-77.16609","3":"315"},{"1":"39.12004","2":"-77.15699","3":"4"},{"1":"39.12351","2":"-77.15741","3":"4"},{"1":"NA","2":"NA","3":"11101"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
US <- get_stamenmap(
  bbox = c(left = -77.2201, bottom = 38.8006, right = -76.8459, top = 38.9692),
  maptype = "terrain",
  zoom = 12
)
```

```
## Source : http://tile.stamen.com/terrain/12/1169/1565.png
```

```
## Source : http://tile.stamen.com/terrain/12/1170/1565.png
```

```
## Source : http://tile.stamen.com/terrain/12/1171/1565.png
```

```
## Source : http://tile.stamen.com/terrain/12/1172/1565.png
```

```
## Source : http://tile.stamen.com/terrain/12/1173/1565.png
```

```
## Source : http://tile.stamen.com/terrain/12/1169/1566.png
```

```
## Source : http://tile.stamen.com/terrain/12/1170/1566.png
```

```
## Source : http://tile.stamen.com/terrain/12/1171/1566.png
```

```
## Source : http://tile.stamen.com/terrain/12/1172/1566.png
```

```
## Source : http://tile.stamen.com/terrain/12/1173/1566.png
```

```
## Source : http://tile.stamen.com/terrain/12/1169/1567.png
```

```
## Source : http://tile.stamen.com/terrain/12/1170/1567.png
```

```
## Source : http://tile.stamen.com/terrain/12/1171/1567.png
```

```
## Source : http://tile.stamen.com/terrain/12/1172/1567.png
```

```
## Source : http://tile.stamen.com/terrain/12/1173/1567.png
```

```
## Source : http://tile.stamen.com/terrain/12/1169/1568.png
```

```
## Source : http://tile.stamen.com/terrain/12/1170/1568.png
```

```
## Source : http://tile.stamen.com/terrain/12/1171/1568.png
```

```
## Source : http://tile.stamen.com/terrain/12/1172/1568.png
```

```
## Source : http://tile.stamen.com/terrain/12/1173/1568.png
```

```r
ggmap(US)+
  geom_point(data = departures_station,
             aes(x = long,
                 y = lat,
                 color = freq_stations),
             alpha = .8,
             size = 2)+
  theme_map()+
  theme(legend.background = element_blank())+
  labs(color = "Rides per Station")
```

```
## Warning: Removed 46 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-11-1.png)<!-- -->
  
  10. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? Also plot this on top of a map. I think it will be more clear what the patterns are.
  

  
### COVID-19 data

The following exercises will use the COVID-19 data from the NYT.

  11. Create a map that colors the states by the most recent cumulative number of COVID-19 cases (remember, these data report cumulative numbers so you don't need to compute that). Describe what you see. What is the problem with this map?
  
  12. Now add the population of each state to the dataset and color the states by most recent cumulative cases/10,000 people. See the code for doing this with the Starbucks data. You will need to make some modifications. 
  
  13. **CHALLENGE** Choose 4 dates spread over the time period of the data and create the same map as in exercise 12 for each of the dates. Display the four graphs together using faceting. What do you notice?
  
## Minneapolis police stops

These exercises use the datasets `MplsStops` and `MplsDemo` from the `carData` library. Search for them in Help to find out more information.

  14. Use the `MplsStops` dataset to find out how many stops there were for each neighborhood and the proportion of stops that were for a suspicious vehicle or person. Sort the results from most to least number of stops. Save this as a dataset called `mpls_suspicious` and display the table.  
  
  15. Use a `leaflet` map and the `MplsStops` dataset to display each of the stops on a map as a small point. Color the points differently depending on whether they were for suspicious vehicle/person or a traffic stop (the `problem` variable). HINTS: use `addCircleMarkers`, set `stroke = FAlSE`, use `colorFactor()` to create a palette.  
  
  16. Save the folder from moodle called Minneapolis_Neighborhoods into your project/repository folder for this assignment. Make sure the folder is called Minneapolis_Neighborhoods. Use the code below to read in the data and make sure to **delete the `eval=FALSE`**. Although it looks like it only links to the .sph file, you need the entire folder of files to create the `mpls_nbhd` data set. These data contain information about the geometries of the Minneapolis neighborhoods. Using the `mpls_nbhd` dataset as the base file, join the `mpls_suspicious` and `MplsDemo` datasets to it by neighborhood (careful, they are named different things in the different files). Call this new dataset `mpls_all`.


```r
mpls_nbhd <- st_read("Minneapolis_Neighborhoods/Minneapolis_Neighborhoods.shp", quiet = TRUE)
```

  17. Use `leaflet` to create a map from the `mpls_all` data  that colors the neighborhoods by `prop_suspicious`. Display the neighborhood name as you scroll over it. Describe what you observe in the map.
  
  18. Use `leaflet` to create a map of your own choosing. Come up with a question you want to try to answer and use the map to help answer that question. Describe what your map shows. 
  
  
## GitHub link

  19. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 04_exercises.Rmd, provide a link to the 04_exercises.md file, which is the one that will be most readable on GitHub.


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
