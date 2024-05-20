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
