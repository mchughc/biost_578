HW1 Biost 578, Caitlin McHugh
========================================================

Homework 1 assignment is to find all HCV gene expression Illumina data, submitted by a Yale investigator. 

I initially load in the SQLite database from GEO.

```r
source("http://bioconductor.org/biocLite.R")
```

```
## Bioconductor version 2.13 (BiocInstaller 1.12.0), ?biocLite for help
```

```r
biocLite(c("GEOmetadb", "GEOquery"))
```

```
## BioC_mirror: http://bioconductor.org
## Using Bioconductor version 2.13 (BiocInstaller 1.12.0), R version 3.0.2.
## Installing package(s) 'GEOmetadb' 'GEOquery'
```

```
## 
## The downloaded binary packages are in
## 	/var/folders/vk/w81j5cj13ks7lfyxj5t0hrjr0000gn/T//RtmpNQV5xy/downloaded_packages
```

```r
library(GEOmetadb)
```

```
## Loading required package: GEOquery
## Loading required package: Biobase
## Loading required package: BiocGenerics
## Loading required package: parallel
## 
## Attaching package: 'BiocGenerics'
## 
## The following objects are masked from 'package:parallel':
## 
##     clusterApply, clusterApplyLB, clusterCall, clusterEvalQ,
##     clusterExport, clusterMap, parApply, parCapply, parLapply,
##     parLapplyLB, parRapply, parSapply, parSapplyLB
## 
## The following object is masked from 'package:stats':
## 
##     xtabs
## 
## The following objects are masked from 'package:base':
## 
##     anyDuplicated, append, as.data.frame, as.vector, cbind,
##     colnames, duplicated, eval, evalq, Filter, Find, get,
##     intersect, is.unsorted, lapply, Map, mapply, match, mget,
##     order, paste, pmax, pmax.int, pmin, pmin.int, Position, rank,
##     rbind, Reduce, rep.int, rownames, sapply, setdiff, sort,
##     table, tapply, union, unique, unlist
## 
## Welcome to Bioconductor
## 
##     Vignettes contain introductory material; view with
##     'browseVignettes()'. To cite Bioconductor, see
##     'citation("Biobase")', and for packages 'citation("pkgname")'.
## 
## Setting options('download.file.method.GEOquery'='auto')
## Loading required package: RSQLite
## Loading required package: DBI
```

```r
library(GEOquery)
library(RSQLite)
library(data.table)
getSQLiteFile()
```

```
## Unzipping...
```

```
## Warning: closing unused connection 5
## (http://gbnci.abcc.ncifcrf.gov/geo/GEOmetadb.sqlite.gz)
```

```
## Metadata associate with downloaded file:
##                 name               value
## 1     schema version                 1.0
## 2 creation timestamp 2014-01-25 18:19:52
```

```
## [1] "/Users/c8linmch/Documents/biost-578-bigDataOmics/hw1/GEOmetadb.sqlite"
```


I look at the names of the tables to identify which I want to use to find the HCV Illumina gene expression data.
This query finds the five of the ten gene expression samples from an investigator at Yale studying HCV. These five are the `case' samples, or those with HCV. The platform used is Illumina.


```r
geo_con <- dbConnect(SQLite(), "GEOmetadb.sqlite")
```

```
## Error: could not find function "dbConnect"
```

```r
rs <- dbGetQuery(geo_con, paste("select gsm.title,gsm.series_id,gpl.gpl,gpl.manufacturer,gpl.description", 
    "from gsm join gpl on gsm.gpl=gpl.gpl", "where gpl.manufacturer like 'Illumina%'", 
    "and gsm.extract_protocol_ch1 like '% HCV %'", "and gsm.title like '%HCV%'", 
    "and gsm.source_name_ch1='PBMC'"))
```

```
## Error: could not find function "dbGetQuery"
```

```r
dim(rs)
```

```
## Error: object 'rs' not found
```

```r
names(rs)
```

```
## Error: object 'rs' not found
```

```r
rs
```

```
## Error: object 'rs' not found
```


In order to perform the above query using the data.table package, I first convert all database tables from the GEO database to data.table objects using the `dbReadTable()' function.
I then join them and perform the query on the joined tables.


```r
gsmTable <- dbReadTable(geo_con, "gsm")
```

```
## Error: could not find function "dbReadTable"
```

```r
gsm <- data.table(gsmTable)
```

```
## Error: could not find function "data.table"
```

```r

gplTable <- dbReadTable(geo_con, "gpl")
```

```
## Error: could not find function "dbReadTable"
```

```r
gpl <- data.table(gplTable)
```

```
## Error: could not find function "data.table"
```

```r

dim(gsm)
```

```
## Error: object 'gsm' not found
```

```r
dim(gpl)
```

```
## Error: object 'gpl' not found
```

```r
head(gsm)
```

```
## Error: object 'gsm' not found
```

```r
head(gpl)
```

```
## Error: object 'gpl' not found
```

```r
setkey(gsm, "gpl")
```

```
## Error: could not find function "setkey"
```

```r
setkey(gpl, "gpl")
```

```
## Error: could not find function "setkey"
```

```r
gsmGpl <- merge(gsm, gpl, all = TRUE)
```

```
## Error: object 'gsm' not found
```

```r

rsDT <- gsmGpl[manufacturer %like% "Illumina" & extract_protocol_ch1 %like% 
    " HCV " & title.x %like% "HCV" & source_name_ch1 == "PBMC", list(title.x, 
    series_id, gpl, manufacturer, description.y)]
```

```
## Error: object 'gsmGpl' not found
```

```r
dim(rsDT)
```

```
## Error: object 'rsDT' not found
```

```r
names(rsDT)
```

```
## Error: object 'rsDT' not found
```

```r
rsDT
```

```
## Error: object 'rsDT' not found
```

