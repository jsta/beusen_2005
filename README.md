
<!-- README.md is generated from README.Rmd. Please edit that file -->

Data for:

> Beusen, A.H.W., Dekkers, A.L.M., Bouwman, A.F., Ludwig, W. and
> Harrison, J., 2005. Estimation of global river transport of sediments
> and associated particulate C, N, and P. Global Biogeochemical Cycles,
> 19(4).

``` reval
library(tabulizer)

inpath <- "Beusen et al. - 2005 - Estimation of global river transport of sediments .pdf"

table_a1 <- tabulizer::extract_tables(
  inpath,
  pages = 13:15,
  columns = list(c(300)),
  output = "data.frame")

saveRDS(table_a1, "table_a1.rds")
```

``` r
library(janitor)
library(dplyr)
library(tidyr)

table_a1 <- readRDS("table_a1.rds")

make_table <- function(dt){
  dt <- janitor::remove_empty(dt)
  
  if(suppressWarnings(
    !all(is.na(
      tidyr::separate(dt, 1, c("a", "b"), sep = "\\s(?=\\d)")[,c("b")])))){
    dt <- tidyr::separate(dt, 1, c("a", "b"), sep = "\\s(?=\\d)", convert = TRUE)
  }
  
  dt[,3]  <- coalesce(dt[,3], dt[,4])
  dt      <- dt[,-4]
  dt      <- setNames(dt, 
           c("name", "basin_area_1000km2", "tss_tonkm2y1", 
             "trapping_eff", "fournier_precip_mmd1", "fournier_slope_mkm1",
             "lithology_class", "grass_pct", "wetland_rice_pct", 
             "poc_pred_tonkmyr1", "pn_pred_tonkmyr1", "pp_pred_tonkmyr1", 
             "source"))
}

table_a1 <- lapply(table_a1, make_table)
table_a1 <- bind_rows(table_a1)
write.csv(table_a1, "table_a1.csv", row.names = FALSE)
```

    #> # Description: data.frame [126 × 13] 
    #>    ---------------------------printing row #1---------------------------
    #>    name  basin_area_1000… tss_tonkm2y1 trapping_eff fournier_precip…
    #>    <chr>            <int>        <int>        <dbl>            <dbl>
    #>  1 Amaz…             5854          206         0                 7.3
    #>  2 Nile              3826           31         0.92              2.9
    #>  3 Zaire             3699           12         0                 5.6
    #>  4 Miss…             3203          156         0.06              2.5
    #>  5 Amur              2903           18         0.15              2.3
    #>  6 Para…             2661           30         0.59              4.3
    #>  7 Yeni…             2582            5         0.26              1.8
    #>  8 Ob                2570            6         0.03              1.5
    #>  9 Lena              2418            7         0.01              1.5
    #> 10 Niger             2240           18         0.38              4  
    #> 
    #>    ---------------------------printing row #2---------------------------
    #>    fournier_slope_… lithology_class grass_pct wetland_rice_pct
    #>               <int>           <int>     <int>            <int>
    #>  1                7               3         2                0
    #>  2               14               4        15                0
    #>  3               11               1         3                0
    #>  4                7               3         1                0
    #>  5               10               1         3                0
    #>  6                8               3        11                0
    #>  7               11               4         0                0
    #>  8                5               3        12                0
    #>  9               12               4         0                0
    #> 10               10               1         8                0
    #> 
    #>    ---------------------------printing row #3---------------------------
    #>    poc_pred_tonkmyr1 pn_pred_tonkmyr1 pp_pred_tonkmyr1 source
    #>                <dbl>            <dbl>            <dbl>  <int>
    #>  1               2.9              0.5              0.1      1
    #>  2               0.6              0.1              0        1
    #>  3               1.4              0.2              0.1      1
    #>  4               0.4              0.1              0        1
    #>  5               0.2              0                0        2
    #>  6               1                0.2              0        1
    #>  7               0.4              0.1              0        1
    #>  8               0.3              0.1              0        1
    #>  9               0.3              0                0        1
    #> 10               0.6              0.1              0        1
    #> # … with 116 more rows
