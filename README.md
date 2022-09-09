Background on ACS NY data with PUMS
================

### Econ B2000, MA Econometrics

### Kevin R Foster, the Colin Powell School at the City College of New York, CUNY

### Fall 2022

### A different dataset…

Now we will pivot and use a different dataset, ACS data on wage,
education (and lots of other things). You can get this data from [course
page](https://kfoster.ccny.cuny.edu/classes/fall2022/).

We will work on an extract with only people who live in the state of New
York. It includes quite granular geographic information, getting down to
a person’s neighborhood (referred to as PUMA). Well, that’s not quite
true. In NYC PUMA means a relatively small area although in upstate,
where the population density is so much lower, a PUMA could be quite
large in order to collect together enough people. In NYC, PUMA is a
neighborhood.

The PUMA codes are cryptic. You have to go to the codebook (or, in this
case, the file PUMA_levels.csv or acs2017_codebook.txt from the zip
file) to find out that 3801 codes for Washington Heights/Inwood, 3802 is
Hamilton Heights/Manhattanville/West Harlem, etc. The program will
happily calculate the average value for PUMA (type in *mean(PUMA)* and
see for yourself!) but this is a meaningless value – wtf is the average
neighborhood code value!? A PUMA value here is like a zip code, it’s a
number but you wouldn’t want to do number-type operations such as taking
a mean. (Although R would let you do that.)

If you want to select just people living in a particular neighborhood
then you’d have to look at the list below.

| PUMA | Neighborhood                                                        |
|------|---------------------------------------------------------------------|
| 3701 | NYC-Bronx CD 8–Riverdale, Fieldston & Kingsbridge                   |
| 3702 | NYC-Bronx CD 12–Wakefield, Williamsbridge & Woodlawn                |
| 3703 | NYC-Bronx CD 10–Co-op City, Pelham Bay & Schuylerville              |
| 3704 | NYC-Bronx CD 11–Pelham Parkway, Morris Park & Laconia               |
| 3705 | NYC-Bronx CD 3 & 6–Belmont, Crotona Park East & East Tremont        |
| 3706 | NYC-Bronx CD 7–Bedford Park, Fordham North & Norwood                |
| 3707 | NYC-Bronx CD 5–Morris Heights, Fordham South & Mount Hope           |
| 3708 | NYC-Bronx CD 4–Concourse, Highbridge & Mount Eden                   |
| 3709 | NYC-Bronx CD 9–Castle Hill, Clason Point & Parkchester              |
| 3710 | NYC-Bronx CD 1 & 2–Hunts Point, Longwood & Melrose                  |
| 3801 | NYC-Manhattan CD 12–Washington Heights, Inwood & Marble Hill        |
| 3802 | NYC-Manhattan CD 9–Hamilton Heights, Manhattanville & West Harlem   |
| 3803 | NYC-Manhattan CD 10–Central Harlem                                  |
| 3804 | NYC-Manhattan CD 11–East Harlem                                     |
| 3805 | NYC-Manhattan CD 8–Upper East Side                                  |
| 3806 | NYC-Manhattan CD 7–Upper West Side & West Side                      |
| 3807 | NYC-Manhattan CD 4 & 5–Chelsea, Clinton & Midtown Business District |
| 3808 | NYC-Manhattan CD 6–Murray Hill, Gramercy & Stuyvesant Town          |
| 3809 | NYC-Manhattan CD 3–Chinatown & Lower East Side                      |
| 3810 | NYC-Manhattan CD 1 & 2–Battery Park City, Greenwich Village & Soho  |
| 3901 | NYC-Staten Island CD 3–Tottenville, Great Kills & Annadale          |
| 3902 | NYC-Staten Island CD 2–New Springville & South Beach                |
| 3903 | NYC-Staten Island CD 1–Port Richmond, Stapleton & Mariner’s Harbor  |
| 4001 | NYC-Brooklyn CD 1–Greenpoint & Williamsburg                         |
| 4002 | NYC-Brooklyn CD 4—Bushwick                                          |
| 4003 | NYC-Brooklyn CD 3–Bedford-Stuyvesant                                |
| 4004 | NYC-Brooklyn CD 2–Brooklyn Heights & Fort Greene                    |
| 4005 | NYC-Brooklyn CD 6–Park Slope, Carroll Gardens & Red Hook            |
| 4006 | NYC-Brooklyn CD 8–Crown Heights North & Prospect Heights            |
| 4007 | NYC-Brooklyn CD 16–Brownsville & Ocean Hill                         |
| 4008 | NYC-Brooklyn CD 5–East New York & Starrett City                     |
| 4009 | NYC-Brooklyn CD 18–Canarsie & Flatlands                             |
| 4010 | NYC-Brooklyn CD 17–East Flatbush, Farragut & Rugby                  |
| 4011 | NYC-Brooklyn CD 9–Crown Heights South, Prospect Lefferts & Wingate  |
| 4012 | NYC-Brooklyn CD 7–Sunset Park & Windsor Terrace                     |
| 4013 | NYC-Brooklyn CD 10–Bay Ridge & Dyker Heights                        |
| 4014 | NYC-Brooklyn CD 12–Borough Park, Kensington & Ocean Parkway         |
| 4015 | NYC-Brooklyn CD 14–Flatbush & Midwood                               |
| 4016 | NYC-Brooklyn CD 15–Sheepshead Bay, Gerritsen Beach & Homecrest      |
| 4017 | NYC-Brooklyn CD 11–Bensonhurst & Bath Beach                         |
| 4018 | NYC-Brooklyn CD 13–Brighton Beach & Coney Island                    |
| 4101 | NYC-Queens CD 1–Astoria & Long Island City                          |
| 4102 | NYC-Queens CD 3–Jackson Heights & North Corona                      |
| 4103 | NYC-Queens CD 7–Flushing, Murray Hill & Whitestone                  |
| 4104 | NYC-Queens CD 11–Bayside, Douglaston & Little Neck                  |
| 4105 | NYC-Queens CD 13–Queens Village, Cambria Heights & Rosedale         |
| 4106 | NYC-Queens CD 8–Briarwood, Fresh Meadows & Hillcrest                |
| 4107 | NYC-Queens CD 4–Elmhurst & South Corona                             |
| 4108 | NYC-Queens CD 6–Forest Hills & Rego Park                            |
| 4109 | NYC-Queens CD 2–Sunnyside & Woodside                                |
| 4110 | NYC-Queens CD 5–Ridgewood, Glendale & Middle Village                |
| 4111 | NYC-Queens CD 9–Richmond Hill & Woodhaven                           |
| 4112 | NYC-Queens CD 12–Jamaica, Hollis & St. Albans                       |
| 4113 | NYC-Queens CD 10–Howard Beach & Ozone Park                          |
| 4114 | NYC-Queens CD 14–Far Rockaway, Breezy Point & Broad Channel         |

So you want to tell R to treat PUMA as a factor.

``` r
load("acs2017_ny_data.RData")
attach(acs2017_ny)
PUMA <- as.factor(PUMA)
```

And you might want to do that for some of the others too.

I will leave you to worry over the recoding of the other variables,
because it’s good for the soul. I will show you 2 ways – the quick and
dirty way, and the fancy correct way.

First the quick and dirty way.

``` r
female <- as.factor(female)
print(levels(female))
```

    ## [1] "0" "1"

``` r
levels(female) <- c("male","female")
```

Well, ways,

``` r
educ_indx <- factor((educ_nohs + 2*educ_hs + 3*educ_somecoll + 4*educ_college + 5*educ_advdeg), levels=c(1,2,3,4,5),labels = c("No HS","HS","SmColl","Bach","Adv"))
```

(If you can figure out how that bit of code works, that would be good)

These just type in the levels. But for things like PUMA, it could be a
long list and might not even match every one. To do it better, we need
help from an R package.

``` r
library(tidyverse)
library(plyr)
levels_n <- read.csv("PUMA_levels.csv")
levels_orig <- levels(PUMA) 
levels_new <- join(data.frame(levels_orig),data.frame(levels_n))
levels(PUMA) <- levels_new$New_Level
```

Those commands read in a little csv file that I had made, with the PUMA
codes, then matches the old codes with the new complete text. Note that
I’m lazy so codes in NY state outside of NYC are coded NA.

R will do the summary differently when it knows the variable is a
factor,

``` r
summary(PUMA)
```

    ##                   NYC-Bronx CD 8--Riverdale, Fieldston & Kingsbridge 
    ##                                                                  936 
    ##                NYC-Bronx CD 12--Wakefield, Williamsbridge & Woodlawn 
    ##                                                                 1092 
    ##              NYC-Bronx CD 10--Co-op City, Pelham Bay & Schuylerville 
    ##                                                                  767 
    ##               NYC-Bronx CD 11--Pelham Parkway, Morris Park & Laconia 
    ##                                                                 1115 
    ##        NYC-Bronx CD 3 & 6--Belmont, Crotona Park East & East Tremont 
    ##                                                                 1311 
    ##                NYC-Bronx CD 7--Bedford Park, Fordham North & Norwood 
    ##                                                                  854 
    ##           NYC-Bronx CD 5--Morris Heights, Fordham South & Mount Hope 
    ##                                                                 1112 
    ##                   NYC-Bronx CD 4--Concourse, Highbridge & Mount Eden 
    ##                                                                  917 
    ##              NYC-Bronx CD 9--Castle Hill, Clason Point & Parkchester 
    ##                                                                 1307 
    ##                  NYC-Bronx CD 1 & 2--Hunts Point, Longwood & Melrose 
    ##                                                                 1166 
    ##        NYC-Manhattan CD 12--Washington Heights, Inwood & Marble Hill 
    ##                                                                 1238 
    ##   NYC-Manhattan CD 9--Hamilton Heights, Manhattanville & West Harlem 
    ##                                                                  872 
    ##                                  NYC-Manhattan CD 10--Central Harlem 
    ##                                                                  897 
    ##                                     NYC-Manhattan CD 11--East Harlem 
    ##                                                                  769 
    ##                                  NYC-Manhattan CD 8--Upper East Side 
    ##                                                                 1167 
    ##                      NYC-Manhattan CD 7--Upper West Side & West Side 
    ##                                                                  949 
    ## NYC-Manhattan CD 4 & 5--Chelsea, Clinton & Midtown Business District 
    ##                                                                  944 
    ##          NYC-Manhattan CD 6--Murray Hill, Gramercy & Stuyvesant Town 
    ##                                                                  758 
    ##                      NYC-Manhattan CD 3--Chinatown & Lower East Side 
    ##                                                                 1062 
    ##  NYC-Manhattan CD 1 & 2--Battery Park City, Greenwich Village & Soho 
    ##                                                                 1136 
    ##          NYC-Staten Island CD 3--Tottenville, Great Kills & Annadale 
    ##                                                                 1303 
    ##                NYC-Staten Island CD 2--New Springville & South Beach 
    ##                                                                 1173 
    ##  NYC-Staten Island CD 1--Port Richmond, Stapleton & Mariner's Harbor 
    ##                                                                 1621 
    ##                         NYC-Brooklyn CD 1--Greenpoint & Williamsburg 
    ##                                                                 1293 
    ##                                          NYC-Brooklyn CD 4--Bushwick 
    ##                                                                 1060 
    ##                                NYC-Brooklyn CD 3--Bedford-Stuyvesant 
    ##                                                                 1082 
    ##                    NYC-Brooklyn CD 2--Brooklyn Heights & Fort Greene 
    ##                                                                 1320 
    ##            NYC-Brooklyn CD 6--Park Slope, Carroll Gardens & Red Hook 
    ##                                                                 1168 
    ##            NYC-Brooklyn CD 8--Crown Heights North & Prospect Heights 
    ##                                                                 1077 
    ##                         NYC-Brooklyn CD 16--Brownsville & Ocean Hill 
    ##                                                                  904 
    ##                     NYC-Brooklyn CD 5--East New York & Starrett City 
    ##                                                                 1321 
    ##                             NYC-Brooklyn CD 18--Canarsie & Flatlands 
    ##                                                                 2422 
    ##                  NYC-Brooklyn CD 17--East Flatbush, Farragut & Rugby 
    ##                                                                 1250 
    ##  NYC-Brooklyn CD 9--Crown Heights South, Prospect Lefferts & Wingate 
    ##                                                                  818 
    ##                     NYC-Brooklyn CD 7--Sunset Park & Windsor Terrace 
    ##                                                                 1291 
    ##                        NYC-Brooklyn CD 10--Bay Ridge & Dyker Heights 
    ##                                                                 1519 
    ##         NYC-Brooklyn CD 12--Borough Park, Kensington & Ocean Parkway 
    ##                                                                 1698 
    ##                               NYC-Brooklyn CD 14--Flatbush & Midwood 
    ##                                                                 1479 
    ##      NYC-Brooklyn CD 15--Sheepshead Bay, Gerritsen Beach & Homecrest 
    ##                                                                 1903 
    ##                         NYC-Brooklyn CD 11--Bensonhurst & Bath Beach 
    ##                                                                 2234 
    ##                    NYC-Brooklyn CD 13--Brighton Beach & Coney Island 
    ##                                                                  925 
    ##                          NYC-Queens CD 1--Astoria & Long Island City 
    ##                                                                 1748 
    ##                      NYC-Queens CD 3--Jackson Heights & North Corona 
    ##                                                                 1316 
    ##                  NYC-Queens CD 7--Flushing, Murray Hill & Whitestone 
    ##                                                                 2290 
    ##                  NYC-Queens CD 11--Bayside, Douglaston & Little Neck 
    ##                                                                 1344 
    ##         NYC-Queens CD 13--Queens Village, Cambria Heights & Rosedale 
    ##                                                                 2148 
    ##                NYC-Queens CD 8--Briarwood, Fresh Meadows & Hillcrest 
    ##                                                                 1393 
    ##                             NYC-Queens CD 4--Elmhurst & South Corona 
    ##                                                                  973 
    ##                            NYC-Queens CD 6--Forest Hills & Rego Park 
    ##                                                                 1041 
    ##                                NYC-Queens CD 2--Sunnyside & Woodside 
    ##                                                                 1158 
    ##                NYC-Queens CD 5--Ridgewood, Glendale & Middle Village 
    ##                                                                 2040 
    ##                           NYC-Queens CD 9--Richmond Hill & Woodhaven 
    ##                                                                 1694 
    ##                       NYC-Queens CD 12--Jamaica, Hollis & St. Albans 
    ##                                                                 2438 
    ##                          NYC-Queens CD 10--Howard Beach & Ozone Park 
    ##                                                                 1304 
    ##         NYC-Queens CD 14--Far Rockaway, Breezy Point & Broad Channel 
    ##                                                                  954 
    ##                                                                 NA's 
    ##                                                               125514

``` r
summary(educ_indx)
```

    ##  No HS     HS SmColl   Bach    Adv 
    ##  53267  55119  34012  30802  23385

To find mean and standard deviation by neighborhood, you could use
something like this,

``` r
ddply(acs2017_ny, .(PUMA), summarize, mean = round(mean(AGE), 2), sd = round(sd(AGE), 2), n_obsv = length(PUMA))
```

    ##     PUMA  mean    sd n_obsv
    ## 1    100 41.70 23.85   1819
    ## 2    200 43.47 23.45   2624
    ## 3    300 44.88 23.60   1724
    ## 4    401 45.12 24.28   1597
    ## 5    402 42.23 24.18   1774
    ## 6    403 43.22 23.66   2202
    ## 7    500 40.01 23.73   2214
    ## 8    600 39.77 23.29   1670
    ## 9    701 36.64 22.44   1692
    ## 10   702 42.13 23.02   1265
    ## 11   703 43.70 24.11   1945
    ## 12   704 44.71 23.83   2043
    ## 13   800 43.93 23.62   1871
    ## 14   901 45.35 24.04   1288
    ## 15   902 40.74 22.29   1126
    ## 16   903 38.70 22.42    947
    ## 17   904 43.70 23.84   1141
    ## 18   905 40.84 22.75   1099
    ## 19   906 41.48 24.26   1752
    ## 20  1000 42.77 23.43   1536
    ## 21  1101 44.33 24.22   1061
    ## 22  1102 42.49 23.23   1153
    ## 23  1201 44.28 24.74    952
    ## 24  1202 42.72 24.68   1122
    ## 25  1203 43.80 23.84    941
    ## 26  1204 43.57 23.84   1701
    ## 27  1205 40.35 23.50   1333
    ## 28  1206 37.94 22.96   1112
    ## 29  1207 45.42 23.92   1787
    ## 30  1300 43.73 23.35   1885
    ## 31  1400 43.80 24.01   1909
    ## 32  1500 40.94 23.49   1914
    ## 33  1600 43.30 24.68   1510
    ## 34  1700 43.52 23.86   1407
    ## 35  1801 42.63 23.60   1092
    ## 36  1802 43.08 23.23   1356
    ## 37  1900 42.81 23.57   1694
    ## 38  2001 37.81 22.86    923
    ## 39  2002 43.45 23.90   2030
    ## 40  2100 46.88 23.54   1404
    ## 41  2201 41.39 23.92   1311
    ## 42  2202 44.64 23.67   1358
    ## 43  2203 44.31 24.08   1950
    ## 44  2300 37.97 22.08   1105
    ## 45  2401 45.15 23.39   1109
    ## 46  2402 42.78 24.42   2172
    ## 47  2500 42.73 24.33   3051
    ## 48  2600 43.71 24.47   2039
    ## 49  2701 44.76 23.71   1118
    ## 50  2702 42.69 23.16   1533
    ## 51  2801 43.81 23.74   1542
    ## 52  2802 43.19 23.91   1620
    ## 53  2901 41.72 23.03   1042
    ## 54  2902 42.99 22.82   1195
    ## 55  2903 34.24 23.39   1323
    ## 56  3001 44.04 23.76    965
    ## 57  3002 42.34 24.24    943
    ## 58  3003 32.23 24.23   1232
    ## 59  3101 43.50 22.72    959
    ## 60  3102 43.58 23.51   1341
    ## 61  3103 43.50 23.89   1283
    ## 62  3104 39.12 23.54   1093
    ## 63  3105 42.69 24.40   1679
    ## 64  3106 42.68 23.85   1518
    ## 65  3107 42.05 23.57   1762
    ## 66  3201 43.25 25.02   1543
    ## 67  3202 44.08 24.19   1376
    ## 68  3203 43.40 24.06   1013
    ## 69  3204 43.77 23.56   1267
    ## 70  3205 42.64 23.83   1215
    ## 71  3206 40.57 23.55   1207
    ## 72  3207 42.79 24.08   1076
    ## 73  3208 43.68 23.96   1188
    ## 74  3209 43.11 23.37   1021
    ## 75  3210 42.34 23.58    987
    ## 76  3211 41.49 22.93    968
    ## 77  3212 44.51 25.12    962
    ## 78  3301 45.37 24.21    934
    ## 79  3302 43.40 24.24   1018
    ## 80  3303 43.01 24.03   1279
    ## 81  3304 41.35 23.85   1268
    ## 82  3305 48.60 24.31   1326
    ## 83  3306 43.79 22.92   1097
    ## 84  3307 41.86 22.68    881
    ## 85  3308 41.31 23.04   1131
    ## 86  3309 43.69 23.07    948
    ## 87  3310 39.30 22.38    975
    ## 88  3311 40.25 23.23   1036
    ## 89  3312 40.43 22.31    974
    ## 90  3313 40.90 23.82    966
    ## 91  3701 43.12 25.58    936
    ## 92  3702 40.22 22.96   1092
    ## 93  3703 43.63 24.07    767
    ## 94  3704 42.05 24.57   1115
    ## 95  3705 34.78 22.47   1311
    ## 96  3706 35.15 22.40    854
    ## 97  3707 33.70 22.15   1112
    ## 98  3708 35.25 22.01    917
    ## 99  3709 38.88 23.99   1307
    ## 100 3710 35.34 22.09   1166
    ## 101 3801 40.55 22.58   1238
    ## 102 3802 35.62 20.80    872
    ## 103 3803 39.45 21.16    897
    ## 104 3804 38.39 21.37    769
    ## 105 3805 43.53 23.63   1167
    ## 106 3806 42.44 22.85    949
    ## 107 3807 40.20 19.30    944
    ## 108 3808 40.66 21.37    758
    ## 109 3809 40.98 22.18   1062
    ## 110 3810 39.03 20.90   1136
    ## 111 3901 42.89 23.76   1303
    ## 112 3902 41.08 23.88   1173
    ## 113 3903 40.75 22.72   1621
    ## 114 4001 35.39 20.42   1293
    ## 115 4002 34.12 19.37   1060
    ## 116 4003 36.03 20.78   1082
    ## 117 4004 36.75 20.23   1320
    ## 118 4005 36.84 20.31   1168
    ## 119 4006 38.50 20.69   1077
    ## 120 4007 39.63 21.48    904
    ## 121 4008 36.65 22.17   1321
    ## 122 4009 41.51 23.14   2422
    ## 123 4010 42.14 23.08   1250
    ## 124 4011 39.77 22.98    818
    ## 125 4012 36.75 21.46   1291
    ## 126 4013 42.91 22.97   1519
    ## 127 4014 35.35 24.37   1698
    ## 128 4015 38.65 23.33   1479
    ## 129 4016 42.18 24.11   1903
    ## 130 4017 39.67 22.74   2234
    ## 131 4018 45.54 24.77    925
    ## 132 4101 38.65 20.21   1748
    ## 133 4102 38.72 22.55   1316
    ## 134 4103 44.60 23.11   2290
    ## 135 4104 45.76 23.24   1344
    ## 136 4105 41.99 23.29   2148
    ## 137 4106 40.17 23.88   1393
    ## 138 4107 40.09 21.87    973
    ## 139 4108 42.64 23.42   1041
    ## 140 4109 41.20 20.83   1158
    ## 141 4110 40.21 22.13   2040
    ## 142 4111 40.69 21.64   1694
    ## 143 4112 40.37 22.80   2438
    ## 144 4113 39.78 22.77   1304
    ## 145 4114 39.47 24.25    954

Although tapply would also work fine.

Here’s the 90th and 10th percentiles of wages by neighborhood,

``` r
dat_use1 <- subset(acs2017_ny,((INCWAGE > 0) & in_NYC))
ddply(dat_use1, .(PUMA), summarize, inc90 = quantile(INCWAGE,probs = 0.9), inc10 = quantile(INCWAGE,probs = 0.1), n_obs = length(INCWAGE))
```

    ##    PUMA  inc90 inc10 n_obs
    ## 1  3701 120000  3220   424
    ## 2  3702  85000  6700   541
    ## 3  3703 100500  3750   366
    ## 4  3704  90000  6980   510
    ## 5  3705  52000  3000   537
    ## 6  3706  63200  5940   359
    ## 7  3707  60000  4000   439
    ## 8  3708  62000  6000   376
    ## 9  3709  78800  5220   503
    ## 10 3710  55000  3580   420
    ## 11 3801 100000  5000   670
    ## 12 3802 120000  3000   399
    ## 13 3803 130000  6000   478
    ## 14 3804 120000  7000   368
    ## 15 3805 300000 17900   636
    ## 16 3806 326000  7860   509
    ## 17 3807 268000 10000   635
    ## 18 3808 300000 20560   460
    ## 19 3809 140000  5000   515
    ## 20 3810 300000  6000   695
    ## 21 3901 127000  6220   617
    ## 22 3902 125000  8000   524
    ## 23 3903 100000  7100   771
    ## 24 4001 149500 10000   736
    ## 25 4002  82000  9000   581
    ## 26 4003 110000  7200   557
    ## 27 4004 166000  7000   786
    ## 28 4005 200000 12000   681
    ## 29 4006 114000  8740   585
    ## 30 4007  79000  4800   361
    ## 31 4008  73000  6000   549
    ## 32 4009 100000  9600  1178
    ## 33 4010  80200  8360   610
    ## 34 4011  95400  7000   407
    ## 35 4012 102200  6880   625
    ## 36 4013 124000  7440   773
    ## 37 4014  90000  5590   654
    ## 38 4015 100000  7450   710
    ## 39 4016 101200  6000   899
    ## 40 4017  97000  7200  1070
    ## 41 4018 100000  5000   368
    ## 42 4101 104000  9600  1041
    ## 43 4102  82400  8000   624
    ## 44 4103 100000  7180  1107
    ## 45 4104 110000  8000   661
    ## 46 4105 100000  7000  1080
    ## 47 4106 102000  8000   641
    ## 48 4107  70000  8000   499
    ## 49 4108 140000 10000   563
    ## 50 4109 129600 11600   655
    ## 51 4110 100000 10000  1049
    ## 52 4111  90000  8680   865
    ## 53 4112  84800  7260  1213
    ## 54 4113  93000  6000   625
    ## 55 4114 108300  6700   378

You could also use table (or crosstabs) for factors with fewer items,

``` r
table(educ_indx,female)
```

    ##          female
    ## educ_indx  male female
    ##    No HS  27180  26087
    ##    HS     27309  27810
    ##    SmColl 15847  18165
    ##    Bach   14632  16170
    ##    Adv    10254  13131

``` r
xtabs(~educ_indx + female)
```

    ##          female
    ## educ_indx  male female
    ##    No HS  27180  26087
    ##    HS     27309  27810
    ##    SmColl 15847  18165
    ##    Bach   14632  16170
    ##    Adv    10254  13131

Want proportions instead of counts?

``` r
prop.table(table(educ_indx,female))
```

    ##          female
    ## educ_indx       male     female
    ##    No HS  0.13826080 0.13270087
    ##    HS     0.13891701 0.14146552
    ##    SmColl 0.08061144 0.09240278
    ##    Bach   0.07443091 0.08225450
    ##    Adv    0.05216064 0.06679553

Try it and see what happens if you use table with PUMA…

This data includes not just whether a person has a college degree but
also what field was the degree in: Economics or Psychology, for
instance. Look over the codebook about DEGFIELD and DEGFIELDD (that
second D means more detail) to see the codes. Maybe look at 10th and
90th percentiles by degree field?
