---
title: "Midterm Project"
author: "Anisha Shrestha"
date: "11/09/2019"
output: 
  html_document:
    keep_md: yes
---


##### Task 1:  You will write a final data set as a csv file to the data folder. Name this file midterm-data-your_github_username.csv. The number of rows should be the same as the number of rows in the GTD data set.


> All the library used in the analysis



```r
options(warn = -1) 
# to suppress all the warnings in the output
library(tidyverse)
```

```
## ── Attaching packages ────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.2.1     ✔ purrr   0.3.3
## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
## ✔ tidyr   1.0.0     ✔ stringr 1.4.0
## ✔ readr   1.3.1     ✔ forcats 0.4.0
```

```
## ── Conflicts ───────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
library(plyr)  # If I will not use plyr then I am getting Error: Column `county` must be length 1 (the group size), not 30089
```

```
## -------------------------------------------------------------------------
```

```
## You have loaded plyr after dplyr - this is likely to cause problems.
## If you need functions from both plyr and dplyr, please load plyr first, then dplyr:
## library(plyr); library(dplyr)
```

```
## -------------------------------------------------------------------------
```

```
## 
## Attaching package: 'plyr'
```

```
## The following objects are masked from 'package:dplyr':
## 
##     arrange, count, desc, failwith, id, mutate, rename, summarise,
##     summarize
```

```
## The following object is masked from 'package:purrr':
## 
##     compact
```

```r
require(data.table)
```

```
## Loading required package: data.table
```

```
## 
## Attaching package: 'data.table'
```

```
## The following objects are masked from 'package:dplyr':
## 
##     between, first, last
```

```
## The following object is masked from 'package:purrr':
## 
##     transpose
```

```r
library(usmap) 
library(ggmap)
```

```
## Google's Terms of Service: https://cloud.google.com/maps-platform/terms/.
```

```
## Please cite ggmap if you use it! See citation("ggmap") for details.
```

```r
library(lubridate)
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:data.table':
## 
##     hour, isoweek, mday, minute, month, quarter, second, wday,
##     week, yday, year
```

```
## The following object is masked from 'package:plyr':
## 
##     here
```

```
## The following object is masked from 'package:base':
## 
##     date
```

```r
library(ggrepel)
library(mapdata)
```

```
## Loading required package: maps
```

```
## 
## Attaching package: 'maps'
```

```
## The following object is masked from 'package:plyr':
## 
##     ozone
```

```
## The following object is masked from 'package:purrr':
## 
##     map
```

```r
library(mapproj)
```



> Using Glimpse() function helps to have a look over all the datasets.



```r
GTD <- read_csv("data/GTD.csv")
```

```
## Parsed with column specification:
## cols(
##   .default = col_double(),
##   provstate = col_character(),
##   city = col_character(),
##   summary = col_character(),
##   attacktype1_txt = col_character(),
##   targtype1_txt = col_character(),
##   targsubtype1_txt = col_character(),
##   corp1 = col_character(),
##   target1 = col_character(),
##   gname = col_character(),
##   weaptype1_txt = col_character(),
##   weapsubtype1_txt = col_character(),
##   propextent_txt = col_character(),
##   propcomment = col_character(),
##   scite1 = col_character()
## )
```

```
## See spec(...) for full column specifications.
```

```r
  glimpse(GTD)
```

```
## Observations: 146
## Variables: 38
## $ iyear            <dbl> 2013, 2013, 2013, 2013, 2013, 2013, 2013, 2013,…
## $ imonth           <dbl> 1, 2, 2, 4, 4, 4, 4, 4, 4, 4, 5, 5, 5, 7, 8, 8,…
## $ iday             <dbl> 17, 7, 25, 15, 15, 16, 17, 18, 18, 19, 20, 20, …
## $ provstate        <chr> "Washington", "California", "Washington", "Mass…
## $ city             <chr> "Tacoma", "Corona", "Seattle", "Boston", "Bosto…
## $ latitude         <dbl> 47.25288, 33.87529, 47.59320, 42.35027, 42.3502…
## $ longitude        <dbl> -122.44429, -117.56644, -122.30057, -71.08098, …
## $ vicinity         <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
## $ summary          <chr> "01/17/2013: An explosive device detonated outs…
## $ doubtterr        <dbl> 0, 1, 0, 0, 0, 1, 1, 0, 0, 0, 1, 1, 1, 0, 0, 0,…
## $ multiple         <dbl> 0, 0, 0, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 0, 0, 0,…
## $ success          <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 0, 0, 0, 1, 1, 1,…
## $ suicide          <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
## $ attacktype1      <dbl> 3, 2, 7, 3, 3, 7, 7, 2, 3, 4, 8, 8, 8, 7, 7, 3,…
## $ attacktype1_txt  <chr> "Bombing/Explosion", "Armed Assault", "Facility…
## $ targtype1        <dbl> 2, 3, 1, 14, 14, 21, 1, 8, 6, 20, 2, 14, 2, 15,…
## $ targtype1_txt    <chr> "Government (General)", "Police", "Business", "…
## $ targsubtype1     <dbl> 14, 25, 12, 78, 78, 107, 5, 50, 44, NA, 17, 82,…
## $ targsubtype1_txt <chr> "Judge/Attorney/Court", "Police Security Forces…
## $ corp1            <chr> "Washington State Department of Corrections", "…
## $ target1          <chr> "Tacoma Community Justice Center", "Officers", …
## $ gname            <chr> "Unknown", "Anti-Police extremists", "Anarchist…
## $ individual       <dbl> 0, 1, 0, 1, 1, 0, 0, 1, 0, 1, 1, 1, 1, 1, 0, 0,…
## $ nperps           <dbl> -99, 1, -99, 2, 2, -99, -99, 2, -99, 2, 1, 1, 1…
## $ nperpcap         <dbl> 0, 0, 0, 1, 1, 0, 0, 1, 0, 1, 1, 1, 1, 2, 0, 0,…
## $ claimed          <dbl> 0, 1, 1, 1, 1, 0, 0, 1, 0, 1, 1, 1, 1, 1, 1, 1,…
## $ weaptype1        <dbl> 6, 5, 8, 6, 6, 5, 8, 5, 6, 6, 1, 1, 1, 8, 8, 6,…
## $ weaptype1_txt    <chr> "Explosives/Bombs/Dynamite", "Firearms", "Incen…
## $ weapsubtype1     <dbl> 17, 4, 18, 12, 12, 4, 18, 5, 17, 17, NA, NA, NA…
## $ weapsubtype1_txt <chr> "Other Explosive Type", "Rifle/Shotgun (non-aut…
## $ nkill            <dbl> 0, 1, 0, 1, 2, 0, 15, 1, 0, 2, 0, 0, 0, 0, 0, 0…
## $ nwound           <dbl> 0, 1, 0, 132, 132, 0, 151, 0, 0, 16, 0, 0, 0, 0…
## $ property         <dbl> 0, 0, 1, 1, 1, 1, 1, 0, 0, 1, 0, 0, 0, 1, 1, 1,…
## $ propextent       <dbl> NA, NA, 3, 4, 4, 4, 4, NA, NA, 3, NA, NA, NA, 3…
## $ propextent_txt   <chr> NA, NA, "Minor (likely < $1 million)", "Unknown…
## $ propvalue        <dbl> NA, NA, 60000, -99, -99, -99, -99, NA, NA, -99,…
## $ propcomment      <chr> NA, NA, "A single family home and construction …
## $ scite1           <chr> "\"Small   bomb explodes   outside WA Correctio…
```


> Getting state name from provstate.
> Placing DC in the state name `District Of Columbia`.
> Putting manually state name `DC` for District Of Columbia.



```r
mutate(GTD,state=state.abb[match(GTD$provstate,state.name)]) %>%
mutate(state = replace_na(state, 'DC')) -> GTD_dat
```


> Removing `NA` values from county/city
> Recoding ``city` name based on the GTD city values and drop NA values form the dataframe.
> From the repeated count choosing first observation (norepeat)
> Then Left merge : `gtd_merge`



```r
county_city <- read_csv("data/county_city.csv")
```

```
## Parsed with column specification:
## cols(
##   latitude = col_double(),
##   longitude = col_double(),
##   city = col_character(),
##   state = col_character(),
##   county = col_character()
## )
```

```r
county_city$city <- recode(county_city$city,
                          'New York'='New York City',
                          'Saint Cloud'='St. Cloud',
                          'Saint Louis'='St. Louis',
                          'Inglewood'='Ingelwood',
                          'Mc Cook'='McCook',
                          'Tyngsboro'='Tyngsborough')

county_city %>%
drop_na() -> county_all


county_all %>% 
group_by(city, state) %>% 
filter(row_number()==1) ->new_county
new_county %>%
mutate(county= str_replace_all(new_county$county," ", "")) -> dat_county


left_join(GTD_dat, dat_county, by = c('city','state'))->gtd_merge_data
gtd_merge_data%>%
distinct(iday, imonth,iyear,city,targtype1, target1, latitude, longitude, vicinity,summary, doubtterr, multiple, success, suicide, attacktype1, attacktype1_txt, targtype1_txt, targsubtype1, targsubtype1_txt, corp1, gname, individual, nperps, claimed, weapsubtype1,nkill, nwound,property,propextent, propextent_txt ,county, state, .keep_all = TRUE)-> gtd_merge
```


> Filtering datasets that has `City` in it as we are trying to combine county and state \n
> Changing the column name. `Area_Name` to `county` and `State` to `state` \n
> Filtering all the values that has `county` word in it from the column name `county` \n
> There are many typos in the values comparing with GTD data, we recode those county values \n
> Removing spaces from the county name and making one word just like above inorder to match for merge \n
> Left Merge data based on `county` and `state` .



```r
poverty <- read_csv("data/poverty.csv")
```

```
## Parsed with column specification:
## cols(
##   State = col_character(),
##   Area_Name = col_character(),
##   PCTPOVALL_2015 = col_double(),
##   PCTPOV017_2015 = col_double(),
##   MEDHHINC_2015 = col_number()
## )
```

```r
Filter_city1<-poverty[-(grep("City", poverty$Area_Name)),]
setnames(Filter_city1, old=c("Area_Name"), new=c("county"))
setnames(Filter_city1, old=c("State"), new=c("state"))
Filter_city1$county<- str_remove_all(Filter_city1$county, "County")

Filter_city1$county <- recode(Filter_city1$county,
                        'St. Louis '='Saint Louis City',
                        'Lafayette Parish'= 'Lafayette',
                        'East Baton Rouge Parish'='EastBatonRouge',
                        'St. Lucie '='Saint Lucie',
                        'Roanoke '='Roanoke City',
                        'Falls Church city'= 'Falls Church City',
                        'McLennan '='Mclennan',
                        'Orleans Parish'= 'Orleans',
                        'District of Columbia'= 'District Of Columbia'
                      ) 


new_poverty<- Filter_city1 %>%
mutate(county= str_replace_all(county," ", ""))

left_join(gtd_merge, new_poverty, by = c('county','state')) -> gtd_merge_county
gtd_merge_county%>%
  distinct(iday, imonth,iyear,city,targtype1, target1, latitude, longitude, vicinity,summary, doubtterr, multiple, success, suicide, attacktype1, attacktype1_txt, targtype1_txt, targsubtype1, targsubtype1_txt, corp1, gname, individual, nperps, claimed, weapsubtype1,nkill, nwound,property,propextent, propextent_txt ,county, state, .keep_all = TRUE) ->final_gtd_county_povert_merge
```



> Getting all state abb from the data population column name `STNAME`. \n
> Replacing `DC` in the place of STNAME i.e `District Of Columbia` and Filtering any values ending with (balance). \n
> Correcting typos in the observation values in order to match with GTD datasets. \n
> Removing all the words ending like `township`,`city`,`City`,`Town`,`village`,`borough`,`County`. \n
> Replacing the values in the data. Some values are different compared from GTD datasets. Column name: `provstate` and changing NAME to city. \n
> Final Merge data from all above is `finalmerge` \n



```r
population <- read_csv("data/population.csv")
```

```
## Parsed with column specification:
## cols(
##   SUMLEV = col_double(),
##   STATE = col_double(),
##   COUNTY = col_double(),
##   PLACE = col_double(),
##   NAME = col_character(),
##   STNAME = col_character(),
##   POPESTIMATE2013 = col_double(),
##   POPESTIMATE2014 = col_double(),
##   POPESTIMATE2015 = col_double(),
##   POPESTIMATE2016 = col_double()
## )
```

```r
population %>%
mutate(population,state=state.abb[match(population$STNAME,state.name)])-> cityname
cityname %>% mutate(state = replace(state, STNAME == "District of Columbia", "DC"))->Pop

values_filt<-Pop[-(grep("(balance)", population$NAME)),]
values_filt$NAME<- str_remove_all(values_filt$NAME, " township")
values_filt$NAME<- str_remove_all(values_filt$NAME, " city")
values_filt$NAME<- str_remove_all(values_filt$NAME, " City")
values_filt$NAME<- str_remove_all(values_filt$NAME, " Town")
values_filt$NAME<- str_remove_all(values_filt$NAME, " town")
values_filt$NAME<- str_remove_all(values_filt$NAME, " village")
values_filt$NAME<- str_remove_all(values_filt$NAME, " borough")
values_filt$NAME<- str_remove_all(values_filt$NAME, " County")


values_filt$NAME <- recode(values_filt$NAME,
                        'New Castle'=  'New City',
                        'New York'= 'New York City',
                        'Kansas'= 'Kansas City',
                        'Inglewood'= 'Ingelwood',
                        'District of Columbia' = 'District of Columbia')

setnames(values_filt, old=c("NAME"), new=c("city"))
values_filt %>% mutate(state = replace(state, city == "District of Columbia", "DC"))->Change_Pop
 

gtd_pop_merge <- Change_Pop %>%
  distinct(POPESTIMATE2013, POPESTIMATE2014, POPESTIMATE2015, POPESTIMATE2016, state, city)
  left_join(final_gtd_county_povert_merge, gtd_pop_merge, by = c('city','state')) -> gtd_pop_coun_pov_merge

  gtd_pop_coun_pov_merge %>%
  distinct(iday, imonth,iyear,city,targtype1, target1, latitude, longitude, vicinity,summary, doubtterr, multiple, success, suicide, attacktype1, attacktype1_txt, targtype1_txt, targsubtype1, targsubtype1_txt, corp1, gname, individual, nperps, claimed, weapsubtype1,nkill, nwound,property,propextent, propextent_txt ,county, state, .keep_all = TRUE) -> finalmerge
```


##### Make a plot of the U.S. with each incident present on the map according to its location. You should incorporate in the graph the city’s population size. Repeat for the three poverty variables.


> Registering API key for google map. \n
> Ploting latitude.y and longitude.y from county/city in order in the map. \n
> fill with color RED for the incident occured in the map. \n



```r
ggmap::register_google(key = "AIzaSyDwCPffwbMwHDQkMITsr4k5ENYXGuCJzSQ")

ggmap(get_googlemap(center=as.numeric(geocode("United States")), scale=2, zoom=4), extent="normal")+ geom_point(data= finalmerge, mapping=aes(x=longitude.x, y=latitude.x), color="red") + labs(x = "Longitude", y = "Latitude") + ggtitle("Incidents (red dots) occured in different places")
```

```
## Source : https://maps.googleapis.com/maps/api/geocode/json?address=United+States&key=xxx
```

```
## Source : https://maps.googleapis.com/maps/api/staticmap?center=37.09024,-95.712891&zoom=4&size=640x640&scale=2&maptype=terrain&key=xxx
```

![](MidtermProject_files/figure-html/unnamed-chunk-6-1.png)<!-- -->


> Map For poverty datasets \n
> plot_usmap helps to map data frames of varying region types (e.g. county, state) \n
> Used Scale_fill_continous to get the gradient of the color \n
> the scale is shown in the right hand side for all Poverty variables.



```r
plot_usmap(data = finalmerge, values = "PCTPOVALL_2015", color = "red") + 
  scale_fill_continuous(name = "Poverty Level 2015 (Percentage)", label = scales::comma) + 
  theme(legend.position = "right")+ labs( subtitle = "Poverty all (2015)") + 
  theme(panel.background = element_rect(color = "white", fill = "lightblue"))
```

![](MidtermProject_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

```r
plot_usmap(data = finalmerge, values = "PCTPOV017_2015", color = "red") + 
  scale_fill_continuous(name = "Poverty Level 2015 - 2017(Percentage)", label = scales::comma) + 
  theme(legend.position = "right")+ labs( subtitle = "Poverty all (2017-2015)") + 
  theme(panel.background = element_rect(color = "white", fill = "lightblue"))
```

![](MidtermProject_files/figure-html/unnamed-chunk-7-2.png)<!-- -->

```r
plot_usmap(data = finalmerge, values = "MEDHHINC_2015", color = "red") + 
  scale_fill_continuous(name = "Median Income", label = scales::comma) + 
  theme(legend.position = "right")+ labs( subtitle = "Median Income for 2015") + 
  theme(panel.background = element_rect(color = "white", fill = "lightblue")) 
```

![](MidtermProject_files/figure-html/unnamed-chunk-7-3.png)<!-- -->


> options(scipen) to remove scientific notation.
> The population year shows similar pattern, thus using gather funtion to make it as one values and ploting in the map (2013-2016).



```r
options(scipen=999) #removes the scientific notation

finalmerge %>%
select(latitude.y, longitude.y, POPESTIMATE2013, POPESTIMATE2014,POPESTIMATE2015,POPESTIMATE2016) %>%
gather(key = population, value = numbers, -latitude.y, -longitude.y) -> pop

map_data("state")%>%
ggplot() + geom_polygon(mapping = aes(x=long, y=lat, group = group),color="white", fill="maroon" )+coord_fixed(1.3) + theme_bw() + guides(fill=FALSE) +
  geom_point(data=pop, aes(longitude.y, latitude.y, size = numbers), fill = "red", color = "Blue") + ggtitle("Over all Population Sizes") + xlab("Longitude") + ylab("Latitide") +
labs(subtitle = "between 2013-2016", size="Population Values")+ coord_map(projection = "albers", lat0 = 39, lat1 = 45)+ guides(fill = FALSE)
```

```
## Coordinate system already present. Adding new coordinate system, which will replace the existing one.
```

![](MidtermProject_files/figure-html/unnamed-chunk-8-1.png)<!-- -->



> for Poverty datasets.\n. Using gather function to plot poverty for 2015 and 2015-2017.



```r
population_all <- finalmerge %>%
  select(latitude.y, longitude.y,state, PCTPOVALL_2015, PCTPOV017_2015) %>%
  gather(key = population, value = numbers, -latitude.y, -longitude.y,-state)
```



```r
plot_usmap(data = population_all, values = "numbers", color = "white") + 
  scale_fill_continuous(name = "Poverty(2015-20117)", label = scales::comma) + 
  theme(legend.position = "left") + labs( subtitle = "Poverty data for 2015 to 2017") + 
  theme(panel.background = element_rect(color = "white", fill = "lightblue"))
```

![](MidtermProject_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
 


##### Exploratory Analysis 

**Total victim Count in a Box plot**

> We are exploring three factor variable like doubtterr, multiple and success. We wanted to explore if the incident was sucessful and what are the weapon type they used, and also how is the attack done. For that we use box plot, barplot.  \n

> Converting `doubtterr`, `multiple` and `success` into factor variables using as.factor \n
> Filtering doubtterr if its 1 or 0 multiple if its 1 or 0 and sucess if its 1 \n
> We group targettype, attacktype, weapontype with kill count and wound count \n
> Get the summary of these column name \n
> In this summary table, we have target type, attak type and weapon type along with all the successful incident (ONLY) with total number of kill and wound. We have victim column form adding two different column i.e (nkill+nwound) \n



```r
finalmerge$doubtterr = as.factor(finalmerge$doubtterr)
finalmerge$multiple = as.factor(finalmerge$multiple)
finalmerge$success = as.factor(finalmerge$success)
barplot_data <- finalmerge %>%
 dplyr:: filter((doubtterr %in% c("1","0")) | (multiple %in% c("1","0")) & (success == "1")) %>%
 dplyr:: group_by(targtype1_txt, attacktype1_txt, weaptype1_txt, nkill, nwound) %>%
dplyr::summarise(counts = n())
barplot_data<-barplot_data %>%
  mutate(victim= nwound+nkill) %>%  
  filter(victim != 0) #filtering all victim not equal to 0
```



**From the Boxplot, It shows there is high number of victim with primary weapon type used  `Explosive/Bombs/Dynamite` and `Incendiary` compared to other weapon types**



```r
barplot_data <- finalmerge %>%
 dplyr:: filter((doubtterr %in% c("1","0")) | (multiple %in% c("1","0")) & (success == "1")) %>%
 dplyr:: group_by(targtype1_txt, attacktype1_txt, weaptype1_txt, nkill, nwound) %>%
dplyr::summarise(counts = n())
barplot_data<-barplot_data %>%
  mutate(victim= nwound+nkill) %>%
  filter(victim != 0) #filtering all victim not equal to 0
head(barplot_data)
```

```
## # A tibble: 6 x 7
## # Groups:   targtype1_txt, attacktype1_txt, weaptype1_txt, nkill [5]
##   targtype1_txt  attacktype1_txt   weaptype1_txt nkill nwound counts victim
##   <chr>          <chr>             <chr>         <dbl>  <dbl>  <int>  <dbl>
## 1 Abortion Rela… Hostage Taking (… Firearms          3      9      1     12
## 2 Airports & Ai… Armed Assault     Firearms          1      4      1      5
## 3 Airports & Ai… Armed Assault     Incendiary        1      2      1      3
## 4 Business       Armed Assault     Firearms          3      9      1     12
## 5 Business       Armed Assault     Melee             1      4      1      5
## 6 Business       Armed Assault     Melee             1     10      1     11
```

```r
#Boxplot for weaptype and victim

ggplot(barplot_data, aes(x = weaptype1_txt, y = victim, fill = weaptype1_txt)) + geom_boxplot() +
facet_wrap( ~ weaptype1_txt,scales = "free_x")+ labs(title = "Comparision of Successful Incident (Victim Count)", subtitle = " with weapon type", x = "Primary Weapon Type ", y = "Victim ")+theme_minimal() +scale_fill_discrete(name = "Weapon Type") + theme( text=element_text(size=11.5, family="Arial light"))
```

![](MidtermProject_files/figure-html/unnamed-chunk-12-1.png)<!-- -->


**From the Barplot, we came can see that there is highest number of victim with attack type is `Bombing/Explosion` and `Facility and Infrastructure`. The highest target type was `Business` and `Private Citizen and property` with lest number in `journalist` and `media` in the barplot.**



```r
ggplot(data = barplot_data, aes(x = targtype1_txt, y = victim, fill = attacktype1_txt))+ geom_col(position = position_dodge()) +coord_flip()+ labs(title = "Target and Attack Type Comparision", subtitle = " with Victim Count", x = "Target Type", y = "Victim Count")+theme_minimal() +scale_fill_discrete(name = "Attack Type") + theme(panel.border = element_blank(), text=element_text(size=11.5, family="Arial light"))
```

![](MidtermProject_files/figure-html/unnamed-chunk-13-1.png)<!-- -->




```r
Population_estimate <- finalmerge %>%
 group_by(city, county, nkill, nwound,  PCTPOV017_2015,PCTPOVALL_2015, MEDHHINC_2015,POPESTIMATE2013, POPESTIMATE2014, POPESTIMATE2015, POPESTIMATE2016 )
his_data<-Population_estimate %>%
  mutate(victim= nwound+nkill) %>% # showing total Victim 
  filter(victim != 0) 

ggplot(his_data, aes(x = PCTPOV017_2015)) +
        geom_histogram(aes(fill = ..count..), binwidth = 3)
```

![](MidtermProject_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

```r
        ggtitle("Histogram For Total wound") +theme_minimal() + theme(panel.border = element_blank(), text=element_text(size=9.5, family="Arial light"))
```

```
## NULL
```


> Relationships between wound and kill based on weapon type used in the attack. \n
> We convert weapon type in to factor variables first and replace any `NA` values with 0.\n 
> Similarly, If we have any frequency of weapon type less than 2 , we group them as `Other` and rest as it is.\n
> Filter any kill and wound if any of them have `0`. \n
> From the scatter plot, we can see that Incendiary has the highest total wound and Explosive/Bombs/Dynamite has the highest total kill. \n



```r
scatter_data<- finalmerge %>%
  select( nkill, nwound, weaptype1, weaptype1_txt) %>%
 mutate(weaptype1 = factor(weaptype1, levels = c(1, 2,3,4,5,6,7,8,9,10,11,12))) %>%
 mutate_if(is.numeric , replace_na, replace = 0) %>%
mutate(weaptype1_txt= replace(weaptype1_txt, weaptype1 < 2, 'Other')) %>%
  filter(nkill>0 | nwound > 0)

ggplot(scatter_data, aes(nkill, nwound, colour = weaptype1_txt, shape = weaptype1_txt)) + geom_point() + theme_classic()+scale_colour_brewer(palette = "Dark2") +scale_shape_manual(values = c(0, 16, 3, 9, 11, 6)) + labs(title = "Scatter Plot for Wound and Kill", subtitle = "by weapon type", x = "Total Kill", y = "Total Wound") +scale_fill_discrete(name = "Weapon Type Text")+theme_minimal()  + theme(panel.border = element_blank(), text=element_text(size=9.5, family="Arial light"))
```

![](MidtermProject_files/figure-html/unnamed-chunk-15-1.png)<!-- -->


> Histogram for poverty data. \n
> Looking at the medain income of house hold form the Poverty data


```r
 ggplot(finalmerge, aes(x = MEDHHINC_2015)) +
        geom_histogram(aes(y=..count../sum(..count..)), binwidth = 10050) +
        scale_y_continuous(name = "Count") +
        ggtitle("Histogram For Median Income of people") +theme_minimal() + theme(panel.border = element_blank(), text=element_text(size=9.5, family="Arial light"))
```

![](MidtermProject_files/figure-html/unnamed-chunk-16-1.png)<!-- -->




###### Dates in format consistent with "yyyy-mm-dd".

> Converting dates into year-months-day formate by the help of as.date function and converting it into data formate in GTD


```r
finalmerge %>%
mutate(Date= as.Date(with(finalmerge, paste(iyear, imonth, iday, sep="-")), "%Y-%m-%d"))-> GTD_withDate 
```


##### 2. Make a plot of the number of incidents by month, according to type of incident as described by the variable individual, and separately by the variable claimed.


```r
GTD_withDate  %>%
mutate(months= strftime(as.POSIXlt(GTD_withDate$Date, format = "%Y-%m-%d"), format="%m")) %>%
select(months, individual, claimed) -> incident_bymonths
incident_bymonths$individual= as.factor(incident_bymonths$individual)
incident_bymonths$claimed = as.factor(incident_bymonths$claimed)


ggplot(data = incident_bymonths, aes(x = months, fill = claimed))+ geom_bar(position = position_dodge()) +coord_flip()+ labs(title = "Individuals affiliated", subtitle = " with or without group", x = "months", y = "Frequency")+theme_minimal() +scale_fill_discrete(name = "Individuals") + theme(panel.border = element_blank(), text=element_text(size=11.5, family="Arial light"))
```

![](MidtermProject_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

> From the above barplot we can see the highest number of incidents by months with 0 (i.e. the perpetrators were either not identified by name, or were known to be affiliated with a group or organization). Individuals are highest on 8 and 7 whereas individuals with highest 1(The perpetrator were identified by name or specific unnamed minors) and not known to be affiliated with a group or organization) is on months 9.


**Bar Plot for Climaed Responsibility for the Attack**


```r
ggplot(data = incident_bymonths, aes(x = months, fill = individual))+ geom_bar(position = position_dodge()) +coord_flip()+ labs(title = " Claimed responsibility for the attack", subtitle = "yes or No", x = "months", y = "Frequency")+theme_minimal() +scale_fill_discrete(name = "Claimed") + theme(panel.border = element_blank(), text=element_text(size=11.5, family="Arial light"))
```

![](MidtermProject_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

> From the above second barplot we can see the highest months with 0 (No claim of responsibility was made) claimed are highest on 8 and 7 whereas individuals with highest 1 (group or person claimed responsibility for the attack) are on months 11 and 12.



##### 3. If we consider the number of wounded and killed combined to be of primary interest, explore the relationships of population size and the three poverty variables to this combined count. Label the most interesting data point in one of the graphs with the group name (gname) that committed the incident.

> The number of wounded and killed combine along with the population size and three proverty variables are explained by the help of scatter plot. \n
> Combining all the nkill and nwound and taking out the percent of population Estimate for all the years.\n
> Here, the group name for victiom count more than 30 are labelled in the graphs.\n



> First we combine the number of kill and wound into one i.e `total_incident`. \n
> If we have any NA values, then we pass 0 and create a new dataframe called `total_summary`.\n
> The population data have very large values so, we convert it into percentage by dividing each with total population count and multiplying it by 100.\n
> We use ggplot inorder to get the scatter plot, where wecan show the color name along with its label. The label shown here is for `total_incident > 30`.\n
> Thus, This is done for all three poverty variables in the datasets.\n
> Hidden `GNAME` label from right.\n
> I used round up to get only 3 numbers after decimals.
> I used digit = 3 inorder to round of the values.



```r
finalmerge %>%
  select(nkill,nwound,POPESTIMATE2013, POPESTIMATE2014,POPESTIMATE2015,POPESTIMATE2016, PCTPOVALL_2015, PCTPOV017_2015,MEDHHINC_2015, gname) %>%
mutate(total_incident = ( finalmerge$nkill + finalmerge$nwound))%>%
mutate_if(is.numeric , replace_na, replace = 0) -> percent_count

ggplot(percent_count, aes(PCTPOVALL_2015,total_incident, colour = gname)) + geom_point()+  geom_text(aes(label=ifelse(total_incident> 50,as.character(gname),'')),hjust=-0.3,vjust=-0.3)+  theme_classic() + labs(title = "Scatter Plot for Victim and Population 2015", subtitle = "by gname", x = "The percent of all residents under the poverty level in the county", y = "Total Victim") +scale_fill_discrete(name = "Weapon Type Text")+theme_minimal()  + theme(panel.border = element_blank(), text=element_text(size=9.5, family="Arial light"))+ theme(legend.position='none')
```

![](MidtermProject_files/figure-html/unnamed-chunk-20-1.png)<!-- -->


The scatter plot shows, `Unknown`, `Muslim Extremist` and `Jihadi-inspired extremists` have the highest count for the attack



```r
ggplot(percent_count, aes(PCTPOV017_2015,total_incident, colour = gname)) + geom_point()+  geom_text(aes(label=ifelse(total_incident> 50,as.character(gname),'')),hjust=-0.3,vjust=-0.3)+  theme_classic() + labs(title = "Scatter Plot for Victim and Population 15_17", subtitle = "by gname", x ="The percent of all residents under the poverty level in the county", y = "Total Victim") +scale_fill_discrete(name = "Weapon Type Text")+theme_minimal()  + theme(panel.border = element_blank(), text=element_text(size=9.5, family="Arial light"))+ theme(legend.position='none')
```

![](MidtermProject_files/figure-html/unnamed-chunk-21-1.png)<!-- -->




```r
ggplot(percent_count, aes(MEDHHINC_2015,total_incident, colour = gname)) + geom_point()+  geom_text(aes(label=ifelse(total_incident> 50,as.character(gname),'')),hjust=-0.3,vjust=-0.3)+  theme_classic() + labs(title = "Scatter Plot for Victim and Median Income POP", subtitle = "by gname", x = "Median household income in the county", y = "Total Victim") +scale_fill_discrete(name = "Weapon Type Text")+theme_minimal()  + theme(panel.border = element_blank(), text=element_text(size=9.5, family="Arial light"))+ theme(legend.position='none')
```

![](MidtermProject_files/figure-html/unnamed-chunk-22-1.png)<!-- -->




> Here, we can see that gname with Unknown followed by Muslim Extremists are the group that has occured highest victims through out the Poverty datasets.



```r
ggplot(percent_count, aes((POPESTIMATE2013),total_incident, colour = gname)) + geom_point()+  geom_text(aes(label=ifelse(total_incident> 50,as.character(gname),'')),hjust=-0.3,vjust=-0.3)+  theme_classic() + labs(title = "Scatter Plot for Victim and Population 2013", subtitle = "by gname", x = "Population  2013", y = "Total Victim") +scale_fill_discrete(name = "Weapon Type Text")+theme_minimal()  + theme(panel.border = element_blank(), text=element_text(size=9.5, family="Arial light"))+ theme(legend.position='none')
```

![](MidtermProject_files/figure-html/unnamed-chunk-23-1.png)<!-- -->

```r
ggplot(percent_count, aes((POPESTIMATE2014),total_incident, colour = gname)) + geom_point()+  geom_text(aes(label=ifelse(total_incident> 50,as.character(gname),'')),hjust=-0.3,vjust=-0.3)+  theme_classic() + labs(title = "Scatter Plot for Victim and Population 2014", subtitle = "by gname", x = "Population  2014", y = "Total Victim") +scale_fill_discrete(name = "Weapon Type Text")+theme_minimal()  + theme(panel.border = element_blank(), text=element_text(size=9.5, family="Arial light"))+ theme(legend.position='none')
```

![](MidtermProject_files/figure-html/unnamed-chunk-23-2.png)<!-- -->

```r
ggplot(percent_count, aes((POPESTIMATE2015),total_incident, colour = gname)) + geom_point()+  geom_text(aes(label=ifelse(total_incident> 50,as.character(gname),'')),hjust=-0.3,vjust=-0.3)+  theme_classic() + labs(title = "Scatter Plot for Victim and Population 2015", subtitle = "by gname", x = "Population  2015", y = "Total Victim") +scale_fill_discrete(name = "Weapon Type Text")+theme_minimal()  + theme(panel.border = element_blank(), text=element_text(size=9.5, family="Arial light"))+ theme(legend.position='none')
```

![](MidtermProject_files/figure-html/unnamed-chunk-23-3.png)<!-- -->

```r
ggplot(percent_count, aes((POPESTIMATE2016),total_incident, colour = gname)) + geom_point()+  geom_text(aes(label=ifelse(total_incident> 50,as.character(gname),'')),hjust=-0.3,vjust=-0.3)+  theme_classic() + labs(title = "Scatter Plot for Victim and Population 2016", subtitle = "by gname", x = "Population  2016", y = "Total Victim") +scale_fill_discrete(name = "Weapon Type Text")+theme_minimal()  + theme(panel.border = element_blank(), text=element_text(size=9.5, family="Arial light"))+ theme(legend.position='none')
```

![](MidtermProject_files/figure-html/unnamed-chunk-23-4.png)<!-- -->



> Not all Gname is shown in the scatter plot. The label are hidden in order to focus the top interesting data point in group name (gname) that committed the incident.



##### 4. If we consider the variable attacktype1_txt to be of primary interest, explore the relationships of population size and the three poverty variables to attacktype1_txt. You will need to collapse any categories of attacktype1_txt that have 3 or few incidents into an “other” category.




```r
#collapse any categories of attacktype1_txt that have 3 or few incidents into an `other` category
finalmerge %>%
  select(POPESTIMATE2013, POPESTIMATE2014, POPESTIMATE2015, POPESTIMATE2016, gname,attacktype1_txt, PCTPOVALL_2015, PCTPOV017_2015, MEDHHINC_2015) %>%
  dplyr::group_by(attacktype1_txt) %>% 
  dplyr::mutate(count = n()) %>%
  mutate(attacktype1_txt= ifelse(count <= 3, 'Other', attacktype1_txt))->populationpov
head(populationpov)
```

```
## # A tibble: 6 x 10
## # Groups:   attacktype1_txt [3]
##   POPESTIMATE2013 POPESTIMATE2014 POPESTIMATE2015 POPESTIMATE2016 gname
##             <dbl>           <dbl>           <dbl>           <dbl> <chr>
## 1          202736          204461          207660          211277 Unkn…
## 2          159138          160883          163659          166785 Anti…
## 3          653095          667963          683505          704352 Anar…
## 4          651090          659180          665984          673184 Musl…
## 5          651090          659180          665984          673184 Musl…
## 6         1001580         1013237         1022627         1025350 Unkn…
## # … with 5 more variables: attacktype1_txt <chr>, PCTPOVALL_2015 <dbl>,
## #   PCTPOV017_2015 <dbl>, MEDHHINC_2015 <dbl>, count <int>
```

```r
ggplot(populationpov, aes(x = attacktype1_txt, y = PCTPOVALL_2015, fill = attacktype1_txt)) + geom_boxplot() + labs(title = "Boxplot  for Attack type and Poverty", subtitle = " poverty_2015", x = "Attack Type Description", y = "percent/under the poverty level ")+theme_minimal() +scale_fill_discrete(name = "Attack Type Text") + theme( text=element_text(size=11.5, family="Arial light"))  + coord_flip()        
```

![](MidtermProject_files/figure-html/unnamed-chunk-24-1.png)<!-- -->

```r
ggplot(populationpov, aes(x = attacktype1_txt, y = PCTPOV017_2015, fill = attacktype1_txt)) + geom_boxplot() +
labs(title = "Boxplot  for Attack Type and Poverty", subtitle = " poverty17_2015", x = "Attack Type Description", y = "percent under the poverty level  ")+theme_minimal() +scale_fill_discrete(name = "Attack Type Text") + theme( text=element_text(size=11.5, family="Arial light"))  + coord_flip()
```

![](MidtermProject_files/figure-html/unnamed-chunk-24-2.png)<!-- -->

```r
ggplot(populationpov, aes(x = attacktype1_txt, y = MEDHHINC_2015, fill = attacktype1_txt)) + geom_boxplot() +
labs(title = "Boxplot  for Attack type and Median Income", subtitle = " poverty_2015", x = "Attack Type Description", y = "Median income level in the county ")+theme_minimal() +scale_fill_discrete(name = "Attack Type Text") + theme( text=element_text(size=11.5, family="Arial light"))  + coord_flip()
```

![](MidtermProject_files/figure-html/unnamed-chunk-24-3.png)<!-- -->

In the first Box plot, we can see that `Facility and infrastructure attack` and `Bombing/Explosion`has the highest attack type along with high percentage under poverty.

In the second Box plot, we can see that `Facility and infrastructure attack` and `Bombing/Explosion`has the highest attack type followed by `other` along with high percentage under poverty. Armed Assault has many outlier in the graph.

In the third Box plot, we can see that `Bombing/Explosion` has the highest attack type followed by `Facility and infrastructure attack` and `unarmed assult` is somewhat similar but with different quantile values has the second highest attack type along with high percentage under poverty. 



> In the population graph along with the attack type, we can see most of the values are accumulated around 0 to 2 percent and there is a increase after 6 percent.


```r
# For all 3 year population Estimates

ggplot(populationpov, aes(x = attacktype1_txt, y = POPESTIMATE2013, fill = attacktype1_txt)) + geom_boxplot() +
labs(title = "Boxplot  for attack type and Average Population", subtitle = " population 2013", x = "Attack Type Description", y = "Population Percentage ")+theme_minimal() +scale_fill_discrete(name = "Attack Type Text") + theme( text=element_text(size=11.5, family="Arial light"))  + coord_flip()        
```

![](MidtermProject_files/figure-html/unnamed-chunk-25-1.png)<!-- -->

```r
ggplot(populationpov, aes(x = attacktype1_txt, y = POPESTIMATE2014, fill = attacktype1_txt)) + geom_boxplot() +
labs(title = "Boxplot  for Attack Type and average population", subtitle = "population 2014", x = "Attack Type Description", y = "Population Percentage")+theme_minimal() +scale_fill_discrete(name = "Attack Type Text") + theme( text=element_text(size=11.5, family="Arial light"))  + coord_flip()
```

![](MidtermProject_files/figure-html/unnamed-chunk-25-2.png)<!-- -->

```r
ggplot(populationpov, aes(x = attacktype1_txt, y = POPESTIMATE2015, fill = attacktype1_txt)) + geom_boxplot() +
labs(title = "Boxplot  for Attack type and average population", subtitle = "population 2015", x = "Attack Type Description", y = "Population Percentage")+theme_minimal() +scale_fill_discrete(name = "Attack Type Text") + theme( text=element_text(size=11.5, family="Arial light"))  + coord_flip()
```

![](MidtermProject_files/figure-html/unnamed-chunk-25-3.png)<!-- -->

```r
ggplot(populationpov, aes(x = attacktype1_txt, y = POPESTIMATE2016, fill = attacktype1_txt)) + geom_boxplot() +
labs(title = "Boxplot  for Attack type and average population", subtitle = "population 2016", x = "Attack Type Description", y = "Population Percentage")+theme_minimal() +scale_fill_discrete(name = "Attack Type Text") + theme( text=element_text(size=11.5, family="Arial light"))  + coord_flip()
```

![](MidtermProject_files/figure-html/unnamed-chunk-25-4.png)<!-- -->

> From the above boxplot, we can see all of them have similar nature, where Unarmed Assult have the highest attack type, followed by Armed Assult. Rest we cannot say exactly based on their outlier. 


##### 5. Explore the relationship between your collapsed version of attacktype1_txt from the previous problem and the variable property, and separately the variable individual.

> Below is the Barplot in order to show the relationship between attach type and variables property and individual.


```r
finalmerge %>%
  select(property,individual, attacktype1_txt)%>%
   mutate(property = factor(property, levels = c(1, 0, -9))) %>%
   mutate(individual = factor(individual, levels = c(1, 0))) %>%
  dplyr::group_by(attacktype1_txt) %>% 
  dplyr::mutate(count = n()) %>%
  mutate(attacktype1_txt= ifelse(count <= 3, 'Other', attacktype1_txt))-> newdata 
#newdata$attacktype1_txt %>% table
         
ggplot(data = newdata, aes(x = attacktype1_txt, fill = individual))+ geom_bar(position = position_dodge()) +coord_flip()+ labs(title = "Attack was carried out by an individual", subtitle = "Yes or No", x = "Attack Type Description", y = "Count")+theme_minimal() +scale_fill_discrete(name = "Individual") + theme(panel.border = element_blank(), text=element_text(size=11.5, family="Arial light"))
```

![](MidtermProject_files/figure-html/unnamed-chunk-26-1.png)<!-- -->

> From the above graph, we can see that the highest number of attack type are seen `Facility/Infrastructure attack`  in `property value` 1,  followed by other in property value 0. on the second. Property value 1 means the incident resulted in property damage due to respective attack type and vice-versa.



```r
ggplot(data = newdata, aes(x = attacktype1_txt, fill = property))+ geom_bar(position = position_dodge()) +coord_flip()+ labs(title = "Evidence of property damage from the incident", subtitle = "(Yes=1) or (No=0) and (Unknown= -9)", x = "Attack Type Description", y = "Count")+theme_minimal() +scale_fill_discrete(name = "Property") + theme(panel.border = element_blank(), text=element_text(size=11.5, family="Arial light"))
```

![](MidtermProject_files/figure-html/unnamed-chunk-27-1.png)<!-- -->


> From the above graph, we can see that the highest number of attack type are seen `Facility/Infrastructure` and property type is 1 followed by Armed Assult attack type being second on the property label 0. 

> Individual value 1 means The perpetrator(s) were identified by name (or specific unnamed minors) and not known to be affiliated with a group or organization and vice-versa.


##### Writing the file in the data folder


```r
write_excel_csv(finalmerge, "data/midterm-finalmerge-annie5696.csv")
```
