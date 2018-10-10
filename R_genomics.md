#Downloading data

## From GEO NCBI
library(GEOquery)
gg <- getGEOSuppFiles('GSE104333', fetch_files = FALSE)
