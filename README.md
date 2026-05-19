# GIS Portfolio

A collection of geospatial analyses, risk assessments, and cartographic work built with ArcGIS Pro, Hazus, and related tools. This portfolio documents how raw public-data sources (Census, FEMA, CDC, USGS) move through a reproducible workflow into finished maps that answer specific questions for decision-makers.

**Live site:** [peter-aina.github.io/gis-portfolio](https://peter-aina.github.io/gis-portfolio/)

---

## What this portfolio is

This is a curated record of geospatial work, not a feed of every map I have ever made. Each project on the site was selected because it shows something specific about how I think about spatial problems: how I scope a study area, choose a hazard model, defend a symbology decision, or read social vulnerability against physical risk. The maps are paired with the question they answered, the data behind them, and the takeaway, so that a reviewer can evaluate the work rather than just look at the pictures.

The portfolio currently centers on two multi-hazard risk assessments completed for INFO P-502 (Indiana University, Applied Data Science M.S.). Both projects use Hazus 7.0 as the modeling engine and ArcGIS Pro for cartographic finishing. As more work moves out of coursework and into deliverable form, additional projects will be added here.

---

## Why GIS, and why this kind of GIS

Geographic Information Systems are a category of software, but GIS as a practice is something narrower and more useful: the discipline of building defensible answers to questions that have a location. Where is the risk concentrated? Who lives there? What is the cost of a 100-year event versus a 500-year event? Which neighborhoods need outreach first?

These are not abstract questions. County emergency managers, public-health departments, infrastructure planners, and insurance underwriters need answers to them on a working timeline, and the answers have to be defensible enough that someone can write a check or evacuate a neighborhood based on them. A map that looks beautiful but cannot be reproduced or explained is not useful in that setting.

The work in this portfolio is built with that audience in mind. Symbology choices are deliberate, not aesthetic. Classification methods are documented. Data sources are traceable. When a number appears on a map, the table behind that number is reproducible.

---

## Tools and stack

The maps on this site were produced with a stack that reflects what is actually used in professional risk and emergency-management work, rather than what is trendy:

**Modeling and analysis**
- Hazus 7.0 (FEMA's standardized loss-estimation methodology) for hurricane wind, riverine flood, and other natural hazard scenarios
- ArcGIS Pro for spatial joins, clipping, classification, and cartographic layout
- QGIS as a secondary tool where licensing or workflow makes it the right choice

**Data sources**
- FEMA Hazus default datasets, National Risk Index, and the Resilience Analysis and Planning Tool
- CDC Social Vulnerability Index and CDC WONDER
- U.S. Census Bureau TIGER/Line boundaries and the American Community Survey
- USGS 3D Elevation Program (3DEP) and the National Hydrography Dataset
- HURREVAC for historical storm tracks

**Supporting infrastructure**
- Python (pandas, geopandas, arcpy) for data preparation and batch processing
- SQL for querying Hazus output tables and joining to spatial layers
- Git and GitHub for version control and hosting

---

## Projects in this portfolio

### Hurricane Michael damage and loss assessment, Florida Panhandle

A Hazus 7.0 hurricane wind analysis for Gadsden, Leon, and Wakulla counties (1,967 sq mi, population 381,645) following the Category 5 landfall of Hurricane Michael near Mexico Beach on October 10, 2018. The analysis covers hazard inputs (wind speed, peak gust, storm track, terrain), physical damage (building counts by occupancy and construction type), economic loss (~$32.7M across all three counties), debris generation (~196,445 tons, over 99% from trees), and a comparison of CDC SVI and FEMA Community Resilience Challenges Index for the three counties.

Gadsden County absorbed 56% of total economic loss despite representing only 11.7% of the area's population, with residential buildings accounting for over 90% of damage. The county also had the highest social vulnerability (SVI 0.9545) and the highest community-resilience challenges (90th percentile), which is the kind of overlap that should drive crisis-management priorities.

### Hamilton County flood risk assessment, Central Indiana

A Hazus 7.0 riverine flood analysis for Hamilton County, Indiana (402 sq mi, population 387,036) using five return periods (10, 25, 50, 100, and 500-year) plus Average Annualized Loss. The analysis covers the terrain and transportation context of the Upper White River Watershed, the count of substantially damaged buildings under each scenario (4 to 20 buildings), total economic loss by return period ($163.11M to $343.74M), and an AAL of $20.45M as a single-number summary of long-term annual flood cost.

The maps share consistent classification breaks across return periods (set from the 500-year scenario) so they can be read directly against one another. The risk pattern concentrates along the White River corridor through Noblesville, Carmel, and Fishers, and along Fall Creek. Social vulnerability is generally low across the county (SVI 0.032), but a few tracts in Hortonville and Westfield show notably higher vulnerability, and most of those tracts do not overlap with the highest physical-risk areas, which has implications for how outreach resources should be targeted.

---

## How the maps were made

The general workflow is consistent across both projects:

1. **Define the study area** in Hazus 7.0 at the county level. Hazus compiles a default building inventory from Census TIGER/Line boundaries, Oak Ridge National Laboratory building stock models, and American Community Survey demographics.

2. **Configure the hazard scenario**. For hurricane work, the storm track is loaded from HURREVAC through the Hazus Create Scenario window. For flood work, depth grids for each return period are imported and assigned to their matching scenarios, with the Average Annualized Loss option enabled.

3. **Run the analysis** with Hazus default parameters. Output layers cover hazard intensity, damage state counts, economic loss by occupancy class, debris, and other derived quantities.

4. **Finalize cartography in ArcGIS Pro**. This is where most of the design judgment lives. Symbology uses manual interval classification (typically four to five classes) rather than automated breaks, so the legend is meaningful rather than statistically convenient. Where multiple maps need to be read against each other (return-period series, for example), the highest-magnitude scenario is symbolized first and its breakpoints applied to every lower scenario. Legend labels are cleaned to remove underscores and include units.

5. **Layer in social context**. CDC SVI tract data is clipped to the study area and symbolized with manual classes that match the CDC reference scheme. FEMA NRI and CRCI values are collected manually from the FEMA web map for the relevant counties.

---

## Cartographic principles

A few decisions show up across every map in this portfolio:

**One primary variable per map.** A map that tries to show three things shows none of them. Supporting context (roads, water, county boundaries) is kept light so the primary variable is unambiguous.

**Classification is a content decision, not a default.** Automated breaks (Jenks, quantile, equal interval applied blindly) frequently produce legends that are statistically optimal but communicate nothing. Manual classification, applied consistently across a series, lets the reader compare scenarios directly.

**Comparable series share legends.** When five return-period maps appear side by side, all five use the same breakpoints. The reader's eye does the comparison; the cartography stays out of the way.

**Accessibility before flourish.** Color schemes work in grayscale and for the most common forms of color blindness. No three-color rainbow ramps. No pure red-on-green.

**Numbers are auditable.** If a map shows $32.7M in economic loss, the Hazus tables that produced that number are documented in the project methodology. Numbers that cannot be traced do not appear on the maps.

---

## Repository structure

```
gis-portfolio/
├── index.html              # The portfolio site itself
├── images/                 # All map images and the profile photo
│   ├── economy.jpg         # Hurricane Michael economic loss (hero)
│   ├── Flood_SVI.jpg       # Hamilton County flood + SVI (hero)
│   ├── E10.jpg ... E500.jpg     # Hamilton economic loss by return period
│   ├── B10.jpg ... B500.jpg     # Hamilton substantially damaged buildings by return period
│   ├── AAL.jpg                  # Hamilton Average Annualized Loss
│   ├── WindSpeed1.jpg, PeakGust3.jpg, ST4_1.jpg, Terrain.jpg   # Michael hazard inputs
│   ├── damaged.jpg              # Michael damaged building counts
│   ├── Building_debris.jpg, Tree_debris.jpg, Combined_debris.jpg, Utility.jpg
│   ├── Elevation_hamiliton.jpg, Ham_Road.jpg, SVI_Full_Flood.jpg
│   └── profile_photo.png        # About-section photo
└── README.md               # This file
```

The site is a single static HTML file. There is no build step, no framework, and no JavaScript dependencies beyond a small lightbox script that lives in the same file. The lightbox lets visitors click any thumbnail to view it at full resolution.

The map images are exported from ArcGIS Pro at 300 DPI and saved as JPEGs at quality 85, which keeps each file under about 500 KB while preserving the legibility of small symbology elements.

---

## Limitations of the work

Anyone using these maps to make a decision should know what they cannot tell you:

**Hazus default inventory** comes from Census-derived building stock models. In counties with recent rapid growth (Hamilton County is a good example), the inventory may underrepresent new construction and therefore underestimate exposure. A field-validated local building inventory would produce more accurate loss estimates.

**Hazus hurricane modeling considers wind only.** Storm surge, hurricane-induced flooding, and tornado damage are not captured in the loss totals shown for Hurricane Michael. Actual losses were higher than the wind-only model reports.

**Hazus does not produce flood debris estimates.** The debris estimates in the Hurricane Michael analysis come from the wind module. Hamilton County flood debris must be projected from external sources.

**Social vulnerability data is tract-level.** A census tract that contains both a wealthy neighborhood and a low-income mobile home park gets a single averaged SVI score. The averaging can mask pockets of high vulnerability inside otherwise resilient tracts. Block-group or block-level analysis is more granular when the data is available.

**Return-period assumptions are stationary.** The AAL calculation assumes a 100-year flood today is the same as a 100-year flood thirty years from now. Climate non-stationarity and ongoing land-use change in the watershed make this assumption increasingly questionable.

These limitations are worth stating up front because the alternative (presenting modeled numbers as ground truth) is what gives quantitative risk assessment a bad reputation. The maps are best read as defensible estimates under documented assumptions, not as predictions.

---

## About me

I am a graduate student in Applied Data Science at Indiana University (M.S. expected May 2026) and a Data and Geospatial Engineering Intern at The POLIS Center at the IU Indianapolis Luddy School. My work sits at the intersection of geospatial analysis, hazard and risk modeling, and data engineering. Before graduate school I spent several years in fintech analytics, including fraud and risk work at Kraken and Wise Payments, which shaped how I think about defensibility and reproducibility in any analytical pipeline.

I am interested in geospatial collaborations, hazard and resilience work, and conversations about data infrastructure for public-interest analysis.

**Contact**
- Email: ainapeter.o@gmail.com
- LinkedIn: [linkedin.com/in/peteraina](https://www.linkedin.com/in/peteraina/)
- GitHub: [github.com/peter-aina](https://github.com/peter-aina)
- Location: Indianapolis, Indiana

---

## License and attribution

All maps and analyses in this portfolio are produced by the author unless otherwise credited. Source datasets (Census, FEMA, CDC, USGS) are public-domain or openly licensed by their producing agencies and remain subject to those terms. The portfolio site code in this repository is provided as-is for reference; if it is useful to you as a starting point for your own portfolio, you are welcome to adapt it.
