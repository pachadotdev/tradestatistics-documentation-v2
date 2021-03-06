--- 
title: "Open Trade Statistics"
author: "Pachá (Mauricio Vargas Sepúlveda)"
date: "2022-01-23"
site: bookdown::bookdown_site
documentclass: book
bibliography: [references.bib]
link-citations: yes
url: 'https\://docs.tradestatistics.io'
github-repo: tradestatistics/documentation-v2
---

# About

## Project goals

Open Trade Statistics is an independent project that values reproducible research and provides tidy trade data. This project advocates for openness and collaboration, so the contribution is focused on:

* Releasing all the source code on [GitHub](https://github.com/tradestatistics)
* Sharing curated data (direct download and API)
* Pull requests and new ideas are always welcome

The ultimate goal of this project is to ease data analysis and promote a reproducibility culture. You can contact me at `mavargas11@uc.cl`.

## Data sources and availability

All of the product data shown on the OTS site is classified using either SITC (Standard International Trade Classification) or HS (Harmonized System), the raw data was obtained from [UN COMTRADE](http://comtrade.un.org/) and its under their authorization that we share claned versions of the original raw datasets.

We used different trade classifications according to the data availability.

|Classification |Availability|
|---------------|------------|
|HS rev 2002    |2002 - 2011 |
|HS rev 2012    |2012 - 2020 |

These datasets were used all together in order to create a unified data series for the period 2002-2020 with all products classified under HS rev 2012.

To adjust for inflation (see the details in the R package), I also obtained GDP and inflation data from the World Bank.

## Code of Conduct

Before you proceed to download the data, please read this carefully.

No matter your gender, gender identity and expression, age, sexual orientation, disability, physical appearance, body size, race, ethnicity, religion (or lack thereof), or technology choices you are able to use this data for any non-commercial purpose, including academic.

Our software (dashboard, R package, and API) is released under [Apache 2.0 license](https://www.apache.org/licenses/LICENSE-2.0) but all the data is under Creative Commons 
Attribution-NonCommercial 4.0 International License.

Commercial purposes are strictly out of the boundaries of what you can do with this data according to [UN Comtrade dissemination clauses](https://comtrade.un.org/db/help/PolicyOnUseAndRedissemination.pdf).

Except where otherwise noted, content on this site is licensed under the [Creative Commons BY-NC 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). Besides CC-BY-NC, when downloading datasets you also agree to the usage conditions explained both to [UN Comtrade Online Usage Agreement](https://comtrade.un.org/db/help/licenseagreement.aspx).

## Open Source Sponsorships

This project is hosted on [DigitalOcean](https://digitalocean.com) as a part of the Open Source Sponsorships. If you register with this [link](https://m.do.co/c/6119f0430dad), you'll get free credits to try DigitalOcean and we also get credits for the project:

```
Referral Link
https://m.do.co/c/6119f0430dad
```
