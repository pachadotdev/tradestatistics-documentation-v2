# Data processing

## Tidy Data

The project follows Tidy Data principles exposed in [@tidydata2014] and  [@r4ds2016]. Those principles are closely tied to those of relational databases and Codd’s relational algebra.

<div class="figure">
<img src="fig/data-science.svg" alt="Data pipeline"  />
<p class="caption">(\#fig:unnamed-chunk-1)Data pipeline</p>
</div>

## Data cleaning

In order to ease data processing (i.e. obtaining trade balance, rankings, etc.) the raw datasets from UN Comtrade were processed to create single tables where:

1. The data considers an aggregation level of four digits in the Harmonized System
2. All the observations before 1988 were converted from SITC rev 1 and 2 to HS rev 1992
3. There is one column for exports and one for imports, respecting that one column is in FOB units and the other in CIF units
4. A full join operation was performed, where missing or non-existing flows are replaced by zero values

## GitHub repositories

* [Getting and cleaning data from UN Comtrade (OTS Yearly Data)](https://github.com/tradestatistics/uncomtrade-datasets-arrow)
* [Creating historic HS92 series (OTS Visualization Data)](https://github.com/tradestatistics/hs92-historic-series)
* [Inflation data (included in the R package)](https://github.com/tradestatistics/inflation-data)
* [Shiny dashboard](https://github.com/tradestatistics/visualization-with-shiny)
* [Plumber API](https://github.com/tradestatistics/plumber-api)
* [Product and country codes from UN Comtrade (OTS Comtrade Codes)](https://github.com/tradestatistics/comtrade-codes)
* [Scraping data from The Atlas of Economic Complexity (OTS Atlas Data)](https://github.com/tradestatistics/atlas-data)

## Coding style and performant code

I followed [Tidyverse Style Guide](http://style.tidyverse.org/), [@advancedr2014] and [@masteringsoftware2017].