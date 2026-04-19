##Overview 
Automated monthly performance KPI (OTIF Customer (Supply chain)) consumed by senior stakeholders for monitoring business performance. 

Previous visuals were generated on xls and power point. Aim to automate the extraction, transformation of the data, and creation of visuals to improve efficiency and reliavility of the reporting process. Time savings - 2h per month. 

 Extraction - automated data extraction, ingesting directly from AFO templates. 
 Trasnformation - adjusting types, hadling duplicates, pivoting columns, aligning of key dimensions etc
 Modeling - connecting reported data with anual targets, including a calendar table for date context. 
 Visuals - 
Type1. Global performance MTD overview with KPI + conditional formating, by grand total, networks, regions, segments, regional operational units and franchises. 
Type2. Line charts for MTD evolution. 
Tyoe3. YTD figures for OTIF value and OTIF Item. 
  
**Calendar table for date context*

Calendar Common =
DISTINCT (
    UNION (
        SELECTCOLUMNS ( 'OTIFc Grand Total',    "Calendar Month", 'OTIFc Grand Total'[Calendar Month] ),
        SELECTCOLUMNS ( 'OTIFc by Regions',     "Calendar Month", 'OTIFc by Regions'[Calendar Month] ),
        SELECTCOLUMNS ( 'OTIFc by ROPUs',       "Calendar Month", 'OTIFc by ROPUs'[Calendar Month] ),
        SELECTCOLUMNS ( 'OTIFc by Segments',    "Calendar Month", 'OTIFc by Segments'[Calendar Month] ),
        SELECTCOLUMNS ( 'OTIFc by Franchise',   "Calendar Month", 'OTIFc by Franchise'[Calendar Month] )
    )
)


Examples of **Measures** for the region ASIA.

--the KPI visual only accepts calculated values. The specific region is selected at the filter level--
OTIFc value Regions =  Average( 'OTIFc by Regions'[OTIF Customer Value] ) 

--the KPI visual only takes calculated values for targets--
OTIF Targets ASIA = AVERAGE ( 'OTIFc Yearly Targets'[Region] )

--Conditional formatting of given KPI--
OTIF Conditional AH Total = 
VAR v = [OTIF value Regions]
VAR t = [OTIF Targets ASIA]
RETURN
IF (
    v >= t,              "#4CAF50",        -- green (>= target)
    IF ( v >= t - 2,     "#FFC107",        -- orange (0 a -2)
                         "#F44336"         -- red (< -2)
    )
)

--YTD figure calculation. Then filtered by ASIA at the visual level--
OTIFc YTD Regions = DIVIDE(SUM('OTIFc by Region'[Value in Time and in Full]),SUM('OTIFc Grand Total'[Orders]),0)
