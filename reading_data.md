reading_data
================
Yu
2024-05-20

## Scrape a table

I want the first table from [this
page](https://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm)

read in the html

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"
drug_use_html = read_html(url)

drug_use_html
```

    ## {html_document}
    ## <html lang="en">
    ## [1] <head>\n<link rel="P3Pv1" href="http://www.samhsa.gov/w3c/p3p.xml">\n<tit ...
    ## [2] <body>\r\n\r\n<noscript>\r\n<p>Your browser's Javascript is off. Hyperlin ...

extract the table(s); focus on the first one

``` r
tabl_marj = 
drug_use_html %>% 
  html_nodes(css = "table") %>% 
  .[[1]] %>% 
  html_table() %>% 
  slice(-1)
```

html_nodes(css = “table”): extract the tables .\[\[1\]\]/first(): just
use the first one html_table: converts an HTML table into a data frame
slice(-1): delete the first row

## Star Wars Movie info

I want the data form \[here\] (<https://www.imdb.com/list/ls070150896/>)

``` r
url = "https://www.imdb.com/list/ls070150896/"

swm_html = read_html(url)
```

Grab elements that I want.

``` r
title_vec = 
  swm_html %>% 
  html_nodes(css = ".lister-item-header a") %>% 
  html_text()

gross_rev_vec = 
  swm_html %>%
  html_elements(".text-small:nth-child(7) span:nth-child(5)") %>%
  html_text()

runtime_vec = 
  swm_html %>%
  html_elements(".runtime") %>%
  html_text()

swm_df = 
  tibble(
    title = title_vec,
    rev = gross_rev_vec,
    runtime = runtime_vec)
```

“.lister-item-header a”: is from Selector Gadget. html_text: only select
texts tibble(): form a data frame

## Get some water data

This is coming from an API

``` r
nyc_water = 
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.csv") %>% 
  content("parsed")
```

    ## Rows: 45 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## dbl (4): year, new_york_city_population, nyc_consumption_million_gallons_per...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
nyc_water = 
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.json") %>% 
  content("text") %>% 
  jsonlite::fromJSON() %>% 
  as_tibble()
```

GET: get data from this API content(): parse content in the data json:
another type of dataset jsonlite::fromJSON(): parse the data

## BRFSS

Some process, different data

``` r
brfss_2010 = 
  GET("https://chronicdata.cdc.gov/resource/acme-vg9e.csv", 
      query = list("$limit" = 5000)) %>% 
  content("parsed")
```

    ## Rows: 5000 Columns: 23
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (16): locationabbr, locationdesc, class, topic, question, response, data...
    ## dbl  (6): year, sample_size, data_value, confidence_limit_low, confidence_li...
    ## lgl  (1): locationid
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

query = list(“\$limit” = 5000): default is 1000
