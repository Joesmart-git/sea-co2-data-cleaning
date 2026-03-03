# Data Cleaning & Transformation: Southeast Asia CO2 Emissions

[![View Visual Case Study on Notion](https://img.shields.io/badge/View_Visual_Case_Study-Notion-black?logo=notion)](https://whispering-crater-183.notion.site/Project-Data-Cleaning-Transformation-Southeast-Asia-CO2-Emissions-using-Power-Query-313e54702f0b80a98ab7c047868f19bb?source=copy_link) 

## Project Overview
Real-world data is rarely ready for immediate visualization. In this project, I processed and cleaned a highly unstructured raw dataset containing the carbon emissions of Southeast Asian countries. 

Before (Raw Data)
<img width="1752" height="552" alt="Before" src="https://github.com/user-attachments/assets/344fde58-173b-459d-93bb-82aa8c28c32e" />
After (Raw Data)
<img width="1756" height="561" alt="After" src="https://github.com/user-attachments/assets/f1191541-aa14-4db7-9dc7-9bd298a09aa4" />


Initial data profiling revealed severe structural errors, including unequal distinct values in geographical columns, mismatched string formats, and mathematical anomalies. Using Power Query, I systematically resolved these inconsistencies to build a reliable, structured data model ready for deep-dive analysis.

* **Tools Used:** Power Query, Power BI, Advanced M Code
* **Role:** Data Analyst

## The Challenge & Methodology
To prepare this data for accurate reporting, I executed an end-to-end data cleansing workflow:

* **Geographical Standardization & Deduplication:** Unified inconsistent naming conventions and removed 64 duplicate rows (reducing the dataset from 1,074 to 1,010 unique records) to prevent artificially inflated aggregates.
* **Complex Text-to-Number Conversions:** Extracted numerical values from dirty strings (e.g., converting "Year 1919" into 1919), successfully preserving over 70 rows of valuable emissions data that would have otherwise been lost to type-conversion errors..
* **Mathematical Anomaly Resolution:** Identified impossible negative values in both the population and GDP columns and corrected them using mathematical transformations.
* **Logical Data Imputation:** Systematically resolved null values in the categorical `income_group` column by referencing the `iso_code` to assign the correct category.

## Technical Implementation: Power Query (M Code)
Rather than relying purely on the graphical interface, custom M code steps were required to safely handle errors without losing valuable rows of data. 

### 1. Handling Negative Mathematical Anomalies
To correct logically invalid negative GDP values, I applied a Scientific Absolute Value transformation directly to the column:

```powerquery
= Table.TransformColumns(#"Sorted Rows", {{"gdp", Number.Abs, Int64.Type}})
```
### 2. Standardizing Text-to-Number Conversions
To standardize population data, varying text formats were systematically replaced with numerical equivalents before converting the entire column to a whole number type:
```
= Table.ReplaceValue(#"8.38 million to 8380000","267.6Million", 267600000, Replacer.ReplaceValue, {"population"})
= Table.ReplaceValue(#"267.6Million to 267600000", "264.6 Million", 264600000, Replacer.ReplaceValue, {"population"})
= Table.ReplaceValue(#"264.6 Million to 264600000", "118 thousand", 118000, Replacer.ReplaceValue, {"population"})
```

### 3. Safe Error Handling and Null Imputation
Rather than deleting rows with unreadable text strings during type conversion, I isolated the anomalies and explicitly replaced the errors with nulls or zeros to preserve the rest of the row's data:

```powerquery
= Table.ReplaceErrorValues(#"consumption_co2_per_capita > D.decimal", {{"consumption_co2_per_capita", null}})
```

## Repository Structure
* [(Dirty) Expanded SEA Dataset.xlsx]((Dirty)%20Expanded%20SEA%20Dataset.xlsx) - The raw, unstructured dataset.
* [Cleaning Dataset Using Power Query - APAN.pbix](Cleaning%20Dataset%20Using%20Power%20Query%20-%20APAN.pbix) - The Power BI file containing the complete Power Query applied steps.

## Let's Connect
If you're looking for a Data Analyst who can turn complex datasets into clear, actionable business insights, let's connect.

* **Email:** joesmartverdilloapan@gmail.com 
* **LinkedIn:** [Joesmart V. Apan](https://www.linkedin.com/in/joesmart-apan-7735ab215)

