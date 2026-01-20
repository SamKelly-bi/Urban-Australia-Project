# Australan Urban Analysis - Final Report

## Executive Summary
After reviewing Resonance Consultancy’s [2025 Best Cities in the USA Report](https://www.worldsbestcities.com/rankings/americas-best-cities/), I was inspired to undertake a comparable analysis focused on Australian cities.

This study evaluates and ranks major Australian cities across a broad set of indicators, including **Median Household Income**, **Cultural Attractions**, and **Business Ecosystem strength**. Where possible, I adopted the same metrics, data sources, and methodological approach used by Resonance to ensure consistency and comparabile analytical performance.

The analysis combines **SQL-based data exploration**, an **Excel-driven z-score normalization framework**, and an **interactive Power BI dashboard** to present results in a clear, data-driven, and accessible format. Together, these tools provide a comprehensive view of each city’s relative performance across livability, lovability, and economic opportunity.

---

## Key Findings
**Sydney** and **Melbourne** emerged as the two top-performing cities. Their large populations, diverse economies, and wide range of cultural and lifestyle attractions enabled them to score highly across most metrics.

Many of the results aligned with common perceptions of each city. For example, Sydney offers higher average salaries but is also characterised by high rental costs, while the Gold Coast performs strongly in tourism-related measures yet lags in areas such as rental affordability. As expected, the methodology tended to favour larger cities, reflecting the broader range of services and opportunities typically available in major urban centres.

In future iterations of this study, I would like to incorporate public transport as a standalone metric, as it plays a significant role in overall urban livability. Overall, I am satisfied with the integrity of the analysis and believe it provides an accurate and balanced representation of living conditions across Australia’s eight largest cities.

---

## Data Sources
| Data Source | Description |
|------------|-------------|
| TripAdvisor | Contributed data for cultural attractions, tourism activity, and lifestyle metrics |
| WalkScore | Comprehensive walkability, transit, and bikeability data for major cities worldwide |
| PeopleForBikes | Global bike infrastructure and cycling accessibility data |
| IQAir | Real-time and historical air quality measurements for cities worldwide |
| Australian Bureau of Meteorology | Up-to-date climate and weather data across Australia |
| Australian Census | Official demographic and socioeconomic data compiled by the Australian Government |
| SGS Economics & Planning | Melbourne-based consultancy providing social, economic, and environmental insights |
| Australian Bureau of Statistics (ABS) | Comprehensive open datasets covering Australia’s economy, population, and society |
| FlightsFrom | Global flight route and airport connectivity data |
| QS World University Rankings | Annual rankings of top tertiary institutions worldwide |
| StartupBlink | Global city rankings based on startup ecosystems and entrepreneurial activity |


**Data Processing Steps**
- Imported raw datasets into Excel and performed data cleaning and normalization in preparation for importing into SQL
- Designed and created the database and table schema in SQL prior to importing data via the SQL Import Wizard
- Conducted reproducible query analysis using SQL views to highlight key insights and identify anomalies within the dataset
- Imported curated datasets into Power BI and integrated SQL findings to produce a high-level, interactive visual analysis

---

## Methodology

### Excel (Z-Score Normalization)
Z-scores measure how many standard deviations a given value is from the mean, allowing all metrics—regardless of scale or unit—to be compared on a consistent basis. This approach ensures each city is weighted accurately across all indicators.

**Z-Score Interpretation**
- Z = 0 → Exactly average
- Z > 0 → Above average
- Z < 0 → Below average
- Z = ±1 → One standard deviation from the mean
- Z = ±2 → Significantly above or below average (potential outlier)

**Formula**
Z = (Value − Mean) / Standard Deviation

---

### SQL
- Database structured into five core tables:
  - Cities  
  - Livability  
  - Lovability  
  - Prosperity  
  - City Z-Scores
- SQL views were prioritized to demonstrate analytical proficiency, surface key insights, and identify anomalies and performance outliers within the dataset
- Views enabled reproducible analysis and streamlined downstream reporting

---

### Power BI

#### Data Model
- **1 Fact Table**
  - Cities
- **7 Dimension Tables**
  - Livability lookup  
  - Livability z-scores  
  - Lovability lookup  
  - Lovability z-scores  
  - Prosperity lookup  
  - Prosperity z-scores  
  - Overall city z-scores

#### Visuals
- Z-score methodology overview and explanation
- KPI cards highlighting performance outliers and anomalies
- Clustered column chart showing category-level city z-score breakdowns
- Stacked bar chart illustrating overall city performance
- Metric-level cards with custom tooltips displaying:
  - Metric definition
  - Highest and lowest values
  - Underlying data source
- Matrix visuals showing each city’s rank across all evaluated metrics

---

## Key Results

- **Sydney** ranked as the top overall city, driven by strong performance in walkability, access to parks, cultural attractions, and business opportunities.
- **Melbourne** narrowly missed the top position, excelling in overall livability, restaurant density, and university rankings.
- **Brisbane** consistently performed above the dataset average across most metrics, securing a comfortable third-place finish.
- **Perth**, **Canberra**, **Adelaide**, and the **Gold Coast** produced near-identical overall scores, with Canberra’s result notably strengthened by high average wages.
- **Newcastle** finished in eighth place, trailing other cities largely due to scale limitations and lower performance relative to larger metropolitan regions.










