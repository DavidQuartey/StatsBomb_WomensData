Getting Started With
================

StatsBomb Data
--------------

have recently launched itself as a data provider of event data, it's an exciting release and certainly enters the market from the off as highly competitive to Opta and others. All of the Statsbomb staff seemed to have started their careers in football analytics as a hobby via personal blogs and the Statsbomb blog itself. They know too well that access to data is one of the biggest barriers to getting involved in this fast-growing hobby and profession. So it's great to see Ted, Thom and others opening the castle gates to others instead of gleefully teasing from the turrets as they announce free data!

Statsbomb have made an initial release of 9 matches of their data, what makes it better is that the data is from women's football. Making it a double good deed, free data and encouraging the development and exposure of the women's game.

Getting the Data
----------------

First and foremost you must sign-up for the data, this allows Statsbomb to keep track of their release and if we abide by there request then hopefully it will enable them to release more free data. Head over to ???? to sign up.

Once you sign up it will lead you to a GitHub page that will have instructions on how to install the helpful R package Statsbomb have released and get that data!

But for the purposes of helping people get stuck in, I will share how I did it.

The Initial Setup
-----------------

You need the latest version of R for the package to work, so I updated mine on a mac via this code - \[uncomment to use\]

``` r
## run to start with (MAC)
# update R studeio to latest version 
#devtools::install_github("AndreaCirilloAC/updateR")
#library(updateR)
#updateR(admin_password = "PASSWORD") - replace PASSWORD with your system password 
```

Then I installed the package via devtools (make sure that is installed and loaded)

``` r
# install and load the StatsBombR package
devtools::install_github("statsbomb/StatsBombR")
```

    FALSE Downloading GitHub repo statsbomb/StatsBombR@master
    FALSE from URL https://api.github.com/repos/statsbomb/StatsBombR/zipball/master

    FALSE Installing StatsBombR

    FALSE '/Library/Frameworks/R.framework/Resources/bin/R' --no-site-file  \
    FALSE   --no-environ --no-save --no-restore --quiet CMD INSTALL  \
    FALSE   '/private/var/folders/xl/8s0d62w940z5p1w1hxynt0yw0000gn/T/Rtmp1HIiIU/devtools9e2673489c64/statsbomb-StatsBombR-b8a3c47'  \
    FALSE   --library='/Library/Frameworks/R.framework/Versions/3.5/Resources/library'  \
    FALSE   --install-tests

    FALSE 

``` r
require(StatsBombR)
```

    FALSE Loading required package: StatsBombR

    FALSE Loading required package: dplyr

    FALSE 
    FALSE Attaching package: 'dplyr'

    FALSE The following objects are masked from 'package:stats':
    FALSE 
    FALSE     filter, lag

    FALSE The following objects are masked from 'package:base':
    FALSE 
    FALSE     intersect, setdiff, setequal, union

    FALSE Loading required package: stringi

    FALSE Loading required package: stringr

    FALSE Loading required package: tibble

    FALSE Loading required package: rvest

    FALSE Loading required package: xml2

    FALSE Loading required package: RCurl

    FALSE Loading required package: bitops

    FALSE Loading required package: doParallel

    FALSE Loading required package: foreach

    FALSE Loading required package: iterators

    FALSE Loading required package: parallel

    FALSE Loading required package: httr

    FALSE Loading required package: jsonlite

    FALSE Loading required package: purrr

    FALSE 
    FALSE Attaching package: 'purrr'

    FALSE The following object is masked from 'package:jsonlite':
    FALSE 
    FALSE     flatten

    FALSE The following objects are masked from 'package:foreach':
    FALSE 
    FALSE     accumulate, when

    FALSE The following object is masked from 'package:rvest':
    FALSE 
    FALSE     pluck

    FALSE Loading required package: SDMTools

Loop Through and Grab That Data!
--------------------------------

The StatsbombR package makes this so easy to get the data and into a format that is ready to use, other data providers take note! So to download all of the event data from the matches I simply grabbed a list of available games and then looped through each game and pulled the event data. This meant I was left with a dataframe of all events from all matches, perfect!

``` r
require(dplyr) ## the package was used to bind rows (bind_rows())

## see what competitions are available 
Comp <- FreeCompetitions()
```

    FALSE [1] "Whilst we are keen to share data and facilitate research, we also urge you to be responsible with the data. Please register your details on https://www.statsbomb.com/resource-centre and read our User Agreement carefully."

``` r
Matches <- FreeMatches(Comp$competition_id)
```

    FALSE [1] "Whilst we are keen to share data and facilitate research, we also urge you to be responsible with the data. Please register your details on https://www.statsbomb.com/resource-centre and read our User Agreement carefully."

``` r
## load in the events from each match 
counter <- 1
for (i in 1:nrow(Matches)) {
  temp <- get.matchFree(Matches[i,])
  temp <- cleanlocations(temp)
  temp <- goalkeeperinfo(temp)
  temp <- shotinfo(temp)
  temp <- defensiveinfo(temp)
  
  if(counter == 1){
   events <- temp
  }else{
   events <- bind_rows(events, temp)
  } # end of else
  counter <- 1 + counter 
} # end of for loop
```

    FALSE [1] "Whilst we are keen to share data and facilitate research, we also urge you to be responsible with the data. Please register your details on https://www.statsbomb.com/resource-centre and read our User Agreement carefully."
    FALSE [1] "Whilst we are keen to share data and facilitate research, we also urge you to be responsible with the data. Please register your details on https://www.statsbomb.com/resource-centre and read our User Agreement carefully."
    FALSE [1] "Whilst we are keen to share data and facilitate research, we also urge you to be responsible with the data. Please register your details on https://www.statsbomb.com/resource-centre and read our User Agreement carefully."
    FALSE [1] "Whilst we are keen to share data and facilitate research, we also urge you to be responsible with the data. Please register your details on https://www.statsbomb.com/resource-centre and read our User Agreement carefully."
    FALSE [1] "Whilst we are keen to share data and facilitate research, we also urge you to be responsible with the data. Please register your details on https://www.statsbomb.com/resource-centre and read our User Agreement carefully."
    FALSE [1] "Whilst we are keen to share data and facilitate research, we also urge you to be responsible with the data. Please register your details on https://www.statsbomb.com/resource-centre and read our User Agreement carefully."
    FALSE [1] "Whilst we are keen to share data and facilitate research, we also urge you to be responsible with the data. Please register your details on https://www.statsbomb.com/resource-centre and read our User Agreement carefully."
    FALSE [1] "Whilst we are keen to share data and facilitate research, we also urge you to be responsible with the data. Please register your details on https://www.statsbomb.com/resource-centre and read our User Agreement carefully."
    FALSE [1] "Whilst we are keen to share data and facilitate research, we also urge you to be responsible with the data. Please register your details on https://www.statsbomb.com/resource-centre and read our User Agreement carefully."

Status Report
-------------

Just a little bit of code to run to double-check that everything went smoothly, unnecessary but handy.

``` r
print("-------- STATSBOMB FREE DATA COLLECTION --------")
```

    ## [1] "-------- STATSBOMB FREE DATA COLLECTION --------"

``` r
print(paste0("### TOTAL MATCHES                              ",length(unique(events$match_id))))
```

    ## [1] "### TOTAL MATCHES                              9"

``` r
print(paste0("### TOTAL EVENTS                           ",nrow(events)))
```

    ## [1] "### TOTAL EVENTS                           22745"

``` r
print("--------------------/ end /---------------------")
```

    ## [1] "--------------------/ end /---------------------"

Ready, Set.... Go
-----------------

No we have a database (called 'events') of all actions from all matches that we can then use in our Statsbomb discovery projects moving forwards. So that we don't need to run the above code each time, I am going to save the events dataframe as an RDS file. RDS files can superquick to load and will allow to us explore the world of possibilities in nano-seconds everytime we start a project.

``` r
saveRDS(events, "SB_events_DB.RDS")
```
