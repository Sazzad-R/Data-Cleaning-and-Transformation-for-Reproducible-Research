# Data-Cleaning-and-Transformation-for-Reproducible-Research
## Objective
This project demonstrates an R script that performs essential data cleaning and transformation tasks using the tidyverse package.  
Focus areas:
- Identifying and fixing missing values, type errors, and inconsistent entries
- Using reproducible tidyverse workflows
- Saving cleaned data for future use

Needed package
library(tidyverse)
library(knitr)

## Workflow Steps
1. Create and inspect a messy sample dataset
2. Coerce character values in numeric columns
3. Parse and standardize diverse date formats
4. Standardize categorical entries
5. Handle out-of-range and missing values
6. Save the cleaned dataset as a CSV

#6. Final Inspection and Save the Cleaned Dataset
summary(messy_data5)
str(messy_data5)

##**Save the transformed dataset**:
if (!dir.exists("data_processed")) dir.create("data_processed")
write_csv(messy_data, "data_processed/cleaned_field_data.csv")

