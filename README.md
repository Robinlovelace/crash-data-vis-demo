
<!-- README.md is generated from README.Rmd. Please edit that file -->

# crash-data-vis-demo

<!-- badges: start -->
<!-- badges: end -->

The goal of crash-data-vis-demo is to demonstrate how to get and
visualise data on road traffic casualties and road geometries.

``` r
# Install the remotes package if not yet installed:
if (!"remotes" %in% installed.packages()) {
  install.packages("remotes")
}
```

``` r
# Check you have the packages installed:
remotes::install_cran(c("tidyverse", "stats19"))
#> Skipping install of 'tidyverse' from a cran remote, the SHA1 (2.0.0) has not changed since last install.
#>   Use `force = TRUE` to force installation
#> Skipping install of 'stats19' from a cran remote, the SHA1 (3.0.3) has not changed since last install.
#>   Use `force = TRUE` to force installation
```

``` r
library(tidyverse) # for data manipulation and visualisation
#> ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
#> ✔ dplyr     1.1.4          ✔ readr     2.1.5     
#> ✔ forcats   1.0.0          ✔ stringr   1.5.1     
#> ✔ ggplot2   3.4.4.9000     ✔ tibble    3.2.1     
#> ✔ lubridate 1.9.3          ✔ tidyr     1.3.1     
#> ✔ purrr     1.0.2          
#> ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
#> ✖ dplyr::filter() masks stats::filter()
#> ✖ dplyr::lag()    masks stats::lag()
#> ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
library(stats19) # for getting road casualty data
#> Data provided under OGL v3.0. Cite the source and link to:
#> www.nationalarchives.gov.uk/doc/open-government-licence/version/3/
library(tmap)
#> 
#> Attaching package: 'tmap'
#> 
#> The following object is masked from 'package:datasets':
#> 
#>     rivers
tmap_mode("view")
#> tmap mode set to 'view'
```

# Get road casualty data

``` r
# Get road casualty data for 2022
cas = get_stats19(year = 2022, type = "cas")
#> Files identified: dft-road-casualty-statistics-casualty-2022.csv
#>    https://data.dft.gov.uk/road-accidents-safety-data/dft-road-casualty-statistics-casualty-2022.csv
#> Data already exists in data_dir, not downloading
#> Rows: 135480 Columns: 19
#> ── Column specification ────────────────────────────────────────────────────────
#> Delimiter: ","
#> chr  (3): accident_index, accident_reference, lsoa_of_casualty
#> dbl (16): accident_year, vehicle_reference, casualty_reference, casualty_cla...
#> 
#> ℹ Use `spec()` to retrieve the full column specification for this data.
#> ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
col = get_stats19(year = 2022, type = "col")
#> Files identified: dft-road-casualty-statistics-collision-2022.csv
#> 
#>    https://data.dft.gov.uk/road-accidents-safety-data/dft-road-casualty-statistics-collision-2022.csv
#> Data already exists in data_dir, not downloading
#> Reading in: 
#> ~/data/stats19/dft-road-casualty-statistics-collision-2022.csv
#> Rows: 106004 Columns: 36── Column specification ────────────────────────────────────────────────────────
#> Delimiter: ","
#> chr   (6): accident_index, accident_reference, date, local_authority_ons_dis...
#> dbl  (29): accident_year, location_easting_osgr, location_northing_osgr, lon...
#> time  (1): time
#> ℹ Use `spec()` to retrieve the full column specification for this data.
#> ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.date and time columns present, creating formatted datetime column
```

If you want to get more data, you can change 2022 to 1979, which
downloads all data from 1979 to the last year for which data is
available.

We’ll get the data for Edinburgh as follows:

``` r
names(col)
#>  [1] "accident_index"                             
#>  [2] "accident_year"                              
#>  [3] "accident_reference"                         
#>  [4] "location_easting_osgr"                      
#>  [5] "location_northing_osgr"                     
#>  [6] "longitude"                                  
#>  [7] "latitude"                                   
#>  [8] "police_force"                               
#>  [9] "accident_severity"                          
#> [10] "number_of_vehicles"                         
#> [11] "number_of_casualties"                       
#> [12] "date"                                       
#> [13] "day_of_week"                                
#> [14] "time"                                       
#> [15] "local_authority_district"                   
#> [16] "local_authority_ons_district"               
#> [17] "local_authority_highway"                    
#> [18] "first_road_class"                           
#> [19] "first_road_number"                          
#> [20] "road_type"                                  
#> [21] "speed_limit"                                
#> [22] "junction_detail"                            
#> [23] "junction_control"                           
#> [24] "second_road_class"                          
#> [25] "second_road_number"                         
#> [26] "pedestrian_crossing_human_control"          
#> [27] "pedestrian_crossing_physical_facilities"    
#> [28] "light_conditions"                           
#> [29] "weather_conditions"                         
#> [30] "road_surface_conditions"                    
#> [31] "special_conditions_at_site"                 
#> [32] "carriageway_hazards"                        
#> [33] "urban_or_rural_area"                        
#> [34] "did_police_officer_attend_scene_of_accident"
#> [35] "trunk_road_flag"                            
#> [36] "lsoa_of_accident_location"                  
#> [37] "datetime"
table(col$police_force)
#> 
#>   Avon and Somerset        Bedfordshire      Cambridgeshire            Cheshire 
#>                2404                1285                1485                1685 
#>      City of London           Cleveland             Cumbria          Derbyshire 
#>                 175                 674                 854                1974 
#>  Devon and Cornwall              Dorset              Durham         Dyfed-Powys 
#>                2752                1449                 621                 982 
#>               Essex     Gloucestershire  Greater Manchester               Gwent 
#>                2901                 930                2804                 587 
#>           Hampshire       Hertfordshire          Humberside                Kent 
#>                3485                1780                1913                3983 
#>          Lancashire      Leicestershire        Lincolnshire          Merseyside 
#>                2765                1076                1604                2267 
#> Metropolitan Police             Norfolk         North Wales     North Yorkshire 
#>               23327                1537                 802                1174 
#>    Northamptonshire         Northumbria     Nottinghamshire     Police Scotland 
#>                1229                1874                1889                4125 
#>         South Wales     South Yorkshire       Staffordshire             Suffolk 
#>                 944                2038                 507                1163 
#>              Surrey              Sussex       Thames Valley        Warwickshire 
#>                2783                3678                3256                 865 
#>         West Mercia       West Midlands      West Yorkshire           Wiltshire 
#>                1658                4943                4397                1380
```

``` r
col_scotland = col |> 
  filter(str_detect(police_force, "Scotland"))
```

Let’s get a case study area:

``` r
edinburgh_boundary = zonebuilder::zb_zone("Edinburgh", n_circles = 2)
```

We’ll convert the collisions to `sf` objects so we can do a spatial
join:

``` r
col_sf = format_sf(col_scotland, lonlat = TRUE)
#> 0 rows removed with no coordinates
```

Now we can do a spatial filter:

``` r
col_edinburgh = col_sf[edinburgh_boundary, ]
```

We can now plot these on a map:

``` r
tm_shape(col_edinburgh) +
  tm_dots(col = "accident_severity", size = 0.1) +
  tm_layout(legend.position = c("left", "bottom"))
```
