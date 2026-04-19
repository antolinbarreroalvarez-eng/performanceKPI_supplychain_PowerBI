OVERVIEW

Automated monthly performance KPI (OTIF Customer – Supply Chain) consumed by senior stakeholders to monitor global business performance.
Previously, visuals were manually created in Excel and PowerPoint.
This solution automates data extraction, transformation, modeling, and visualization, improving reliability and efficiency of the reporting process.
Estimated time savings: ~2 hours per month.

---


Data Pipeline

**Extraction**

Automated data extraction, ingesting directly from AFO templates.

**Transformation**

Data cleansing and preparation steps including data type alignment, duplicate handling, pivoting, harmonization of key dimensions, etc.

**Modeling**

Connecting reported data with annual targets, including a centralized calendar table to provide consistent **date context (MTD / YTD)**.

Calendar Common =
DISTINCT (
    UNION (
        SELECTCOLUMNS ( 'OTIFc AH Total',  "Calendar Month", 'OTIFc AH Total'[Calendar Month] ),
        SELECTCOLUMNS ( 'OTIFc Regions',   "Calendar Month", 'OTIFc Regions'[Calendar Month] ),
        SELECTCOLUMNS ( 'OTIFc ROPUs',     "Calendar Month", 'OTIFc ROPUs'[Calendar Month] ),
        SELECTCOLUMNS ( 'OTIFc Segments',  "Calendar Month", 'OTIFc Segments'[Calendar Month] ),
        SELECTCOLUMNS ( 'OTIFc Franchise', "Calendar Month", 'OTIFc Franchise'[Calendar Month] )
    )
)

---

**Visuals**

Type 1 – KPI Overview

combination of KPI visual showing Global MTD with conditional formatting and breakdown by:
Grand total
Networks
Regions
Segments
Regional Operational Units (ROPUs)
Franchises

Type 2 – Trend Analysis

Line charts showing monthly evolution, Item-based and value-based metrics.

Type 3 – YTD Figures

Aggregated YTD figures (items and value), Displayed using Card visuals.



---

**Measures** – Examples (Region: Asia)


1.KPI Value
Calculated for KPI visuals. The specific region is selected at visual filter level.

'''DAX 
value Regions =AVERAGE ( 'OTIFc Regions'[OTIF Customer Value] )Show more lines


2.Target Value
Used as KPI target benchmark.

DAXOTIF Target ASIA =AVERAGE ( 'OTIFc Yearly Targets'[Region] )Show more lines


Conditional Formatting

Defines KPI color based on performance vs target.

DAXOTIF Conditional AH Total =VAR v = [OTIFc value Regions]VAR t = [OTIF Target ASIA]RETURNIF (    v >= t, "#4CAF50",        -- green (>= target)    IF ( v >= t - 2, "#FFC107", "#F44336" ) -- orange / red)


YTD Calculation

Weighted YTD OTIF calculation. Region Asia selected at filter level.

OTIFc YTD Regions =
DIVIDE (
    SUM ( 'OTIFc Regions'[Value in Time and in Full] ),
    SUM ( 'OTIFc Regions'[Order PNS] ),
    0
)
