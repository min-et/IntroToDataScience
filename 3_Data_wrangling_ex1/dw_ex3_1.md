---
title: "Data_Wrangling_ex1"
author: "Cesar Escobedo"
date: "April 10, 2018"
output: md_document
---



```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
library(dplyr)
```



## R Markdown

```{r}
refine_df <- read.csv(file="C:/Users/min_e/OneDrive/Documents/Springboard/Intro to Data Science/3_Data_wrangling/refine_original.csv", header=TRUE, sep=",")
```


```{r}
# Clean up brand names
refine_df$company <- sapply(refine_df$company, tolower)
refine_df$company[grepl("ps", refine_df$company)] <- "philips"
refine_df$company[grepl("ak", refine_df$company)] <- "akzo"
refine_df$company[grepl("van", refine_df$company)] <- "van houten"
refine_df$company[grepl("uni", refine_df$company)] <- "unilever"

# Separate product code and number
names(refine_df)[2] <- "ProductNameNumber"
refine_df <- mutate(refine_df, ProductName = gsub("-.*", "", refine_df$ProductNameNumber))
refine_df <- mutate(refine_df, ProductNumber = gsub(".*-", "", refine_df$ProductNameNumber))

# Add product categories
refine_df = within(refine_df, {
  ProductCategory = ifelse(ProductName == "p", "Smartphone", ifelse(ProductName == "v", "TV", ifelse(ProductName == "x" , "Laptop", ifelse(ProductName == "q" , "Tablet", "Not in list")))) 
    })

# Add full address for geocoding
refine_df$full_address <- paste(refine_df$address,",",refine_df$city,",",refine_df$country)

# Create dummy variables for company and product category
refine_df$company_philips <- 1
refine_df$company_akzo <- 0
refine_df$company_van_houten <- 1
refine_df$company_unilever <- 0

# Write refine_df to CSV file
write.csv(refine_df, file = "C:/Users/min_e/OneDrive/Documents/Springboard/Intro to Data Science/3_Data_wrangling/refine_clean.csv",row.names=FALSE)
```
