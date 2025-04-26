<<<<<<< HEAD
# Mercury Concentrations in Resident Lake Fish
=======
# 🐟 Mercury Concentrations in Resident Lake Fish from Lake Clark National Park and Preserve

---

## 📖 Introduction
Mercury contamination in aquatic ecosystems presents risks to wildlife and human health. This project explores mercury concentrations in resident lake fish from Lake Clark National Park and Preserve, Alaska. The goal is to identify spatial, temporal, and biological patterns that could inform conservation strategies and resource management decisions.

---

## 🎯 Scenario
You are a data analyst working with ecological researchers at the National Park Service. Your task is to analyze fish tissue mercury data to support risk assessment, public health advisories, and conservation efforts in Lake Clark National Park and Preserve.

---

## ❓ Ask

### Business Task
Understand spatial differences, temporal trends, and health implications of mercury (Hg) contamination in fish species across Lake Clark and Kijik Lake in LACL. Use this analysis to support ecological monitoring and inform potential public health decisions.

**Key Questions:**  
* How do mercury concentrations vary between different lakes and years?
* Is there a relationship between fish size (length/weight) and mercury concentration?
* Which species or environments present higher mercury risks?
* What recommendations can be made to park managers based on the findings?

---

## 📦 Prepare 
**Dataset:** Mercury Concentrations in Fish from Lake Clark National Park and Preserve (LACL) (2019 - 2020)  
**Source:** [Catalog.Data.Gov](https://catalog.data.gov/dataset/mercury-concentrations-in-resident-lake-fish-sampled-from-lake-clark-national-park-and-pre)

### Dataset Summary:
* 49 fish samples across two lakes (Lake Clark and Fishtrap Lake).
* Data collected over two years: 2019 and 2020.
* Key variables include:
   * Species
   * Length (mm)
   * Weight (g)
   * Mercury Concentration (ppm)
   * Location
   * Date of capture

---

## 🧹 Process

### Data Cleaning Steps:
* Imported and cleaned data.
* Standardized column names for consistency.
* Verified data types and handled missing values.
* Created exploratory data summaries (e.g., mean, median, distribution checks).
* Structured the data for visualization and modeling.

```r
# Load necessary libraries
library(tidyverse)
library(lubridate)

# Load dataset
SWAN_FW_Contaminants_Data_LACL_2019_2020 <- read_csv("SWAN_FW_Contaminants_Data_LACL_2019_2020.csv")

# Select and rename key variables
processed_data <- filtered_data %>%
  select(-Comments, -Methyl_Hg) %>%
  rename(
    total_hg = Total_Hg,
    length = Length,
    weight = Weight,
    age = Age
  )

# Filter out rows with missing values in key fields
processed_data <- processed_data %>%
  filter(!is.na(total_hg), !is.na(length), !is.na(weight), !is.na(age))

# Convert date column if applicable (if needed for temporal analysis)
processed_data$date <- mdy(processed_data$Date_Collected)

# Inspect cleaned data
summary(processed_data)

```

## 📊 Analyze
* Spatial Analysis: Compared mercury concentrations between Fishtrap Lake and Lake Clark.
* Temporal Analysis: Assessed year-to-year differences between 2019 and 2020 samples.
* Biological Analysis:
  * Correlated fish length and weight with mercury concentration.
  * Analyzed species-specific differences in mercury accumulation.
* Outlier Detection: Identified individual fish with exceptionally high mercury levels for further attention

```{r}
# Summary stats by lake
summary_stats_lake <- processed_data %>%
  group_by(Lake) %>%
  summarise(
    avg_mercury = mean(total_hg, na.rm = TRUE),
    max_mercury = max(total_hg, na.rm = TRUE),
    sample_count = n()
  )
summary_stats_lake

# Summary stats by species
summary_stats_species <- processed_data %>%
  group_by(Species) %>%
  summarise(
    avg_mercury = mean(total_hg, na.rm = TRUE),
    max_mercury = max(total_hg, na.rm = TRUE),
    sample_count = n()
  )
summary_stats_species
```

---

## 📤 Share
![Plot Mercury by Lake](https://github.com/ssagastume11/Mercury-Concentrations-in-Resident-Lake-Fish/blob/main/plot_mercury_by_lake.png)
```{r}
# Plot: Mercury concentration by lake
ggplot(processed_data, aes(x = Lake, y = total_hg)) +
  geom_boxplot(fill = "#69b3a2") +
  labs(title = "Mercury Concentration by Lake",
       x = "Lake",
       y = "Total Mercury (ppm)",
       caption = "Source: SWAN Mercury Monitoring Dataset, catalog.data.gov")
```
![Plot Mercury by Species]()
```{r}
# Plot: Mercury concentration by species
ggplot(processed_data, aes(x = Species, y = total_hg)) +
  geom_boxplot(fill = "#d95f02") +
  labs(title = "Mercury Concentration by Species",
       x = "Species",
       y = "Total Mercury (ppm)",
       caption = "Source: SWAN Mercury Monitoring Dataset, catalog.data.gov")
```

```{r}
# Optional: Relationship between mercury and weight
ggplot(processed_data, aes(x = weight, y = total_hg, color = Species)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE) +
  labs(title = "Mercury vs. Fish Weight by Species",
       x = "Weight (g)",
       y = "Total Mercury (ppm)",
       caption = "Source: SWAN Mercury Monitoring Dataset, catalog.data.gov")
```
