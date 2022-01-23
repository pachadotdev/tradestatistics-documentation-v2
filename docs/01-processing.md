# Data processing

## Tidy Data

The project follows Tidy Data principles exposed in [@tidydata2014] and  [@r4ds2016]. Those principles are closely tied to those of relational databases and Codd’s relational algebra.

<div class="figure">
<img src="fig/data-science.svg" alt="Data pipeline"  />
<p class="caption">(\#fig:unnamed-chunk-1)Data pipeline</p>
</div>

## Data cleaning

In order to ease data processing (i.e. obtaining trade balance, rankings, etc.) the raw datasets from UN COMTRADE were processed to create single tables where:

1. The data considers an aggregation level of six digits in the Harmonized System.
2. All the observations before 2012 were converted from HS rev 2002 to HS rev 2012.
3. There is one column for exports and one for imports, respecting that exports are expressed Free on Board (i.e. no transportation costs) and imports are expressed with Cost of Insurance and Freight.
4. Four tables were created for each year: exports, imports, re-exports and re-imports. Then a full join operation was performed between imports and exports, where missing or non-existing flows are replaced by zero values.
5. The data is offered the same as UN COMTRADE but tidy. There are reporters and partners that appear as "0-unspecified" instead of "NA"/"NULL"/" "/etc.

I also provide alternative tables where I removed transportation costs and mismatches by imputing around 40% of the observations with a gravity-type model and also removed flows with no attributed origin/destination. These tables discount re-imports and re-exports besides harmonizing mismatching flows. The idea of imputing some rows is because I applied a criteria of replacing the observed values with data from the model if and only if that reduced the mismatching between trading partners or if that removed exporter/importer aggregated mismatches larger than 35% or countries reporting zero exports in a year.

## GitHub repositories

* [Getting and cleaning data from UN Comtrade (OTS Yearly Data)](https://github.com/tradestatistics/uncomtrade-datasets-arrow)
* [Creating historic HS12 series (OTS Visualization Data)](https://github.com/tradestatistics/hs12-historic-series)
* [Creating gravity imputed historic HS12 series (also OTS Visualization Data)](https://github.com/pachadotdev/baci-like-hs12)
* [Inflation data (included in the R package)](https://github.com/tradestatistics/inflation-data)
* [RTAs and Tariffs data (included in the API)](https://github.com/pachadotdev/rtas-and-tariffs)
* [Shiny dashboard](https://github.com/tradestatistics/visualization-with-shiny)
* [Plumber API](https://github.com/tradestatistics/plumber-api)
* [Product and country codes from UN Comtrade (OTS Comtrade Codes)](https://github.com/tradestatistics/comtrade-codes)
