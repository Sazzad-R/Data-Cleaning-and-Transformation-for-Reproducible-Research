# Data-Cleaning-and-Transformation-for-Reproducible-Research
## Objective
This project demonstrates an R script that performs essential data cleaning and transformation tasks using the tidyverse package.  
Focus areas:
- Identifying and fixing missing values, type errors, and inconsistent entries
- Using reproducible tidyverse workflows
- Saving cleaned data for future use

# Load tidyverse for data manipulation
library(tidyverse)
library(knitr)

## Workflow Steps
#1. Create and inspect a messy sample dataset
messy_data <- tibble(
ID = 1:7,
ObservationDate = c("2023-01-15", "Jan 20, 2023", "01/25/2025", "2023-02-05", "missing",
"2023-02-15", "03/01/2023"),
Temperature_C = c("15.2", "18.0", "14.5", "invalid", "17.3", "20.1", NA),
Humidity_Percent = c(70, -72, NA, 65, 780, 80, 71),
Site = c("North", "nirth", "South", "East", "west", "NORTH", "Souther")
)
#Inspect the dataset
summary(messy_data)
str(messy_data)

#2. Handle Characters in Numeric Columns
messy_data%>%
mutate(Temperature_C= parse_number(Temperature_C)
)

#3. Correct Date Column Issues
messy_data%>%
mutate(
ObservationDate= case_when(
str_detect(ObservationDate,"^[0-9]{4}-")~ymd(ObservationDate),
str_detect(ObservationDate, "^[A-Za-z]") ~mdy(ObservationDate),
str_detect(ObservationDate, "/") ~ mdy(ObservationDate),
TRUE ~ NA
)
)

#4. Standardize Categorical Data (Inconsistent Entries)
messy_data4<- messy_data3%>%
mutate(
Site=str_to_lower(Site),
Site=case_when(
Site%in% c("north","nirth") ~ "North",
Site%in% c("south","souther") ~ "South",
Site== "east"~ "East",
Site== "west" ~ "West",
TRUE~ NA
)
)

#5. Address Out-of-Range Humidity Values
messy_data5<- messy_data4 %>%
mutate(
Humidity_Percent= if_else(
Humidity_Percent>=0 & Humidity_Percent<=100, Humidity_Percent, NA_real_
)
)

#6. Final Inspection and Save the Cleaned Dataset
summary(messy_data5)
str(messy_data5)

##**Save the transformed dataset**:
if (!dir.exists("data_processed")) dir.create("data_processed")
write_csv(messy_data, "data_processed/cleaned_field_data.csv")

