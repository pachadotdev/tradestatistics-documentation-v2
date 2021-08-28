# Accesing the data

## Before downloading datasets

If you are going to download data, you have to read the [Code of Conduct](https://docs.tradestatistics.io/index.html#code-of-conduct) first.

## API

*Since Aug 2021 the API features an interactive page https://api.tradestatistics.io/__docs__/.*

The advantage of the API over https download is that you can filter what to obtain and also access some additional tables.

To obtain exactly the same data as with compressed files, please refer to \@ref(yrpc-year-reporter-partner-and-commodity-code).

If you use R you'll need `jsonlite` package.


```r
library(jsonlite)
```

These packages are also useful:


```r
library(dplyr)
library(stringr)
```

### Available tables


```r
as_tibble(fromJSON("https://api.tradestatistics.io/tables"))
```

```
## # A tibble: 14 × 3
##    table                   description                  source                  
##    <chr>                   <chr>                        <chr>                   
##  1 countries               Countries metadata           UN Comtrade (with modif…
##  2 reporters               Reporters for a given year   UN Comtrade (with modif…
##  3 partners                Partners for a given year    UN Comtrade (with modif…
##  4 commodities             Commodities metadata         Center for Internationa…
##  5 commodities_shortnames  Commodities short names      Center for Internationa…
##  6 commodities_communities Commodities communities      Open Trade Statistics   
##  7 yrpc                    Reporter-Partner trade at c… Open Trade Statistics   
##  8 yrp                     Reporter-Partner trade at a… Open Trade Statistics   
##  9 yrc                     Reporter trade at commodity… Open Trade Statistics   
## 10 yr                      Reporter trade at aggregate… Open Trade Statistics   
## 11 yr-communities          Reporter trade at commodity… Open Trade Statistics   
## 12 yr-groups               Reporter trade at commodity… Open Trade Statistics   
## 13 yc                      Commodity trade at detailed… Open Trade Statistics   
## 14 years                   Minimum and maximum years w… Open Trade Statistics
```

### Metadata


```r
## Countries (no filter)
rda_countries <- "countries.rda"

if (!file.exists(rda_countries)) {
  countries <- as_tibble(fromJSON(
    "https://api.tradestatistics.io/countries"
  ))

  save(countries, file = rda_countries, compress = "xz")

  countries
} else {
  load(rda_countries)

  countries
}
```

```
## # A tibble: 249 × 6
##    country_iso country_name_engli… country_fullname_engl… continent_id continent
##    <chr>       <chr>               <chr>                         <int> <chr>    
##  1 afg         Afghanistan         Afghanistan                       1 Asia     
##  2 alb         Albania             Albania                           2 Europe   
##  3 dza         Algeria             Algeria                           3 Africa   
##  4 asm         American Samoa      American Samoa                    4 Oceania  
##  5 and         Andorra             Andorra                           2 Europe   
##  6 ago         Angola              Angola                            3 Africa   
##  7 aia         Anguilla            Anguilla                          5 Americas 
##  8 atg         Antigua and Barbuda Antigua and Barbuda               5 Americas 
##  9 arg         Argentina           Argentina                         5 Americas 
## 10 arm         Armenia             Armenia                           1 Asia     
## # … with 239 more rows, and 1 more variable: eu28_member <int>
```

```r
## Commodities (no filter)
rda_commodities <- "commodities.rda"

if (!file.exists(rda_commodities)) {
  commodities <- as_tibble(fromJSON(
    "https://api.tradestatistics.io/commodities"
  ))

  save(commodities, file = rda_commodities, compress = "xz")

  commodities
} else {
  load(rda_commodities)

  commodities
}
```

```
## # A tibble: 1,340 × 4
##    commodity_code commodity_fullname_english       group_code group_fullname_en…
##    <chr>          <chr>                            <chr>      <chr>             
##  1 0101           Horses, asses, mules and hinnie… 01         Animals; live     
##  2 0102           Bovine animals; live             01         Animals; live     
##  3 0103           Swine; live                      01         Animals; live     
##  4 0104           Sheep and goats; live            01         Animals; live     
##  5 0105           Poultry; live, fowls of the spe… 01         Animals; live     
##  6 0106           Animals, n.e.s. in chapter 01; … 01         Animals; live     
##  7 0201           Meat of bovine animals; fresh o… 02         Meat and edible m…
##  8 0202           Meat of bovine animals; frozen   02         Meat and edible m…
##  9 0203           Meat of swine; fresh, chilled o… 02         Meat and edible m…
## 10 0204           Meat of sheep or goats; fresh, … 02         Meat and edible m…
## # … with 1,330 more rows
```

Please notice that these tables include some aliases. 

`countries` includes some meta-codes, `c-xx` where `xx` must the first two letters of a continent and `all`, this is:


```
## # A tibble: 6 × 2
##   Alias Meaning                                      
##   <chr> <chr>                                        
## 1 c-af  Alias for all valid ISO codes in Africa      
## 2 c-am  Alias for all valid ISO codes in the Americas
## 3 c-as  Alias for all valid ISO codes in Asia        
## 4 c-eu  Alias for all valid ISO codes in Europe      
## 5 c-oc  Alias for all valid ISO codes in Oceania     
## 6 all   Alias for all valid ISO codes in the World
```

`commodities` also includes some meta-codes, `xx` for the first two digits of a code and those digits are the commodity group and `all`, this is:


```
## # A tibble: 98 × 2
##    Alias Meaning                                                                
##    <chr> <chr>                                                                  
##  1 01    Alias for all codes in the group Animals; live                         
##  2 02    Alias for all codes in the group Meat and edible meat offal            
##  3 03    Alias for all codes in the group Fish and crustaceans, molluscs and ot…
##  4 04    Alias for all codes in the group Dairy produce; birds' eggs; natural h…
##  5 05    Alias for all codes in the group Animal originated products; not elsew…
##  6 06    Alias for all codes in the group Trees and other plants, live; bulbs, …
##  7 07    Alias for all codes in the group Vegetables and certain roots and tube…
##  8 08    Alias for all codes in the group Fruit and nuts, edible; peel of citru…
##  9 09    Alias for all codes in the group Coffee, tea, mate and spices          
## 10 10    Alias for all codes in the group Cereals                               
## # … with 88 more rows
```

### API parameters

The tables provided withing our API contain at least one of these fields:

* Year (`y`) 
* Reporter ISO (`r`)
* Partner ISO (`p`)
* Commodity Code (`c`)

The most detailed table is `yrpc` that contains all bilateral flows at commodity level.

With respect to `y` you can pass any integer contained in [1962,2020].

Both `r` and `p` accept any valid ISO code or alias contained in the [countries](https://api.tradestatistics.io/countries) table. For example, both `chl` (valid ISO code) and `c-am` (continent Americas, an alias) are valid API filtering parameters.

`c` takes any valid commodity code or alias from the [commodities](https://api.tradestatistics.io/commodities). For example, both `0101` (valid HS commodity code) and `01` (valid HS group code) are valid API filtering parameters.

By default the API takes `c = "all"` by default.

You can always skip `c`, but `y`, `r` and `p` are required to return data.

### Available reporters

The only applicable filter is by year.


```r
# Available reporters (filter by year)
as_tibble(fromJSON(
  "https://api.tradestatistics.io/reporters?y=2018"
))
```

```
## # A tibble: 158 × 1
##    reporter_iso 
##    <chr>        
##  1 0-unspecified
##  2 abw          
##  3 afg          
##  4 ago          
##  5 alb          
##  6 and          
##  7 are          
##  8 arg          
##  9 arm          
## 10 atg          
## # … with 148 more rows
```

### YRPC (Year, Reporter, Partner and Commodity Code)

The applicable filters here are year, reporter, partner and commodity code.


```r
# Year - Reporter - Partner - Commodity Code

yrpc <- as_tibble(fromJSON(
  "https://api.tradestatistics.io/yrpc?y=2018&r=usa&p=mex&c=8703"
))

yrpc
```

```
## # A tibble: 1 × 6
##   year  reporter_iso partner_iso commodity_code trade_value_usd_exp
##   <chr> <chr>        <chr>       <chr>                        <dbl>
## 1 2018  usa          mex         8703                    2883606299
## # … with 1 more variable: trade_value_usd_imp <dbl>
```

Columns definition:

* `reporter_iso`: Official ISO-3 code for the reporter (e.g. the country that reports X dollars in exports/imports from/to country Y)
* `partner_iso`: Official ISO-3 code for the partner
* `commodity_code`: Official Harmonized System rev. 1992 (HS92) commodity code (e.g. according to the \code{commodities} table in the API, 8703 stands for "Motor cars and other motor vehicles; principally designed for the transport of persons (other than those of heading no. 8702), including station wagons and racing cars")
* `export_value_usd`: Exports measured in nominal United States Dollars (USD)
* `import_value_usd`: Imports measured in nominal United States Dollars (USD)

### YRC (Year, Reporter and Commodity Code)

The only applicable filter is by year, reporter and commodity code.


```r
# Year - Reporter - Commodity Code

yrc <- as_tibble(fromJSON(
  "https://api.tradestatistics.io/yrc?y=2018&r=chl"
))

yrc
```

```
## # A tibble: 1,207 × 5
##    year  reporter_iso commodity_code trade_value_usd_exp trade_value_usd_imp
##    <chr> <chr>        <chr>                        <dbl>               <dbl>
##  1 2018  chl          0101                       6394932             8809619
##  2 2018  chl          0102                        505423               12140
##  3 2018  chl          0103                         18500             1241532
##  4 2018  chl          0105                             0             5430990
##  5 2018  chl          0106                       1569353             1255946
##  6 2018  chl          0201                      40726955          1088763116
##  7 2018  chl          0202                      34881935           123568194
##  8 2018  chl          0203                     438007756           168831581
##  9 2018  chl          0204                      34683313                   0
## 10 2018  chl          0206                      52906487             3607658
## # … with 1,197 more rows
```

### YRP (Year, Reporter and Partner)

The only applicable filter is by year, reporter and partner.


```r
# Year - Reporter - Partner
yrp <- as_tibble(fromJSON(
  "https://api.tradestatistics.io/yrp?y=2018&r=chl&p=arg"
))

yrp
```

```
## # A tibble: 1 × 5
##   year  reporter_iso partner_iso trade_value_usd_exp trade_value_usd_imp
##   <chr> <chr>        <chr>                     <int>               <dbl>
## 1 2018  chl          arg                  1004446750          3664898260
```

### YC (Year and Commodity Code)

The only applicable filter is by year and commodity code.


```r
# Year - Commodity Code
yc <- as_tibble(fromJSON(
  "https://api.tradestatistics.io/yc?y=2018&c=0101"
))

yc
```

```
## # A tibble: 1 × 4
##   year  commodity_code trade_value_usd_exp trade_value_usd_imp
##   <chr> <chr>                        <dbl>               <dbl>
## 1 2018  0101                    2738929567          2818410753
```

### YR (Year and Reporter)

The only applicable filter is by year and reporter.


```r
## Year - Reporter
yr <- as_tibble(fromJSON(
  "https://api.tradestatistics.io/yr?y=2018&r=chl"
))

yr
```

```
## # A tibble: 1 × 4
##   year  reporter_iso trade_value_usd_exp trade_value_usd_imp
##   <chr> <chr>                      <dbl>               <dbl>
## 1 2018  chl                  94429692300         87054036525
```

## R Package

To ease API using, we provide an [R Package](https://ropensci.github.io/tradestatistics/). This package is a part of [ROpenSci](https://ropensci.org/) and its documentation is available on a separate [pkgdown site](https://ropensci.github.io/tradestatistics/).

Here's what the package does:

<div class="figure">
<img src="fig/data-diagram.svg" alt="R package flow"  />
<p class="caption">(\#fig:unnamed-chunk-8)R package flow</p>
</div>

## Dashboard (beta)

To ease API using, we provide a [Shiny Dashboard](https://shiny.tradestatistics.io/) that is still under improvements.

## Apache Arrow datasets

This [zip](https://tradestatistics.io/hs92-visualization.zip) contains all the arrow datasets used to run the API and dashboard. Please check the [md5sum](https://tradestatistics.io/hs92-visualization-zip-md5sum.txt) to verify data integrity after downloading.

However, it's maybe more efficient to sync from this DigitalOcean Space: https://tradestatistics.ams3.digitaloceanspaces.com. This can be used, for example, with [rclone](https://rclone.org/) by running the command
```
rclone sync spaces:tradestatistics/hs92-visualization hs92-visualization
```
And also check the [md5sums](https://tradestatistics.io/hs92-visualization-spaces-md5sums.txt) if you sync from DO Spaces.
<!-- update with: find ~/hs-rev1992-visualization/* -type f -print0 | xargs -0 md5sum > ~/md5sums.txt -->

